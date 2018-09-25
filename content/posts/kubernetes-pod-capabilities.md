---
title: "Kubernetes PodのCapabilities"
date: "2018-09-25T16:16:25+09:00"
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "kubernetes"
tags:
  - "kubernetes"
---
KubernetesにPodを作るとき，`securityContext`フィールドにコンテナの各種`capabilities`を追加，あるいは削除することができます． 例えば下のようなPodサンプルYamlファイルがあります．

```
$ tee capabilities-pod.yaml <<-'EOF'
apiVersion: v1
kind: Pod
metadata:
 name: mypod
spec:
 containers:
   - name: myshell
     image: "ubuntu:14.04"
     command:
       - /bin/sleep
       - "300"
     securityContext:
       capabilities:
         add:
           - NET_ADMIN  <<---- これ
EOF
```

```
$ kubectl create -f capability-test-pod.yaml # Podを作る
pod "mypod" created

$ kubectl exec mypod -- capsh --print # コンテナのcapabilitiesの中にやはりcap_net_admin出てきた
Current: = ...,cap_net_admin, ..., cap_setfcap+eip
...
```

`capsh --print`コマンドはコンテナの`capabilities`を表示させるコマンドです． そもそも `Container Capabilities`はなんですか？ それを気になって，調べていました．


### Linux Capabilities

Container Capabilitiesを説明する前にまずLinux Capabilitiesを知る必要があります． Linux Kernel 2.2からスーパユーザーの権限をもっと細かくコントロールすることができました． `man capabilities`マニュアルの中に40ぐらいの種類の`capabilities`があります． 前はスーパーユーザーでプログラムを実行するか， 普通ユーザーで実行するか2つだけ実行権限でしたが， Linux Capabilitiesを利用すると，プログラムが各リソースへの利用権限を細かく分類して， rootユーザーがすべてのリソースへアクセスできてしまうリスクを減らすことができます．

Linux Capabilityiesのリストはここに詳細が書かれています．

http://man7.org/linux/man-pages/man7/capabilities.7.html

#### `ping`

`ping`プログラムに使われている `cap_net_raw` capabilityを見てみます．これは`Raw Socket`を扱うときに使うcapabilityです． `Raw Socket`は一からパケットを組み立てるときに使うソケットのタイプです．

```
$ ls -l /usr/bin/ping
-rwxr-xr-x. 1 root root 44896 Nov 21  2015 /usr/bin/ping

$ getcap /usr/bin/ping
/usr/bin/ping = cap_net_admin,cap_net_raw+p
```

もし`cap_net_raw` capabilityなくなったら， pingもできなくなります．

```
$ sudo setcap -r /usr/bin/ping # pingプログラムのcapabilityをすべて削除

$ ping www.google.com # raw
ping: icmp open socket: Operation not permitted


$ sudo setcap cap_net_raw+p /usr/bin/ping

$ ping www.google.com
PING www.google.com (172.217.26.4) 56(84) bytes of data.
64 bytes from nrt20s02-in-f4.1e100.net (172.217.26.4): icmp_seq=1 ttl=54 time=4.44 ms
...
```

つまりプログラムは必要なcapabilityだけをつけて，そのリソースだけアクセスするときに，スーパーユーザーの権限を持たせています．


もう一つの例で`Raw Socket`の作成はスーパーユーザー権限が必要であることを確認してみます． プログラムをコンパイルした後に[`setcap`コマンド](https://www.insecure.ws/linux/getcap_setcap.html
)でcapabilityを追加して，やっと`Raw Socket`を開くことができました．


```
$ tee raw_socket.c <<-'EOF'
#include <sys/socket.h>
#include <arpa/inet.h>
#include <stdio.h>

int main(int argc, char *argv[]) {
        int sock;
        sock = socket(AF_INET, SOCK_RAW, IPPROTO_TCP); # Raw Socketを開く
        if (sock == -1) {
                perror("Failed to open raw socket");
                return 1;
        } else {
                printf("Raw socket successfully opened");
        }
}
EOF

$ gcc raw_socket.c -o raw_socket

$ ./raw_socket # 普通ユーザー権限で実行できないことが確認できた
Failed to open raw socket: Operation not permitted

$ sudo setcap cap_net_raw+ep ./raw_socket 

$ ./raw_socket # cap_net_raw権限をつけると実行できた
Raw socket successfully opened
```

### Container Capabilities

KubernetesのPodを作る話に戻りますが， Container Capabilitiesと Linux Capabilitiesの紐づくリストはここに書かれています．

https://kubernetes.io/docs/concepts/policy/container-capabilities/

下のようなPodの設定ファイルは**起動できない**はずです．


```
$ tee capabilities-test-pod.yaml <<-'EOF'
apiVersion: v1
kind: Pod
metadata:
 name: mypod
spec:
 containers:
   - name: ping-without-net-raw
     image: centos
     command:
       - /usr/bin/ping
       - "www.google.com"
     securityContext:
       capabilities:
         drop:
           - NET_RAW # cap_net_raw capabilityを削除する
EOF

$ kubectl create -f capabilities-test-pod.yaml


$ kubectl get pod mypod
NAME      READY     STATUS             RESTARTS   AGE
mypod     0/1       CrashLoopBackOff   4          2m


$ kubectl delete pod mypod
```

上の `capabilities-test-pod.yaml`ファイルを修正して,もう一回作ると，正しく動きます。

```
$ tee capabilities-test-pod.yaml <<-'EOF'
apiVersion: v1
kind: Pod
metadata:
 name: mypod
spec:
 containers:
   - name: ping-without-net-raw
     image: centos
     command:
       - /usr/bin/ping
       - "www.google.com"
     securityContext:
       capabilities:
         add:
           - NET_RAW # cap_net_raw 削除差ない限り，デフォルトではすでに入っている
EOF

$ kubectl create -f capability-test-pod.yaml

$ kubectl logs mypod
PING www.google.com (172.217.27.68) 56(84) bytes of data.
64 bytes from nrt12s15-in-f4.1e100.net (172.217.27.68): icmp_seq=1 ttl=54 time=3.99 ms
64 bytes from nrt12s15-in-f4.1e100.net (172.217.27.68): icmp_seq=2 ttl=54 time=4.06 ms
```
