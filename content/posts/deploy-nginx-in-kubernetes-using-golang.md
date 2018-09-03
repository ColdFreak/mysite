---
title: "Golangを使って、KubernetesにNginxをデプロイ"
date: 2018-07-03T17:47:37+09:00
draft: false
---

### 概要

GolangでKubernetesのAPIを色々試しているので、NginxをKubernetesにデプロイするのもやってみました。

{{< highlight golang >}}

package main

import (
    // "bufio"
    "flag"
    "fmt"
    // "os"
    "path/filepath"

    appsv1 "k8s.io/api/apps/v1"
    apiv1 "k8s.io/api/core/v1"
    metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/tools/clientcmd"
    "k8s.io/client-go/util/homedir"
    // "k8s.io/client-go/util/retry"
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

    // https://godoc.org/k8s.io/api/apps/v1#Deployment
    deployment := &appsv1.Deployment{
        // https://godoc.org/k8s.io/apimachinery/pkg/apis/meta/v1#ObjectMeta
        ObjectMeta: metav1.ObjectMeta{
            Name: "demo-deployment",
        },
        // https://godoc.org/k8s.io/api/apps/v1#DeploymentSpec
        Spec: appsv1.DeploymentSpec{
            Replicas: int32Ptr(2),
            Selector: &metav1.LabelSelector{
                MatchLabels: map[string]string{
                    "app": "demo",
                },
            },
            Template: apiv1.PodTemplateSpec{
                ObjectMeta: metav1.ObjectMeta{

                    Labels: map[string]string{
                        "app": "demo",
                    },
                },
                Spec: apiv1.PodSpec{
                    Containers: []apiv1.Container{
                        {
                            Name:  "web",
                            Image: "nginx:1.12",
                            Ports: []apiv1.ContainerPort{
                                {
                                    Name:          "http",
                                    Protocol:      apiv1.ProtocolTCP,
                                    ContainerPort: 80,
                                },
                            },
                        },
                    },
                },
            },
        },
    }

    fmt.Println("Creating deployment...")
    // https://godoc.org/k8s.io/client-go/kubernetes/typed/apps/v1#DeploymentInterface
    _, err = deploymentClient.Create(deployment)

    if err != nil {
        panic(err)
    }

}

func int32Ptr(i int32) *int32 { return &i }
{{< / highlight >}}



{{<  highlight bash >}}
# go run deploy-nginx.go
Creating deployment...

# kubectl get pod -n default
NAME                                     READY     STATUS    RESTARTS   AGE
demo-deployment-857588f755-hjwbp         1/1       Running   0          19m
demo-deployment-857588f755-hzxjt         1/1       Running   0          19m

# kubectl get deployment -n default
NAME                     DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
demo-deployment          2         2         2            2           20m
{{< / highlight >}}


意外と上手く行った。

