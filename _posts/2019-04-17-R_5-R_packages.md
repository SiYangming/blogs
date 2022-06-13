---
layout: post
title: "R语言5-R简介：R语言扩展包"
date:   2019-04-17
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---
> utils包中的*.packages函数管理CRAN源的扩展包
>
> BiocManager管理bioconductor中的扩展包
>
> devtools安装开发中扩展包
>
> githubinstall安装github上的扩展包

## 镜像源

* CRAN的镜像列表：[https://mirrors.tuna.tsinghua.edu.cn/CRAN/mirrors.html](https://mirrors.tuna.tsinghua.edu.cn/CRAN/mirrors.html)
* Bioconductor镜像列表：[http://www.bioconductor.org/about/mirrors/](http://www.bioconductor.org/about/mirrors/)
* GitHub也有很多开发中的R包：[https://github.com/](https://github.com/)

```R
## 修改CRAN的镜像源（官方的软件源在国外，速度会受影响）
# 清华镜像
options("repos" = c(CRAN="https://mirrors.tuna.tsinghua.edu.cn/CRAN/"))
options(BioC_mirror="https://mirrors.tuna.tsinghua.edu.cn/bioconductor")
# 中科大镜像
options(repos = c(USTC="https://mirrors.ustc.edu.cn/CRAN/"))
options(BioC_mirror="https://mirrors.ustc.edu.cn/bioc/")
```

清华bioconductor镜像网址：[https://mirrors.tuna.tsinghua.edu.cn/help/bioconductor/](https://mirrors.tuna.tsinghua.edu.cn/help/bioconductor/)

清华CRAN镜像网址：[https://mirrors.tuna.tsinghua.edu.cn/help/CRAN/](https://mirrors.tuna.tsinghua.edu.cn/help/CRAN/)

中科大CRAN镜像网址：[https://mirrors.ustc.edu.cn/help/CRAN.html](https://mirrors.ustc.edu.cn/help/CRAN.html)

## utils包中的*.packages函数

```R


## 列出所有当前可用扩展包
> available.packages()
                              Package                         Version         
A3                            "A3"                            "1.0.0"         
abbyyR                        "abbyyR"                        "0.5.4"         
abc                           "abc"                           "2.1"           
......
## 函数返回一个矩阵形式的扩展包列表，包括软件包名称及当前最新版本

## 安装扩展包
> install.packages("PackageName")

## 查看已安装的软件包
> installed.packages("PackageName")

## 更新软件包
## 更新特定扩展包
> update.packages("PackageName")
## 更新所有软件包
> update.packages()

## 移除扩展包
> remove.packages("PackageName")
```
## BiocManager：管理bioconductor中的扩展包
```R
if (!"BiocManager" %in% rownames(installed.packages()))
     install.packages("BiocManager")
## 修改biocondutor的镜像
options(BioC_mirror="https://mirrors.ustc.edu.cn/bioc/")

## 列出所有可以安装的R扩展包
> BiocManager::available()

## 安装扩展包
> BiocManager::install("PackageName")
## 由于许多biocondutor扩展包是依赖CRAN上的扩展包的，因此，BiocManager::install()也可以用来安装CRAN的扩展包
## 同时安装多个扩展包
pkgs <- c("PackageName1", "PackageName2", "PackageName3",...)
BiocManager::install(pkgs)
```
## devtools安装开发中扩展包
https://cran.r-project.org/package=devtools

devtools包含了一系列用来开发R包的工具。

```R
# install.packages("devtools")
library(devtools)
install_github('hadlley/dplyr')
```
## githubinstall安装github上的扩展包
[https://cran.r-project.org/package=githubinstall](https://cran.r-project.org/package=githubinstall)

```R
#install.packages('githubinstall') 
library(githubinstall)
githubinstall('AnomalyDetection')
```
## R包安装常见错误

### 第一个：`Error in readRDS(file) : unknown input format`

1. 删除.Rhistory和.RData文件

2. 运行 `update.packages()`

3. ```R
   # List the library paths
   # The issue is likely to be in the first directory
   paths = .libPaths()
   
   ## Try and detect bad files
   list.files(paths, 
          pattern = "^00LOCK*|*\\.rds$|*\\.RDS$",
          full.names = TRUE)
   
   ## List files of size 0
   l = list.files(paths, full.names = TRUE)
   l[sapply(l, file.size) == 0]
   ```

参考：https://stackoverflow.com/questions/6473831/readrdsfile-in-r

## 更新日志

2022年6月13日：更新镜像和包的安装，R包安装常见错误。
