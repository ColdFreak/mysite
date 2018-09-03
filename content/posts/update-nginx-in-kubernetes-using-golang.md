---
title: "Kubernetesに既存のDeploymentをGolangでUpdate"
date: 2018-07-06T15:22:50+09:00
draft: false
---


Kubernetesの `client-go-test`ネームスペースに既存の`demo-deployment`という名前のDeploymentはNginx 1.12バージョンを使っているが、それをNginx 1.13にUpdateする。

{{< highlight golang >}}
package main

import (
        "flag"
        "fmt"
        "path/filepath"

        // apiv1 "k8s.io/api/core/v1"
        metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
        "k8s.io/client-go/kubernetes"
        "k8s.io/client-go/tools/clientcmd"
        "k8s.io/client-go/util/homedir"
        "k8s.io/client-go/util/retry"
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
        deploymentClient := clientset.AppsV1().Deployments("client-go-test")

        // https://godoc.org/k8s.io/client-go/util/retry#RetryOnConflict
        // RetryConflict executes the provided function repeatedly,
        // retrying if the server returns a conflicting write.
        // Callers should preserve previous executions if they wish to retry changes.
        // It performs an exponential backoff.

        // retry.RetryOnConflict: func(backoff wait.Backoff, fn func() error) error
        retryErr := retry.RetryOnConflict(retry.DefaultRetry, func() error {
                result, getErr := deploymentClient.Get("demo-deployment", metav1.GetOptions{})
                if getErr != nil {
                        panic(fmt.Errorf("Failed to get latest version of Deployment: %v", getErr))
                }

                result.Spec.Replicas = int32Ptr(1)
                result.Spec.Template.Spec.Containers[0].Image = "nginx:1.13"
                _, updateErr := deploymentClient.Update(result)
                return updateErr
        })

        if retryErr != nil {
                panic(fmt.Errorf("Update failed: %v", retryErr))
        }
        fmt.Println("Updated deployment...")
}

func int32Ptr(i int32) *int32 { return &i }
{{< / highlight >}}
