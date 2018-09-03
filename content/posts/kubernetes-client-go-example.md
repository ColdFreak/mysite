---
title: "client-goでKubernetesのAPIを試す"
date: 2018-07-03T13:00:43+09:00
draft: false
---

### 概要

Kubernetesの`client-go`クライアントを利用して、クラスターへの操作を色々試す。

#### Nodeの取得

{{< highlight golang >}}
package main

import (
    "fmt"
    metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/tools/clientcmd"
    "log"
    "os"
    "path/filepath"
)

func main() {
    // kubernetesの設定ファイルのパスを組み立てる
    kubeconfig := filepath.Join(os.Getenv("HOME"), ".kube", "config")
    
    // BuildConfigFromFlags is a helper function that builds configs from a master url or 
    // a kubeconfig filepath.
    config, err := clientcmd.BuildConfigFromFlags("", kubeconfig)
    if err != nil {
        log.Fatal(err)
    }

    // NewForConfig creates a new Clientset for the given config.
    // https://godoc.org/k8s.io/client-go/kubernetes#NewForConfig
    clientset, err := kubernetes.NewForConfig(config)
    if err != nil {
        log.Fatal(err)
    }

    // https://godoc.org/k8s.io/client-go/kubernetes/typed/core/v1
    nodes, err := clientset.CoreV1().Nodes().List(metav1.ListOptions{})
    if err != nil {
        log.Fatalln("failed to get nodes:", err)
    }

    for i, node := range nodes.Items {
        fmt.Printf("[%d] %s\n", i, node.GetName())
    }
}

{{< / highlight >}}


結果：


{{<  highlight bash >}}

$ go run nodelist.go
[0] kube-01
[1] kube-02
[2] kube-03
[3] kube-04
{{<  / highlight >}}


#### Podの取得

全てのnamespaceの中のPodを取得している。

{{< highlight golang >}}
// https://godoc.org/k8s.io/client-go
package main

import (
        "fmt"
        metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
        "k8s.io/client-go/kubernetes"
        "k8s.io/client-go/tools/clientcmd"
        "log"
        "os"
        "path/filepath"
)

func main() {
        kubeconfig := filepath.Join(os.Getenv("HOME"), ".kube", "config")
        config, err := clientcmd.BuildConfigFromFlags("", kubeconfig)
        if err != nil {
                log.Fatal(err)
        }

        // NewForConfig creates a new Clientset for the given config.
        // https://godoc.org/k8s.io/client-go/kubernetes#NewForConfig
        clientset, err := kubernetes.NewForConfig(config)
        if err != nil {
                log.Fatal(err)
        }

        // We need to create a serializer client to let us access API objects.
        // Type Clientset, from package kubernetes, provides access to generated serializer clients
        // to access versioned API objects
        // https://godoc.org/k8s.io/client-go/kubernetes#Clientset.CoreV1
        pods, err := clientset.CoreV1().Pods("").List(metav1.ListOptions{})
        if err != nil {
                log.Fatalln("failed to get pods:", err)
        }

        // print pods
        // pods.Items: []v1.Pod
        for i, pod := range pods.Items {
                fmt.Printf("[%d] %s\n", i, pod.GetName())
        }
}

{{< / highlight >}}

結果：

{{<  highlight bash >}}
# go run podlist.go
[0] nfs-client-provisioner-65f4b4b45-9k99p
[1] redis-65857fdb8c-w6qjg
[2] api-6dcdd79ff5-ftwsl
[3] database-7d6b5ddd9d-pwrbt
[4] memcached-867fc645f9-vmlqz
...
...
{{<  / highlight >}}



#### Serviceの取得

同じようにKubernetesの全てのNamespaceのServiceも下のように取得できる


{{<  highlight golang >}}
    services, err := clientset.CoreV1().Services("").List(metav1.ListOptions{})
    if err != nil {
        log.Fatalln("failed to get services:", err)
    }
    for i, svc := range services.Items {
        fmt.Printf("[%d] %s\n", i, svc.GetName())
    }
{{<  / highlight >}}

#### Service Accountの取得

{{<  highlight golang >}}
    serviceAccounts, err := clientset.CoreV1().ServiceAccounts("").List(metav1.ListOptions{})
    if err != nil {
        log.Fatalln("failed to get service accounts:", err)
    }
    for i, sa := range serviceAccounts.Items {
        fmt.Printf("[%d] %s\n", i, sa.GetName())
    }
{{<  / highlight >}}

