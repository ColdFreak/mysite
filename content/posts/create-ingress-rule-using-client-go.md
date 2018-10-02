---
title: "client-goでKubernetesのIngress Ruleを作る"
date: 2018-08-02T14:54:47+09:00
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "programming"
tags:
  - "kubernetes"
  - "client-go"
---


JenkinsXのコマンドを検証しているうちに、うまく動作しない部分もあって、どうやってプログラムで、Ingress Ruleを作ること気になったので、ちょっとやってみました。

`client-go`ライブラリを利用して、以下のようなKubernetesのIngress Ruleを作成します。


```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-rule-created-from-go
  namespace: default
spec:
  backend:
    serviceName: nginx
    servicePort: 80
  rules:
  - host: nginx.my.domain
```

上の情報に基づいて、Ingress Ruleを作成するのに以下のの情報を指定すると作成できます。

- apiVersion
- kind
- metadata
- spec.backend.serviceName
- spec.backend.servicePort
- rules.host

Ingress Ruleを作るのにGoのライブラリのドキュメントはかなり役に立っています。ドキュメントを参照して、必要なものを正しい形で指定すれば良いです。


{{< highlight golang>}}
package main

import (
        "fmt"
        v1beta1 "k8s.io/api/extensions/v1beta1"
        metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
        "k8s.io/apimachinery/pkg/util/intstr"
        "k8s.io/client-go/kubernetes"
        "k8s.io/client-go/tools/clientcmd"
        "log"
        "os"
        "path/filepath"
        "reflect"
)

func main() {
        // kubernetesの設定ファイルのパスを組み立てる
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
        // https://godoc.org/k8s.io/api/extensions/v1beta1#Ingress
        ingress, err := clientset.ExtensionsV1beta1().Ingresses("default").Create(&v1beta1.Ingress{
                // https://godoc.org/k8s.io/apimachinery/pkg/apis/meta/v1#TypeMeta
                TypeMeta: metav1.TypeMeta{
                        Kind: "Ingress",
                },
                // https://godoc.org/k8s.io/apimachinery/pkg/apis/meta/v1#ObjectMeta
                ObjectMeta: metav1.ObjectMeta{
                        Name:      "ingress-rule-created-from-go",
                        Namespace: "default",
                },
                // https://godoc.org/k8s.io/api/extensions/v1beta1#IngressSpec
                Spec: v1beta1.IngressSpec{
                        // https://godoc.org/k8s.io/api/extensions/v1beta1#IngressBackend
                        Backend: &v1beta1.IngressBackend{
                                ServiceName: "nginx",
                                // https://godoc.org/k8s.io/apimachinery/pkg/util/intstr#IntOrString
                                ServicePort: intstr.FromInt(80),
                        },
                        Rules: []v1beta1.IngressRule{{Host: "nginx.my.domain"}},
                },
        })
        if err != nil {
                log.Fatalln("failed to create ingresses:", err)
        }

        fmt.Println(ingress)

}
{{< / highlight >}}

実行

```
go run create-ingress.go
```

`kubectl describe ingress -n default ingress-rule-created-from-go`コマンドでチェックしてみるといいです。


もっとも気を使うところは`v1beta1.Ingress{}`の中身です。ソースコードの中で参照ページをリンクしています。一個一個揃っておけば良いです。


Ingressを作成した後にNginxのサービスを作って、本当にアクセスできるかを検証してみます。


```
kubectl run nginx --image=nginx:1.11 -n default
kubectl expose deployment -n default nginx --port 80
```

`nginx.my.domain`ドメインにアクセスすると、Nginxの画面が出てきます。
