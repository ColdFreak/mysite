---
title: "kubectlでPodを作成のプロセス"
date: 2018-07-19T10:09:10+09:00
draft: false
---

`kubectl`でPodを作成する際に、Kubernetesの中で何か起こっているかをわかりやすく説明見つけたので、メモしておく。

https://medium.com/jorgeacetozi/kubernetes-master-components-etcd-api-server-controller-manager-and-scheduler-3a0179fc8186


![kubectlでPodの作成プロセス](../../pod-creation/pod-creation.png)


- kubectl writes to the API Server.
- API Server validates the request and persists it to etcd.
- etcd notifies back the API Server.
- API Server invokes the Scheduler.
- Scheduler decides where to run the pod on and return that to the API Server.
- API Server persists it to etcd.
- etcd notifies back the API Server.
- API Server invokes the Kubelet in the corresponding node.
- Kubelet talks to the Docker daemon using the API over the Docker socket to create the container.
- Kubelet updates the pod status to the API Server.
- API Server persists the new state in etcd.




