---
layout: post
title: "R语言11-R语法：数据框dataframe与R对象的命名"
date:   2019-04-23
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 数据框dataframe

数据框用来储存表格型的数据
* 数据框是一种特殊类型的多维列表，其中每个列表的长度必须相等
* 一个列表就是数据框的一列，列表的长度就是数据框的行数
* 与矩阵不同，数据框可以在一列中储存不同类型的对象（与list类似）；矩阵中的每个元素都必须是相同的数据类型
* 数据框有一个特殊的属性行名`row.names`
* 数据框一般通过调用`read.table()`或`read.csv()`函数构建
* 数据框可以通过调用`data.matrix()`函数转换为矩阵

```source-r
> x <- data.frame(foo = 1:4, bar = c(T, T, F, F)) 
> x
  foo   bar
1   1  TRUE
2   2  TRUE
3   3 FALSE
4   4 FALSE
> nrow(x)
[1] 4
> ncol(x)
[1] 2
```
* * *
## R对象的命名
对R对象进行命名，可以提升代码的可读性。
```r
> x <- 1:3
> names(x)
NULL
> names(x) <- c("foo", "bar", "norf") 
> x
foo bar norf 
  1   2    3
> names(x)
[1] "foo"  "bar"  "norf"
```
* * *
对列表list命名。
```r
> x <- list(a = 1, b = 2, c = 3) 
> x
$a
[1] 1

$b 
[1] 2

$c 
[1] 3
```
* * *
矩阵matrix的命名。
```r
> m <- matrix(1:4, nrow = 2, ncol = 2)
> dimnames(m) <- list(c("a", "b"), c("c", "d")) 
> m
  c d 
a 1 3 
b 2 4
```
