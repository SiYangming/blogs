---
layout: post
title: "R语言4-R简介：R语言的运行环境"
date:   2019-04-16
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---
# R语言的运行环境
> * CRAN的R语言环境
* 集成开发环境Rstudio
* Microsoft R Open
* 基于anaconda/Miniconda构建的R环境
## CRAN的R语言环境
官方网站：[https://www.r-project.org/](https://www.r-project.org/)

windows下载后，双击安装。

Mac系统自带R，也可以下载安装，和windows类似。

```shell
# Ubuntu：
apt-get update
apt-get install r-base r-base-dev
# CentOS
yum install epel-release
yum install R
```
```shell
# 源码编译安装，CentOS7系统测试过，不需要安装依赖
wget https://mirrors.tuna.tsinghua.edu.cn/CRAN/src/base/R-3/R-3.5.3.tar.gz
tar zxvf R-3.5.3.tar.gz
cd R-3.5.3
./configure --enable-R-shlib=yes --with-tcltk --prefix=/opt/biosoft/R
make -j 4
sudo make install
# Linux：CentOS 7.5
echo 'export PATH=/opt/biosoft/R:$PATH' >> ~/.bashrc
```
## 最好用的R语言IDE--Rstudio
从https://www.rstudio.com/products/rstudio/download/（Free版即可）下载RStudio 

Desktop Open Source License

Windows和mac系统安装比较容易。

Linux服务器RStudio Server Open Source License的，需要进行一些网络的设置，安装好后，可以通过服务器ip地址通过网页使用服务器版的RStudio。

## Microsoft R Open
https://mran.microsoft.com/download
## 基于anaconda/Miniconda构建的R环境
软件包下载：https://docs.conda.io/en/latest/miniconda.html

windwos和mac都是可视化安装，比较简单。

Linux系统下载安装包后，需要使用命令行命令bash安装：

```shell
# 注意
bash Anaconda2-5.3.1-Linux-x86_64.sh
# 设置好安装路径，并同意将路径添加到系统PATH中，否则需要自己添加。
# 安装配置好conda后
conda install -y r
```
如此，可以开始R之旅了！！！
