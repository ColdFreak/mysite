---
title: "KubernetesのServiceを監視するコントローラ"
date: 2018-07-13T21:15:13+09:00
draft: false
---


やりたいことは`client-go`というオフィシャルのKubernetesのクライアントを利用して、KubernetesのServiceの作成、削除、更新を監視することです。

ファイル名: `watch_service.go`

{{<  highlight golang >}}
package main

import (
    "flag"
    "fmt"
    "github.com/golang/glog"
    "k8s.io/api/core/v1"
    "k8s.io/apimachinery/pkg/fields"
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/tools/cache"
    "k8s.io/client-go/tools/clientcmd"
    "k8s.io/client-go/util/homedir"
    "path/filepath"
    "time"
)

func main() {
    var kubeconfig *string
    home := homedir.HomeDir()
    kubeconfig = flag.String("kubeconfig", filepath.Join(home, ".kube", "config"), "optional) absolute path to the kubeconfig file")

    flag.Parse()

    config, err := clientcmd.BuildConfigFromFlags("", *kubeconfig)
    if err != nil {
        glog.Fatal(err)
    }

    clientset, err := kubernetes.NewForConfig(config)
    if err != nil {
        glog.Fatal(err)
    }

    watchlist := cache.NewListWatchFromClient(
        clientset.CoreV1().RESTClient(),
        string(v1.ResourceServices),
        v1.NamespaceAll,
        fields.Everything(),
    )

    _, controller := cache.NewInformer(
        watchlist,
        &v1.Service{},
        0,
        cache.ResourceEventHandlerFuncs{
            AddFunc: func(obj interface{}) {
                fmt.Println("Serviceが追加されました。\n")
            },
            DeleteFunc: func(obj interface{}) {
                fmt.Println("Serviceが削除されました。")
            },
            UpdateFunc: func(oldObj, newObj interface{}) {
                fmt.Println("Serviceが変更されました。")
            },
        },
    )
    stop := make(chan struct{})
    defer close(stop)
    go controller.Run(stop)
    for {
        time.Sleep(time.Second)
    }
}
{{< / highlight >}}

実行

```
go run watch_service.go

```


Kubernetesの中にいろんなControllerがあって、各種のリソース(Pod, Deployment, Serviceなど)を監視しています。これらのControllerがKubernetesクラスター上では`kube-controller-manager`の形で存在しています。`ps aux | grep kube-controller-manager`で確認することができます。

Controllerのメインの役割はKubernetesクラスターの現在の状態を期待されている状態を同じようにすることです。コードで表現すると、以下のようです。

```
for {
  desired := getDesiredState()
  current := getCurrentState()
  makeChanges(desired, current)
}
```


上のコードは`client-go`ライブラリを利用したKubernetesのServiceを監視するControllerです。Serviceが追加、変更、削除されると、コンソル上にメッセージが表示されます。



ControllerがKubernetes の API Serverと通信する必要があるので、`kubernetes.NewForConfig`関数で`~/.kube/config`ファイル中の認証情報を利用して、Kubernetes の API Serverと通信できるクライアントを生成します。



Controllerが継続的にKubernetesのAPI Serverに問い合わせるのはコストが高いので、`client-go`が [`cache`](https://godoc.org/k8s.io/client-go/tools/cache)の形でリソースを監視しています。 


上の`cache.NewListWatchFromClient`関数が全てのNamespaceのServiceをWatchするオブジェクトを作成します。

`cache.NewInformer`関数がKubernetesのServiceの状態が変わると通知するオブジェクトを作成します。`client-go`中の`Informer`が重要な概念、KubernetesのListとWatch二種類のAPIを呼び出しています。`Informer`が初期化されている際に一回だけKubernetesのList APIを呼び出して、リソースの一覧を取得します。そのあとはWatch APIを使って、更新した内容を`cache`に保存します。`Informer`は特に再度Syncすることなく、`cache`とKubernetesの情報を一致することを担保している。


`cache.NewInformer`４つ目の引数は[`cache.ResourceEventHandlerFuncs`](https://godoc.org/k8s.io/client-go/tools/cache#ResourceEventHandlerFuncs)で, この場合、Serviceの状態が変わると、どんな動きをするかを定義しておきます。上のコードはただメッセージを出力だけですが、例えば`Slack`に出力したら、[`workqueue`](https://engineering.bitnami.com/articles/a-deep-dive-into-kubernetes-controllers.html)に追加したりすることもできます。


これでKubernetesのServiceを作成、変更、削除してみます。

まずTerminal 1に上のControllerを実行します。初期状態でKubernetes中の全ての9個のサービスを一覧しました。

```
# go run watch_service.go
Serviceが追加されました。
Serviceが追加されました。
Serviceが追加されました。
Serviceが追加されました。
Serviceが追加されました。
Serviceが追加されました。
Serviceが追加されました。
Serviceが追加されました。
Serviceが追加されました。
```


Terminal 2にService `my-nginx`を作ると、 Termainal 1に`Serviceが追加されました。`というメッセージが表示されます。

```
# tee nginx-deployment-service.yaml <<-'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
  labels:
    app: my-nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      labels:
        app: my-nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.7.9
          ports:
            - containerPort: 80
---
kind: Service
apiVersion: v1
metadata:
  name: my-nginx
spec:
  selector:
    app: my-nginx
  ports:
  - protocol: TCP
    port: 80
EOF

# kubectl create -f nginx-deployment-service.yaml
```

`kubectl create`を実行すると、 Terminal 1に`Serviceが追加されました。`メッセージが表示されます。


`kubecctl edit svc my-nginx`コマンドで Serviceにラベルをつけたりして、保存すると、Terminal 1に`Serviceが変更されました。 `メッセージが表示されます。


`kubecctl delete svc my-nginx`コマンドで Serviceを削除すると、Terminal 1に`Serviceが削除されました。 `メッセージが表示されます。


参照：

https://stackoverflow.com/questions/44194842/how-to-watch-kubernetes-event-details-using-its-go-client




