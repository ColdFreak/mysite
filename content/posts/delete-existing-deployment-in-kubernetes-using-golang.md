---
title: "Kubernetesに既存のDeploymentをGolangで削除"
date: 2018-07-04T15:31:07+09:00
draft: false
---

Kubernetesの`Deployment`を削除するときに`DeletionPropagation`を設定する必要があるようです。
ソースコードにDeletionPropagationの説明は

> DeletionPropagation decides if a deletion will propagate to the dependents of the object, and how the garbage collector will handle the propagation.

`DeletionPropagation`は`Foreground`の場合

> The object exists in the key-value store until the garbage collector deletes all the dependents whose ownerReference.blockOwnerDeletion=true from the key-value store.  API sever will put the "foregroundDeletion" finalizer on the object, and sets its deletionTimestamp.  This policy is cascading, i.e., the dependents will be deleted with Foreground.

{{< highlight golang >}}
package main

import (
    "flag"
    "fmt"
    "path/filepath"

    apiv1 "k8s.io/api/core/v1"
    metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/tools/clientcmd"
    "k8s.io/client-go/util/homedir"
)

func main() {
    var kubeconfig *string
    home := homedir.HomeDir()
    kubeconfig = flag.String("kubeconfig", filepath.Join(home, ".kube", "config"), "(optional) absolute path to the kubeconfig file")
    flag.Parse()

    config, err := clientcmd.BuildConfigFromFlags("", *kubeconfig)
    if err != nil {
        panic(err)
    }

    clientset, err := kubernetes.NewForConfig(config)
    if err != nil {
        panic(err)
    }

    // https://godoc.org/k8s.io/client-go/kubernetes/typed/apps/v1#AppsV1Client.Deployments
    // clientset.AppsV1: func() v1.AppsV1Interface
    // .Deployments: func(namespace string) v1.DeploymentInterface
    deploymentClient := clientset.AppsV1().Deployments(apiv1.NamespaceDefault)

    fmt.Println("Deleting deployment...")
    
    // https://github.com/kubernetes/apimachinery/blob/bea47ba1bc0ea54dd3b057916e32338dbeb79011/pkg/apis/meta/v1/types.go
    deletePolicy := metav1.DeletePropagationForeground

    if err := deploymentClient.Delete("demo-deployment", &metav1.DeleteOptions{
        PropagationPolicy: &deletePolicy,
    }); err != nil {
        panic(err)
    }

    fmt.Println("Deleted deployment.")
}
{{< / highlight >}}

実行

{{<  highlight bash >}}
# go run delete-deployment.go
Deleting deployment...
Deleted deployment.
{{< / highlight >}}
