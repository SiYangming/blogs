---
layout: post
title: "R语言13-R语法：R文件操作1"
date:   2019-04-25
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 获取数据的方式
* 利用键盘输入数据
* 读取存储在磁盘上的数据文件
* 通过开放数据库连接（ODBC, Open database Connectivity）访问数据库系统来获取数据(RODBC包)
## 键盘输入数据（数据量较少时）
```R
# 可以使用scan函数一个个输入来创建向量
> patientID <- scan()
1: 1
2: 2
3: 3
4: 4
5: 
Read 4 items
# 也可以直接使用c()函数创建向量，结果是相同的
> patientID <- c(1, 2, 3, 4)
> admdate <- c("10/15/2009", "11/01/2009", "10/21/2009", "10/28/2009")
> age <- c(25, 34, 28, 52)
> diabetes <- c("Type1", "Type2", "Type1", "Type1")
> status <- c("Poor", "Improved", "Excellent", "Poor")
> data <- data.frame(patientID, age, diabetes, status)
> data
  patientID age diabetes    status
1         1  25    Type1      Poor
2         2  34    Type2  Improved
3         3  28    Type1 Excellent
4         4  52    Type1      Poor


data2 <- data.frame(patientID=character(0), age=numeric(0), diabetes=character(), status=character())
# 使用edit和fix函数来添加数据
> data2 <- edit(data2)
> fix(data2)
```
其他R编辑数据的函数，用法与edit和fix类似

* vi(name = NULL, file = "")
* emacs(name = NULL, file = "")
* pico(name = NULL, file = "")
* xemacs(name = NULL, file = "")
* xedit(name = NULL, file = "")
