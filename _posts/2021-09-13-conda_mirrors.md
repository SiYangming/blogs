---
layout: post
title: "conda镜像"
date:   2021-09-13
tags: [转载,conda,linux]
comments: true
author: Si Yangming
toc : true
---

Anaconda 安装包可以到 https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/ 下载。

TUNA 还提供了 Anaconda 仓库的镜像，运行以下命令:

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

## 克隆环境

使用终端或Anaconda提示进行以下步骤。

您可以通过创建它的一个副本来制作一个完整的环境副本：

```shell
conda create --name myclone --clone myenv
conda create -n your_env_name python=X.X（2.7、3.6等)
```

激活一个环境：

```shell
conda activate myenv
```

Conda myenv在您的系统命令上加上路径名。

## 停用的环境

取消激活环境：

```shell
conda deactivate
```

## 在一个环境中使用PIP 

要在您的环境中使用pip，请在终端窗口或Anaconda提示符下运行：

```shell
conda install -n myenv pip
conda activate myenv
pip <pip_subcommand>
```

## 查看您的环境列表

要查看所有环境的列表，请在终端窗口或Anaconda提示符下运行：

```shell
conda info --envs
```

要么

```shell
conda env list
```

## Conda 三方源

当前tuna还维护了一些anaconda三方源。

```shell
# Conda Forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
# msys2
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
# bioconda
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
# menpo
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
# pytorch
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
```

## 转载

原文链接：https://blog.csdn.net/hanguo1577717382/article/details/80245707