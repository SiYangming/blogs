---
layout: post
title: "R语言22-R语法: apply函数家族"
date:   2019-05-04
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 命令行的循环

循环对于编程十分重要，但在R交互式命令行中编写循环结构不太方便，R提供了一系列函数来简化循环操作。

*   `lapply`: 对一个列表进行循环，对列表中的，每个元素执行指定的函数操作，返回与原列表等长的结果
*   `sapply`: 与 `lapply` 相同，简化结果
*   `apply`: 对数组或矩阵执行指定的函数，返回向量、数组或列表
*   `tapply`: 对向量的子集进行操作
*   `mapply`:  `lapply` 和 `sapply` 的升级版

 `split` 函数辅助 `lapply` 会产生一些神操作。

* * *

## lapply

`lapply` 需要三个参数： 
(1) 列表 `X`;
(2) 函数或函数名`FUN`; 
(3) 其他参数通过 ... 参数传递。
如果 `X` 不是列表，会通过 `as.list` 函数强制转换为列表。
```R
> lapply
function (X, FUN, ...) 
{
    FUN <- match.fun(FUN)
    if (!is.vector(X) || is.object(X)) 
        X <- as.list(X)
    .Internal(lapply(X, FUN))
}
<bytecode: 0x7fd35e9dc718>
<environment: namespace:base>
```
实际循环操作是通过内部的C代码完成。

* * *

`lapply` 函数或返回一个列表，但会忽略输入数据的类型。

```R
> x <- list(a = 1:5, b = rnorm(10))
> lapply(x, mean)
$a
[1] 3

$b
[1] -0.1843325

```

* * *

```R
> x <- list(a = 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
> lapply(x, mean)
$a
[1] 2.5

$b
[1] -0.1599965

$c
[1] 1.068789

$d
[1] 4.837933

```

* * *

```R
> x <- 1:4
> lapply(x, runif)
[[1]]
[1] 0.9785864

[[2]]
[1] 0.5644791 0.9242951

[[3]]
[1] 0.9274006 0.3646827 0.2551950

[[4]]
[1] 0.3987640 0.5774553 0.7631348 0.7837444
```

* * *

```R
> x <- 1:4
> lapply(x, runif, min = 0, max = 10)
[[1]]
[1] 4.615299

[[2]]
[1] 3.011078 5.549246

[[3]]
[1] 8.5230253 3.4520337 0.1495026

[[4]]
[1] 5.9855820 7.7427082 2.9215029 0.3520867

```

* * *

`lapply` 也可以使用匿名函数

```R
# 声明包含两个矩阵的列表
> x <- list(a = matrix(1:4, 2, 2), b = matrix(1:6, 3, 2)) 
> x
$a
     [,1] [,2]
[1,]    1    3
[2,]    2    4

$b
     [,1] [,2]
[1,]    1    4
[2,]    2    5
[3,]    3    6


# 提取每个矩阵的第一列的匿名函数
> lapply(x, function(elt) elt[,1])
$a
[1] 1 2

$b
[1] 1 2 3

```

* * *

## sapply

`sapply` 会尽可能的简化 `lapply` 函数的结果。

*   如果列表中的每个元素的长度为1，结果会返回一个向量。
*   如果列表中的每个元素都是长度大于1的等长向量，结果会返回一个矩阵。
*   如果无法简化，返回列表。

* * *

```R
> x <- list(a = 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
> lapply(x, mean)
$a
[1] 2.5

$b
[1] 0.3727818

$c
[1] 1.17464

$d
[1] 5.07108

> sapply(x, mean)
        a         b         c         d 
2.5000000 0.3727818 1.1746396 5.0710795 
> mean(x)
[1] NA
Warning message:
In mean.default(x) : argument is not numeric or logical: returning NA
```
