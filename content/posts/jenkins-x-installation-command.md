---
title: "Jenkins XをKubernetesにインストールする際のコマンド"
date: 2018-07-13T08:17:27+09:00
draft: false
---


事前に用意しているKubernetesクラスターはIDCFクラウド上で作られている。[Nginx Ingress Controller](https://github.com/kubernetes/ingress-nginx/blob/master/docs/deploy/index.md)と[Metallb](https://metallb.universe.tf/installation/)がインストール済み。Metallbを使うと`LoadBalancer` タイプのNginx Ingress Controller Serviceが作られる。 Jenkins Xがこの`LoadBalancer` タイプのIngress Controllerが必要としている。

Jenkins XをKubernetesクラスターにインストールする際のコマンドは以下のようになる。


```
jx install \
--exposer='Ingress' \
--ingress-namespace='ingress-nginx' \
--ingress-service='ingress-nginx' \
--ingress-cluster-role='nginx-ingress-clusterrole' \
--ingress-deployment='nginx-ingress-controller' \
--namespace='jx' \
--provider='kubernetes' \
--default-environment-prefix='ravengeode'
--git-api-token='xxxxxxxxxxxxxxxxxxxx'
```

`--git-api-token`オプションは下のリンクから作成することができる。

```
https://github.com/settings/tokens/new?scopes=repo,read:user,read:org,user:email,write:repo_hook,delete_repo
```


