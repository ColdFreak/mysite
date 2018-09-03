---
title: "GolangをCentos7にインストール"
date: 2018-07-01T01:48:34+09:00
draft: false
---


Centos 7にGolangをインストール


{{< highlight sh >}}
$ wget https://storage.googleapis.com/golang/go1.10.3.linux-amd64.tar.gz
sudo tar -C /usr/local -xvzf go1.10.3.linux-amd64.tar.gz

$ tee -a /etc/profile.d/path.sh <<-'EOF'
export PATH=$PATH:/usr/local/go/bin
EOF

$ tee -a  ~/.bash_profile <<-'EOF'
export GOBIN="$HOME/go-projects/bin"
export GOPATH="$HOME/go-projects"
EOF

$ mkdir -p $HOME/go-projects/bin $HOME/go-projects/src
$ source /etc/profile && source ~/.bash_profile
{{< / highlight >}}
