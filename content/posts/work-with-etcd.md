---
title: "DockerコンテナでEtcd3を動かす"
date: 2018-07-18T18:25:26+09:00
draft: false
disable_comments: false # Optional, disable Disqus comments if true
authorbox: true # Optional, enable authorbox for specific post
toc: true # Optional, enable Table of Contents for specific post
mathjax: true # Optional, enable MathJax for specific post
categories:
  - "docker"
  - "etcd"
tags:
  - "docker"
  - "etcd"
---


ETCD 3.3.8をローカルのDocker コンテナとして動かす。
Etcdは二つのポートを使っている。 一つは2379番ポート、 もう一つは　2380番ポート。 2379はクライアント通信のため, 2380はEtcdサーバー間の通信のためである。

```
rm -rf /tmp/etcd-data.tmp && mkdir -p /tmp/etcd-data.tmp && \
  docker rmi gcr.io/etcd-development/etcd:v3.3.8 || true && \
  docker run \
  -p 2379:2379 \
  -p 2380:2380 \
  --mount type=bind,source=/tmp/etcd-data.tmp,destination=/etcd-data \
  --name etcd-gcr-v3.3.8 \
  gcr.io/etcd-development/etcd:v3.3.8 \
  /usr/local/bin/etcd \
  --name s1 \
  --data-dir /etcd-data \
  --listen-client-urls http://0.0.0.0:2379 \
  --advertise-client-urls http://0.0.0.0:2379 \
  --listen-peer-urls http://0.0.0.0:2380 \
  --initial-advertise-peer-urls http://0.0.0.0:2380 \
  --initial-cluster s1=http://0.0.0.0:2380 \
  --initial-cluster-token tkn \
  --initial-cluster-state new

docker exec etcd-gcr-v3.3.8 /bin/sh -c "/usr/local/bin/etcd --version"
docker exec etcd-gcr-v3.3.8 /bin/sh -c "ETCDCTL_API=3 /usr/local/bin/etcdctl version"
docker exec etcd-gcr-v3.3.8 /bin/sh -c "ETCDCTL_API=3 /usr/local/bin/etcdctl endpoint health"
docker exec etcd-gcr-v3.3.8 /bin/sh -c "ETCDCTL_API=3 /usr/local/bin/etcdctl put foo bar"
docker exec etcd-gcr-v3.3.8 /bin/sh -c "ETCDCTL_API=3 /usr/local/bin/etcdctl get foo"
```


Etcdのversionを調べる。


```
$ curl -L http://127.0.0.1:2379/version
{"etcdserver":"3.3.8","etcdcluster":"3.3.0"}
```

Keyを登録する。

```
$ curl -X PUT http://127.0.0.1:2379/v2/keys/message -d value="Hello"
{"action":"set","node":{"key":"/message","value":"Hello","modifiedIndex":4,"createdIndex":4}}
```

Keyを取得する。

```
$ curl http://127.0.0.1:2379/v2/keys/message
{"action":"get","node":{"key":"/message","value":"Hello","modifiedIndex":5,"createdIndex":5}}
```

