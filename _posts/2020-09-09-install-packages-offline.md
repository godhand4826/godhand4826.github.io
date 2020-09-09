---
layout: post
title:  "install packages offline"
date:   2020-9-9 16:05:03 +0800
categories: linux yum apt
---
# centos with yum
```bash
yum --downloadonly --downloaddir=`pwd` install vim
tar zcvf vim.tgz *.rpm
# copy to offline machine
tar zxvf vim.tgz
yum install *.rpm
```
# ubuntu with apt
```bash
apt depends --recurse -i vim | grep '^\w' | sort -u | xargs apt download
tar zcvf vim.tgz *.deb
# copy to offline machine
tar zxvf vim.tgz
dpkg -i *.deb
```
