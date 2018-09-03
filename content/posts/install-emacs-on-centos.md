---
title: "Centos7に新しいEmacsを入れる"
date: 2018-07-03T10:30:06+09:00
draft: false
---


{{< highlight bash >}}

# yum remove emacs-nox emacs-filesystem emacs-common -y

# yum install gcc ncurses-devel  -y

# cd /opt

# wget http://ftp.gnu.org/pub/gnu/emacs/emacs-25.3.tar.gz

# tar zxvf emacs-25.3.tar.gz

# cd /opt/emacs-25.3

# ./configure --without-x

# make

# make install

# emacs
-bash: /usr/bin/emacs: そのようなファイルやディレクトリはありません

# mv /usr/bin/emacs /usr/bin/emacs.orig

# ln -s /usr/local/bin/emacs /usr/bin/emacs

# mv ~/.emacs.d ~/.emacs.d.orig

# git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d

# emacs 
{{< / highlight >}}
