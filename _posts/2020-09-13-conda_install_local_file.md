---
layout: post
title: "conda安装本地文件"
date:   2020-09-13
tags: [转载,conda,linux]
comments: true
author: Si Yangming
toc : true
---

## 1. 本地安装anaconda包

anaconda本质是python的包，有很多包，在国内镜像源找不到，需要从外网下载，然后，到内网使用本地安装下载。

conda使用本地包安装命令为：

1. 下载安装包到`~/anaconda3/pkgs/` 下
2. cd 到 `~/anaconda3/pkgs/` 目录下
3. 使用命令`conda install --use-local xxx.tar.bz2` 安装本地包。

第二节实践从`https://anaconda.org` 下载r-base包，并使用本地安装。

## 2. 使用本地包安装r-base包

命令如下：

```shell
curl 'https://anaconda.org/r/r-base/3.6.1/download/linux-64/r-base-3.6.1-h9bb98a2_1.tar.bz2' -Lo ~/anaconda3/pkgs/r-base-3.6.1.tar.bz2
cd ~/anaconda3/pkgs
conda install --use-local r-base-3.6.1.tar.bz2
```

运行如上命令，即在本地安装了r-base的anaconda包。

## 3.本地安装包下载

清华大学开源软件镜像站（macOS）：https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/osx-64/

## 转载

[linux笔记: conda安装本地文件](http://www.liangxiaolei.fun/2020/03/16/linux%E7%AC%94%E8%AE%B0-conda%E5%AE%89%E8%A3%85%E6%9C%AC%E5%9C%B0%E6%96%87%E4%BB%B6/)

