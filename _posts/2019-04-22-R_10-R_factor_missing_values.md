---
layout: post
title: "R语言10-R语法：因子factor和缺失值"
date:   2019-04-22
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 因子Factors

因子用来表示数据分组，可以有序或无序。因子可以视作带标签*label*的整型向量。
* 因子可通过模型函数`lm()` 和`glm()`创建
* 使用标签的因子分组比使用数据更直观，“Male”和“Female”分组比1和2分组更易于理解
* * *
```r
> x <- factor(c("yes", "yes", "no", "yes", "no")) 
> x
[1] yes yes no yes no
Levels: no yes
> table(x) 
x
no yes 
 2   3
> unclass(x)
[1] 2 2 1 2 1
attr(,"levels")
[1] "no"  "yes"
```
* * *
水平的先后顺序可以使用`factor()`函数的`levels`参数来指定。这在线性模型中很重要，因为第一个水平常被用来作为对照。
```r
> x <- factor(c("yes", "yes", "no", "yes", "no"),
              levels = c("yes", "no"))
> x
[1] yes yes no yes no 
Levels: yes no
```
* * *
## 缺失值Missing Values
缺失值`NA`和`NaN`代表未定义的数学操作。
* `is.na()`函数用来测试R对象是否为`NA`
* `is.nan()`函数用来测试R对象是否为`NaN`
* `NA`值也有数据类型，整型`NA`， 字符串character `NA`等等
* `NaN`值属于`NA`值
* * *
```r
> x <- c(1, 2, NA, 10, 3)
> is.na(x)
[1] FALSE FALSE  TRUE FALSE FALSE
> is.nan(x)
[1] FALSE FALSE FALSE FALSE FALSE
> x <- c(1, 2, NaN, NA, 4)
> is.na(x)
[1] FALSE FALSE  TRUE  TRUE FALSE
> is.nan(x)
[1] FALSE FALSE  TRUE FALSE FALSE
```
