---
title: "JenkinsfileにPrivate Docker RegistryからイメージをPull"
date: 2018-07-24T16:36:18+09:00
draft: false
---


KubernetesクラスターにJenkinsXが構築されている。Jenkinsfileでパイプラインを実行する際に例えばプライベートのDocker RegistryからイメージをPullしてきたい場合、ログインせずにPullはできない。

例えば下のようなJenkinsfileがあったとして

```
pipeline {
  agent {
    label 'jenkins-ruby'
  }
  stages {
    stage('CI Build and push snapshot') {
      when {
        branch 'PR-*'
      }
      steps {
        dir ('.') {
          container('ruby') {
            sh "docker pull my.private.docker.registry/projectX/app:latest"
          }
        }
      }
    }

  }
  post {
    always {
      cleanWs()
    }
  }
}
```

Pipelineが実行すると、下のようなエラー出てしまう。つまりログインできていないわけ。

```
+ docker pull my.private.docker.registry/projectX/app:latest
Pulling repository my.private.docker.registry/projectX/app:latest
Error: image projectX/app:latest not found
script returned exit code 1
```

[このページ](https://jenkins-x.io/architecture/docker-registry/#update-the-config-json-secret)を参照すると。JenkinsXはDocker Registry関連で`jenkins-docker-cfg`という名前の`Secret`を参照している。この`Secret`の中に正しいプライベートのDocker Registryの認証情報を入っていれば良い。

まず手動でプライベートのDocker Registryにログインして、`~/.docker/config.json`ファイルが作成される。このファイルの中の認証情報を利用して`jenkins-docker-cfg`を作る。

```
docker login my.private.docker.registry
Username : test-user
Password: xxxxx
```
これで認証情報の入っている`~/.docker/config.json`ファイルが出来上がる。`jenkins-docker-cfg`を削除して、もう一回この情報に基づいて、作り直す。

```
kubectl delete secret jenkins-docker-cfg -n jx
kubectl create secret -n jx generic jenkins-docker-cfg --from-file=/root/.docker/config.json
```

これでPipelinもう一回実行して、問題なくプライベートのDocker RegistryからPullできる。

参照リンク：

https://jenkins-x.io/architecture/docker-registry/#update-the-config-json-secret

https://kubernetes.slack.com/archives/C9MBGQJRH/p1532414003000222