#### Deploymentの取得

{{<  highlight golang >}}
    // https://godoc.org/k8s.io/client-go/kubernetes/typed/apps/v1#AppsV1Interface
    deployments, err := clientset.AppsV1().Deployments("").List(metav1.ListOptions{})
    if err != nil {
        log.Fatalln("failed to get deployments:", err)
    }
    for i, deployment := range deployments.Items {
        fmt.Printf("[%d] %s\n", i, deployment.GetName())
    }
{{<  / highlight >}}


#### Persistent Volumeの取得

{{<  highlight golang >}}
    pvs, err := clientset.CoreV1().PersistentVolumes().List(metav1.ListOptions{})
    if err != nil {
        log.Fatalln("failed to get persistent volumes:", err)
    }
    for i, pv := range pvs.Items {
        fmt.Printf("[%d] %s\n", i, pv.GetName())
    }
{{<  / highlight >}}


#### Persistent Volume Claimの取得

{{<  highlight golang >}}
    pvcs, err := clientset.CoreV1().PersistentVolumeClaims("").List(metav1.ListOptions{})
    if err != nil {
        log.Fatalln("failed to get persistent volume claim:", err)
    }
    for i, pvc := range pvcs.Items {
        fmt.Printf("[%d] %s\n", i, pvc.GetName())
    }
{{<  / highlight >}}


#### Namespaceの取得

{{< highlight golang >}}
    namespaces, err := clientset.CoreV1().Namespaces().List(metav1.ListOptions{})
    if err != nil {
        log.Fatalln("failed to get name space:", err)
    }
    for i, ns := range namespaces.Items {
        fmt.Printf("[%d] %s\n", i, ns.GetName())
    }
{{< / highlight >}}


#### Ingressの取得

{{< highlight golang >}}
    // https://godoc.org/k8s.io/client-go/kubernetes/typed/extensions/v1beta1#IngressesGetter
    // https://godoc.org/k8s.io/client-go/kubernetes/typed/extensions/v1beta1
    ingresses, err := clientset.ExtensionsV1beta1().Ingresses("").List(metav1.ListOptions{})
    if err != nil {
        log.Fatalln("failed to get ingress:", err)
    }
    for i, ingress := range ingresses.Items {
        fmt.Printf("[%d] %s\n", i, ingress.GetName())
    }
{{< / highlight >}}

#### Secretの取得

{{< highlight golang >}}
    secrets, err := clientset.CoreV1().Secrets("").List(metav1.ListOptions{})
    if err != nil {
        log.Fatalln("failed to get secret:", err)
    }
    for i, secret := range secrets.Items {
        fmt.Printf("[%d] %s\n", i, secret.GetName())
    }
{{< / highlight >}}

#### Secretの作成

{{< highlight golang >}}
    data := make(map[string][]byte)
    data["user"] = []byte("admin")
    data["password"] = []byte("password")

    // https://godoc.org/k8s.io/api/core/v1
    // https://godoc.org/k8s.io/client-go/kubernetes/typed/core/v1#SecretInterface
    secrets, err := clientset.CoreV1().Secrets("default").Create(&v1.Secret{
            TypeMeta: metav1.TypeMeta{
                    Kind: "Secret",
            },
            ObjectMeta: metav1.ObjectMeta{
                    Name: "generic-secret",
                    Namespace: "default",
            },
            Data: data,
    })
    fmt.Println(secrets)

{{< / highlight >}}


#### Config Mapの取得

{{< highlight golang >}}
    configMaps, err := clientset.CoreV1().ConfigMaps("").List(metav1.ListOptions{})
    if err != nil {
        log.Fatalln("failed to get config map:", err)
    }
    for i, cm := range configMaps.Items {
        fmt.Printf("[%d] %s\n", i, cm.GetName())
    }
{{< / highlight >}}

### Ingressの詳細を取得

{{< highlight golang >}}
    ingress, err := clientset.ExtensionsV1beta1().Ingresses("jx").Get("docker-registry", metav1.GetOptions{})
    if err != nil {
        log.Fatalln("failed to get ingresses:", err)
    }

    fmt.Println(reflect.TypeOf(ingress)) // *v1beta1.Ingress
    fmt.Println(ingress) 
    fmt.Println(ingress.ObjectMeta.Name) // docker-registry
{{< / highlight >}}

