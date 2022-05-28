---
layout: post
title: "R语言14-R语法：R文件操作2"
date:   2019-04-26
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 读取数据
R中有很多函数可以读取数据
* read.table, read.csv用来读取制表符分隔的数据
* readLines 读取文本文件的一行
* source 读取R代码文件 (与dump对应)
* dget 读取R代码文件 (与dput对应
* load 读取保存的工作空间
* unserialize 读取一个serialize封装的二进制R对象
## 写入文件
R中有很多功能类似的函数将数据写入文件
* write.table
* writeLines
* dump
* dput
* save
* serialize
## 使用read.table函数读取数据
read.table函数是最长用的读取数据的函数。它有几个非常重要的参数：
```
file 文件的名字或者链接
header 逻辑值表示文件是否有表头
sep 分隔符，指定列分隔符是什么
colClasses 字符串向量表示数据集中每列的类
nrows 数据集的行数
comment.char 字符串表示注释
skip 忽略开始的多少行
stringsAsFactors 逻辑值，设置字符串变量是否用因子结构储存
```
***
对于中小型数据集，使用read.table函数的默认参数即可
```R
data<-read.table("foo.txt")
```
* R会自动忽略以“#”开头的行
* 设置数据集的行数（需要占用多少内存）和每列数据的变量类型可以加快R的运行效率f
read.csv函数除了默认分隔符是逗号以外，其他参数和read.table函数相同。
## 使用read.table函数读取大数据集
对于大型数据集，首先大致估计一下需要使用多少内存来储存数据集，如果数据所需要的内存大于计算机的物理内存，那就需要使用其他方式处理数据。
* 如果数据集中没有注释行将参数comment.char = "" 
* 使用colClasses参数可以提高一倍的读取速度，这要求指定数据集中每一列的数据类型。如果每列都是数值，可以直接设置参数colClasses = "numeric"。另一个快速设置每列的类型的方法如下：
```
initial<-read.table("datatable.txt",nrows=100)
classes<-sapply(initial,class)
tabAll<-read.table("datatable.txt",colClasses=classes)
```
* 设置nrows参数。不能提速当能够帮助R优化内存的使用，可以使用Linux的wc命令来计算数据集的行数。
## 了解一些操作系统
使用R处理大数据集时，需要了解一些操作系统的情况。
* 电脑的内存多大
* 有什么应用程序在运行
* 是否有其他用户登陆到该操作系统
* 什么操作系统
* 32还是64位操作系统
## 计算内存使用
一个1,500,000行和120列的数值型数据框占用内存的计算：
1,500,000 × 120 × 8 bytes/numeric
= 1440000000 bytes
= 1440000000 / $2^{20}$ bytes/MB
= 1,373.29 MB
= 1.34 GB
