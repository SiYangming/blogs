---
layout: post
title: "R语言15-R语法：R文件操作3"
date:   2019-04-27
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 文本格式
*   `dump`和dput函数的是文本格式，可以直接编辑，在文件出错时，可以恢复。
*   `dump`和`dput`保存了对象的*metadata*（R对象的属性信息），这降低了结果的可读性，但读入数据时，不需要在对该数据集进行额外的处理。
*   文本格式文件方便进行版本控制，git等命令只能追踪文本文件的改变。
*   文件格式用处更广泛，如果文本中出现错误，相比二进制文件，更容易被修复。
*   文本格式更符合Unix哲学
*   缺点：占用更多的空间（相比于二进制文件）
## dput操作R对象
通过dput函数将R对象写入文件，使用`dget`函数将文件读入R。
```r
> y <- data.frame(a = 1, b = "a")
> dput(y)
structure(list(a = 1,
               b = structure(1L, .Label = "a",
                             class = "factor")),
          .Names = c("a", "b"), row.names = c(NA, -1L),
          class = "data.frame")
> dput(y, file = "y.R")
> new.y <- dget("y.R")
> new.y
   a  b 
1  1  a
```
* * *
## dump函数操作R对象
dump函数可以同时打包多个R对象写入文件，可以使用`source`函数将文件读回R。
```r
> x <- "foo"
> y <- data.frame(a = 1, b = "a")
> dump(c("x", "y"), file = "data.R") 
> rm(x, y)
> source("data.R")
> y
  a  b 
1 1  a
> x
[1] "foo"
```

* * *

## 其他数据操作的函数
数据操作可以通过创建一个链接（其他语言称为文件句柄）来实现。 
*   `file` 创建一个普通文件的链接
*   `gzfile` 创建一个gzip格式压缩文件的链接
*   `bzfile` 创建一个bzip2格式压缩文件的链接
*   `url` 创建一个读取网页的链接
## 文件链接
```r
> str(file)
function (description = "", open = "", blocking = TRUE,
          encoding = getOption("encoding"))
```
*   `description`参数设置读取文件的名称
*   `open`参数设置文件的操作权限
    *   “r” 只读
    *   “w” 可写并初始化一个新文件
    *   “a” 追加
    *   “rb”, “wb”, “ab” 读、写、追加二进制格式文件
*   文件链接可以帮助用户更灵活地操作文件或外部对象，我们不用直接对该链接进行操作，R中的函数可以直接处理。
```r
con <- file("foo.txt", "r")
data <- read.csv(con)
close(con)
```
等同于
```source-r
data <- read.csv("foo.txt")
```
* * *
## 按行读取文件
```r
> con <- gzfile("words.gz") 
> x <- readLines(con, 10) 
> x
 [1] "1080"     "10-point" "10th"     "11-point"
 [5] "12-point" "16-point" "18-point" "1st"
 [9] "2"        "20-point"
```
`writeLines`读取字符串向量，将向量中每行对象按行写入文件。
* * *
`readLines`在读取网页数据十分有用
```r
> con <- url("http://www.baidu.com", "r")
> x <- readLines(con)
> head(x)
[1] "<!DOCTYPE html>"  "<!--STATUS OK-->" ""                 ""                
[5] ""  
```
