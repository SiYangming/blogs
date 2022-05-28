---
layout: post
title: "R语言9-R语法：矩阵matrix和列表list"
date:   2019-04-21
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 矩阵matrix

矩阵是具有二维*dimension*  (nrow, ncol)维度属性的向量。
```r
> m <- matrix(nrow = 2, ncol = 3) 
> m
     [,1] [,2] [,3]
[1,]   NA   NA   NA
[2,]   NA   NA   NA
> dim(m)
[1] 2 3
> attributes(m)
$dim
[1] 2 3
> dim(m)
```
* * *
矩阵通过扩展列*column-wise*构建，生成矩阵时，输入从左上角开始，顺列填充。
```r
> m <- matrix(1:6, nrow = 2, ncol = 3) 
> m
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6
```
* * *
矩阵可以通过向量添加维度属性来创建。
```r
> m <- 1:10 
> m
[1] 1 2 3 4 5 6 7 8 9 10 
> dim(m) <- c(2, 5)
> m
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    3    5    7    9
[2,]    2    4    6    8   10
```
* * *
## cbind和rbind
矩阵可以通过列合并*column-binding*函数`cbind()`及行合并*row-binding*函数`rbind()`来创建。
```r
> x <- 1:3
> y <- 10:12
> cbind(x, y)
     x  y 
[1,] 1 10 
[2,] 2 11 
[3,] 3 12
> rbind(x, y) 
  [,1] [,2] [,3]
x    1    2    3
y   10   11   12
```
* * *

## 列表list
列表list是可以包含不同数据类型的特殊向量。列表是R中非常重要的数据类型。
```r
> x <- list(1, "a", TRUE, 1 + 4i) 
> x
[[1]]
[1] 1

[[2]] 
[1] "a"

[[3]]
[1] TRUE

[[4]]
[1] 1+4i
```
