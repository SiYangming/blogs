---
layout: post
title: "R语言16-R语法：提取R对象的子集"
date:   2019-04-28
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

R中有很多操作可以提取R对象的子集。
*   `[` 操作返回跟原R对象类相同的对象；可以同时提取多个对象（例外）
*   `[[` 用来提取列表list或数据框dataframe对象；该操作只能用来提取单个元素，返回的对象不一定是list或数据框
*   `$` 通过list和dataframe的命名name来提取元素，语义上类似于 `[[`
```source-r
> x <- c("a", "b", "c", "c", "d", "a")
> x[1]
[1] "a"
> x[2]
[1] "b"
> x[1:4]
[1] "a" "b" "c" "c"
> x[x > "a"]
[1] "b" "c" "c" "d"
> u <- x > "a"
> u
[1] FALSE  TRUE  TRUE  TRUE  TRUE FALSE
> x[u]
[1] "b" "c" "c" "d"
```
* * *
## 提取矩阵的子集
矩阵的子集的提取通常通过 (*i,j*)来索引。
```source-r
> x <- matrix(1:6, 2, 3)
> x
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6
> x[1, 2]
[1] 3
> x[2, 1]
[1] 2
```
索引可以缺失。
```source-r
> x[1, ]
[1] 1 3 5
> x[, 2]
[1] 3 4
```
当提取矩阵的单个元素时，默认返回一个长度为1的向量，而不是一个1 × 1的矩阵。可以通过设置参数`drop = FALSE`来改变。
```source-r
> x <- matrix(1:6, 2, 3)
> x[1, 2]
[1] 3
> x[1, 2, drop = FALSE]
     [,1]
[1,]    3
```
* * *
类似的，提取矩阵的一列或一行时默认返回向量，而不是矩阵。
```source-r
> x <- matrix(1:6, 2, 3)
> x[1, ]
[1] 1 3 5
> x[1, , drop = FALSE]
     [,1] [,2] [,3]
[1,]    1    3    5

```
***
## 提取列表的子集
第一个例子
```source-r
> x<-list(foo = 1:4, bar = 0.6)
> x[1]
$foo
[1] 1 2 3 4

> x[[1]]
[1] 1 2 3 4
> x$bar
[1] 0.6
> x[["bar"]]
[1] 0.6
> x["bar"]
$bar
[1] 0.6

```
第二个例子
```source-r
> x <- list(foo = 1:4, bar = 0.6, baz = "hello")
> x[c(1, 3)]
$foo
[1] 1 2 3 4

$baz
[1] "hello"

```
`[[` 可以使用变量或表达式作为索引；`$` 只能按照命名的名称来索引。
```source-r
> x <- list(foo = 1:4, bar = 0.6, baz = "hello")
> name <- "foo"
## 使用变量索引 ‘foo’
> x[[name]]
[1] 1 2 3 4
## 不存在元素名称为 ‘name’
> x$name
NULL
## ‘foo’存在
> x$foo
[1] 1 2 3 4
```
* * *
## 提取嵌套列表（多维维列表）的子集
`[[` 可以使用整数序列作为索引。
```source-r
> x <- list(a = list(10, 12, 14), b = c(3.14, 2.81))
> x[[c(1, 3)]]
[1] 14
> x[[1]][[3]]
[1] 14
> x[[c(2, 1)]]
[1] 3.14
```
* * *
## 局部匹配
`[[` 和`$`允许名称的局部匹配来索引子集。
```source-r
> x <- list(aardvark = 1:5)
> x$a
[1] 1 2 3 4 5
> x[["a"]]
NULL
> x[["a", exact = FALSE]]
[1] 1 2 3 4 5
```
* * *
## 去除NA值
在进行数据分析的过程中，经常需要对缺失值(`NA`)进行处理。

```source-r
> x <- c(1, 2, NA, 4, NA, 5)
> bad <- is.na(x)
> x[!bad]
[1] 1 2 4 5
```
去除多个R对象的缺失值，并提取不含缺失值的子集。

第一个例子

```source-r
> x <- c(1, 2, NA, 4, NA, 5)
> y <- c("a", "b", NA, "d", NA, "f")
> good <- complete.cases(x, y)
> good
[1]  TRUE  TRUE FALSE  TRUE FALSE  TRUE
> x[good]
[1] 1 2 4 5
> y[good]
[1] "a" "b" "d" "f"
```
第二个列子
```source-r
> airquality[1:6, ]
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
5    NA      NA 14.3   56     5   5
6    28      NA 14.9   66     5   6
> good <- complete.cases(airquality)
> airquality[good, ][1:6, ]
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
7    23     299  8.6   65     5   7
8    19      99 13.8   59     5   8
```
