---
layout: post
title: "R语言17-R语法: 向量化操作"
date:   2019-04-29
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

R中的很多操作都是针对向量的，这也使得R代码高效、简洁、可读性强。

```source-r
> x <- 1:4; y <- 6:9 
> x
[1] 1 2 3 4
> y
[1] 6 7 8 9
> x + y 
[1]  7  9 11 13
> x > 2
[1] FALSE FALSE  TRUE  TRUE
> x >= 2
[1] FALSE  TRUE  TRUE  TRUE
> y == 8
[1] FALSE FALSE  TRUE FALSE
> x * y
[1]  6 14 24 36
> x / y
[1] 0.1666667 0.2857143 0.3750000 0.4444444
```

* * *

## 矩阵的向量化操作

```source-r
> x <- matrix(1:4, 2, 2); y <- matrix(rep(10, 4), 2, 2)
> x
     [,1] [,2]
[1,]    1    3
[2,]    2    4
> y
     [,1] [,2]
[1,]   10   10
[2,]   10   10
## element-wise乘法（数组元素依次相乘）
> x * y
     [,1] [,2]
[1,]   10   30
[2,]   20   40
> x / y
     [,1] [,2]
[1,]  0.1  0.3
[2,]  0.2  0.4
## 真正的矩阵乘法
> x %*% y
     [,1] [,2]
[1,]   40   40
[2,]   60   60
```
