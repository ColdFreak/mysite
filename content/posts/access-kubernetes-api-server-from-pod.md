---
title: "Pod中からKubernetesのAPI Serverに認証"
date: 2018-07-06T18:21:04+09:00
draft: false
---

### 概要
やりたいことは簡単のアプリケーションを作って、Kubernetesの中にPodとしてデプロイして、このアプリは現在Kubernetesクラスター中に何個のPodがあるかをKubernetesのApi Serverに問い合わせる。

つまりPodの中のアプリケーションはどうやってKubernetesのAPI Serverに認証して、情報を取得することである。

このアプリを作るためにKubernetesの`client-go`ライブラリを使います。サンプルとして[ここに載っています](https://github.com/kubernetes/client-go/blob/master/examples/in-cluster-client-configuration/main.go)。Pod内からKubernetesのApi Serverと通信するための認証部分の設定は `rest.InClusterConfig()`を呼び出すことになる。

## プログラム作成

ファイル名は`in-cluster-client-configuration.go`

{{< highlight golang >}}
package main

import (
        "fmt"
        "time"

        metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
        "k8s.io/client-go/kubernetes"
        "k8s.io/client-go/rest"
)

func main() {
        // creates the in-cluster config
        config, err := rest.InClusterConfig()
        if err != nil {
                panic(err.Error())
        }
        // creates the clientset
        clientset, err := kubernetes.NewForConfig(config)
        if err != nil {
                panic(err.Error())
        }
        for {
                pods, err := clientset.CoreV1().Pods("").List(metav1.ListOptions{})
                if err != nil {
                        panic(err.Error())
                }
                fmt.Printf("There are %d pods in the cluster\n", len(pods.Items))
                time.Sleep(10 * time.Second)
        }
}
{{< / highlight >}}

Podの中からKubernetesのAPI Serverとやり取りするので、まずソースをビルドして、Podの形でKubernetesにデプロイする必要があります。

{{< highlight bash >}}
GOOS=linux go build -o ./app  in-cluster-client-configuration.go
docker build -t in-cluster .
docker tag in-cluster wzj8698/in-cluster
docker push wzj8698/in-cluster
{{< / highlight >}}


## デプロイ

これでアプリのImageを準備できました。Podとして`test-in-cluster`という名前のnamespaceにデプロイしたいので、`test-in-cluster` namespaceを作ります。namespaceが作成されるタイミングで`default`という名前のService Accountが作成される。


{{< highlight bash >}}
# kubectl create namespace test-in-cluster 

# kubectl get serviceaccounts -n test-in-cluster ### defaultという名前のservice account
NAME      SECRETS   AGE
default   1         58m


## 現在のnamespaceをtest-in-clusterに切り替え
# kubectl config set-context $(kubectl config current-context) --namespace=test-in-cluster 
{{< / highlight >}}

`kubectl run`コマンドでデプロイしてみる。


{{< highlight bash >}}
# kubectl run --rm -i demo  --image=wzj8698/in-cluster
{{< / highlight >}}

しかしチェックしてみるとデプロイは失敗しています。
`CrashLoopBackOff`というエラー出ています。

{{< highlight bash >}}
# kubectl get pod -n test-in-cluster
NAME                    READY     STATUS             RESTARTS   AGE
demo-7575dcf9c9-5b595   0/1       CrashLoopBackOff   4          2m


# kubectl describe pod demo-7575dcf9c9-5b595 -n test-in-cluster
......
  Warning  BackOff                1m (x7 over 2m)  kubelet, kube-04   Back-off restarting failed container
......
{{< / highlight >}}

そしてこのPodの内部のログを見てみます。`User "system:serviceaccount:test-in-cluster:default" cannot list pods at the cluster scope`のようなエラーが出ています。

{{< highlight bash >}}
# kubectl logs -f demo-7575dcf9c9-5b595 -n test-in-cluster
panic: pods is forbidden: User "system:serviceaccount:test-in-cluster:default" cannot list pods at the cluster scope
goroutine 1 [running]:
main.main()
        /root/work/golang/in-cluster-client-configuration.go:26 +0x1ec
{{< / highlight >}}

上のエラーの意味はPodが`test-in-cluster:default`というService Accountの身分でKubernetesのApi ServerからPodをリストしようとすると、権限がなくて、失敗している。


## Service Accountの意味

Kubernetesの中で二種類のアカウントがあります。一つはUser Account、もう一つはここに出てきた`Service Account`。

`User Account`は人がKubernetes のApi Serverと通信するために用意されているアカウントで、`Service Account`はPodの中に動いているプロセスがKubernetes のApi Serverと通信するためのアカウント。

つまり`Service Account`はPodのアイデンティティーでもあります。上の例では`demo-7575dcf9c9-5b595` Podはある`test-in-cluster` ネームスペース中の`default`という`Service Account`の身分でApi Serverと通信しようとしている

`Service Account`は色々な違う権限があります。上のアプリの例だと、そのPodがKubernetesのApi Serverからクラスター内の全部のPodの数を取得しようとしています。しかし、`User "system:serviceaccount:test-in-cluster:default" cannot list pods at the cluster scope`というエラーが出ているってことは`test-in-cluster:default`という名前のService AccountがPodをListする権限がない。

ここで`test-in-cluster:default`  Service AccountにPodをリストする権限を付与しないといけない。


Service Accountに権限を付与する際にKubernetesの`Role`や`ClusterRole`が必要となってくる。`ClusterRole`はクラスター範囲内でどんなことができるかを定義している。例えばどんなNamespaceの中でもPod作成、削除できるとか。

Kubernetesの中に[デフォルトでいくつかの`ClusterRole`を定義している](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#default-roles-and-role-bindings)。例えば`cluster-admin`という`ClusterRole`は何でもできる。

逆に`view`という`ClusterRole`は大体のもの（Pod、Deployment, Serviceとか）をみることができるけど、Pod、Deployment, Serviceとか作ることはできない。つまり`read only`になっている。


話が戻りますが、このアプリのService Account `test-in-cluster:default`に`veiw`の`ClusterRole`を付与すれば, KubernetesのApi Serverと通信して、Podの数を取得できはず。

やってみる . Service Account と `ClusterRole`をBindする必要があるので、Kubernetesの中で`clusterrolebinding`を作るということになる

{{< highlight bash >}}
kubectl create clusterrolebinding default-view --clusterrole=view --serviceaccount=test-in-cluster:default
{{< / highlight >}}

上のコマンドの意味は`test-in-cluster:default`という名前のService Accountに`view`という名前の`CluserRole`の権限を付与して、作成される のClusterRoleBindingの名前は `default-view`である。

これでこのアプリは`test-in-cluster:default` Service Accountの身分で動かしているので、今はKubernetesのクラスター内部のPodを全部リストできるはず

もう一回アプリをデプロイしてみると。ちゃんとPodが数が取れた。


{{< highlight bash >}}
kubectl delete demo -n test-in-cluster
kubectl run --rm -i demo  --image=wzj8698/in-cluster
There are 32 pods in the cluster
There are 32 pods in the cluster
There are 32 pods in the cluster
{{< / highlight >}}

