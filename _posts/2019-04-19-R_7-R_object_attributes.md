---
layout: post
title: "R语言7-R语法：对象与属性"
date:   2019-04-19
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---
## R的对象数据类型与数据结构
> R操作的实体在技术上来说都是对象(object)。当R在运行时，所有变量，数据，函数及结果都以对象的形式存在计算机的活动内存中，并有相应的名字对应。
## R的对象
R有五种基本（atomic）的数据类型：
*   字符串 character
*   数值型（实数）numeric
*   整数 integer
*   复合型 complex
*   逻辑型 logistic (True/False)

R最基本的对象是向量
*  向量（vertor）只能储存一种数据类型
*  列表（list）是个特例，用向量表示，可以存储不同数据类型的对象
`vector()` 函数可以创建一个空向量
```R
v <- vector()
```
* * *
## 数值
* 数值在R中储存为numeric对象（例如双精度实数）
* 如果想明确的声明一个整数，添加`L` 后缀
```R
> class(1)
[1] "numeric"
> class(1L)
[1] "integer"
```
* 特殊数值 `Inf` 表示无限infinity，`Inf`也可以用于计算正常计算
```R
> 1/ 0
[1] Inf
> 1 / Inf
[1] 0
```
* `NaN`代表未定义的值 (“not a number”)，可以看作一个缺失值
```R
> 0/0
[1] NaN
```
* * *
## 属性 Attributes
R对象有不同的属性
* 名称，names；行列名，dimnames
* 维度，dimensions (例如矩阵matrix，数组array)
* 类型，class
* 长度，length
* 模式，mode
* 其他自定义的属性attributes/metadata
`attributes()`函数可以查看一个对象的属性。
```R
> x <-matrix(0,4,5)
> x
     [,1] [,2] [,3] [,4] [,5]
[1,]    0    0    0    0    0
[2,]    0    0    0    0    0
[3,]    0    0    0    0    0
[4,]    0    0    0    0    0
> class(x)
[1] "matrix"
> mode(x)
[1] "numeric"
> length(x)
[1] 20
```
* * *
## 输入
 `<-` 符号进行赋值操作。

```r
> x <- 1
> print(x)
[1] 1
> x
[1] 1
> msg <- "hello"
> msg
[1] "hello"
```
编程语言的语法决定表达式是否完成。
```r
## 未完成的表达式
> x <- 
```
#字符开头表示注释，#字符右边的内容被忽略。
* * *

## 赋值计算Evaluation
输入一个完整的表达式，R语言会计算表达式，并返回结果。
```R
> x <- 5  ## 不打印
> x       ## 自动打印
[1] 5
> print(x)  ## 用print来输出结果
[1] 5
```
`[1]`表示`x`是一个向量，`5`是第一个元素。
* * *
## 输出 Printing

```R
> x <- 1:20 
> x
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15
[16] 16 17 18 19 20
```
`:`操作符用来创建整数序列。 
