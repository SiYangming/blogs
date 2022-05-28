---
layout: post
title: "R语言20-R语法: 变量的作用域"
date:   2019-05-02
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 变量与变量名

对相同的变量名多次赋值时，R如何识别该变量名所对应的值？
一个例子：

```source-r
> lm
function (formula, data, subset, weights, na.action, method = "qr", 
    model = TRUE, x = FALSE, y = FALSE, qr = TRUE, singular.ok = TRUE, 
    contrasts = NULL, offset, ...) 
{......
}
<bytecode: 0x7feae2a23638>
<environment: namespace:stats>
> lm <- function(x) { x * x }
> lm
function(x) { x * x }
```
当对 `lm` 赋值后，打印的值发生了变化，不再返回 *stats* 包中的 `lm` 所对应的值。

* * *

当对一个变量赋值时，R会搜索一系列的环境。当R取值时，环境的优先级从高到低如下：

1.  搜索全局环境中该变量名对应的值。
2.  搜索每个扩展包中命名空间中的变量列表。

`search` 函数可以查看R的变量搜索列表。

```source-r
> search()
[1] ".GlobalEnv"        "package:stats"     "package:graphics"
[4] "package:grDevices" "package:utils"     "package:datasets"
[7] "package:methods"   "Autoloads"         "package:base"
```

*   *全局变量global environment*或用户的工作空间在列表的最前面，*base*扩展包排在最后。
*   扩展包的前后顺序很重要。
*   用户可以设置R启动时加载的扩展包。
*   使用 `library` 函数加载扩展包时，该扩展包的命名空间会默认排在搜索列表的第二位，其他变量列表依次后移。
*   R中函数与非函数的命名空间是分离的，可以同时存在名称为c的R对象和函数。

* * *

## 变量的作用域

R语言的作用域规则与S语言截然不同。

*   作用域规则决定函数中值如何与自由变量的关联。
*   R使用的是静态作用域，一般称为lexical scoping，与之对应的是动态作用域。
*   作用域规则就是R在如何在变量列表中搜寻变量的规则。
*   Lexical scoping有助于简化统计计算。 

* * *

## Lexical Scoping
一个示例：
```source-r
f <- function(x, y) {
        x^2 + y / z
}
```
该函数有两个形式参数`x` 和 `y`，函数体内的 `z` 就是一个*自由变量*。编程语言的作用域规则决定值如何赋值给自由变量。自由变量既不是形式参数，也不是本地变量（在函数体内声明的变量）。

* * *

R的作用域规则：*在定义函数的环境中搜索自由变量的值*

环境：

*   环境就是一系列对应的变量名符号和值，如 `x` 是变量名符号，`3.14`是它对应的值。
*   每个环境都有一个母环境；一个环境可以有多个子环境。
*   没有母环境的环境是空环境。
*   函数 + 环境 = *闭包* 或 *函数闭包*（函数可以使用函数之外定义的自由变量）。

* * *

搜索自由变量的值：
*   如果在函数定义的环境中没有找到变量名对应的值，接着搜索母环境的变量。
*   依次搜索环境的母环境，一直到*顶层环境*；顶层环境一般是全局环境（工作空间）或者扩展包的命名空间。
*   顶层环境后，继续搜索直到碰到*空环境*。如果到达空环境，还没找到变量名对应的值，R就会报错。

* * *

如果在全局环境中定义函数，自由变量的值可以在用户的工作空间中找到，这是大多数情况下的正确做法。然而在R中，允许在函数中定义函数（类似C的编程语言不允许这样做）。在这种情况下，函数的定义环境就是其他函数的函数体。

* * *
在函数中定义另一个函数：
```source-r
make.power <- function(n) {
        pow <- function(x) {
                x^n 
        }
        pow 
}
```
该函数返回函数体内定义的另一个函数作为返回值。
```source-r
> cube <- make.power(3)
> square <- make.power(2)
> cube(3)
[1] 27
> square(3)
[1] 9
```

* * *

## 探索函数闭包
函数的环境：
```source-r
> ls(environment(cube))
[1] "n"   "pow"
> get("n", environment(cube))
[1] 3

> ls(environment(square))
[1] "n"   "pow"
> get("n", environment(square))
[1] 2
```

* * *

## 静态作用域vs动态作用域
声明两个函数：
```source-r
y <- 10

f <- function(x) {
        y <- 2
        y^2 + g(x)
}

g <- function(x) { 
        x*y
}

> f(3)
[1] 34
```

* * *

*   `g` 函数中 `y` 在lexical scoping的规则下，在函数定义的环境中（在本例中为全局环境）搜索 `y` 对应的值为10。
*   在动态作用域规则下， `y` 对应的值应该在函数被调用的环境中搜索。
    *   R中的调用环境称为*parent frame*。
    *   根据动态作用规则 `y` 值应当为2。

* * *

当一个函数在全局环境环境中声明并调用时，定义环境和调用环境是相同的，有时表现出动态作用的特征。
```source-r
> rm(list=ls())
> g <- function(x) { 
a <- 3
x+a+y 
}
> g(2)
Error in g(2) : object "y" not found
> y <- 3
> g(2)
[1] 8
```

* * *

其他支持lexical scoping的编程语言
*   Scheme
*   Perl
*   Python
*   Common Lisp

* * *

## Lexical Scoping的重要性

*   R中的所有对象都存储在内存中，所有函数都需要一个指向该函数定义环境的指针（可以在任何地方）。
*   S-PLUS一般在全局工作空间中搜索自由变量，因此所有对象可以储存在磁盘上，因为所有函数的定义环境都相同。

* * *

## 应用：优化

*   R中的优化函数 `optim`, `nlm`, and `optimize` 要求传递一个参数的向量 (如log-likelihood)
*   目标函数可能依赖于除参数之外的许多东西(如 *data*)
*   当对程序进行优化，用户更关注特定的参数

* * *

## 最大化标准似然
构建函数：

```source-r
make.NegLogLik <- function(data, fixed=c(FALSE,FALSE)) {
        params <- fixed
        function(p) {
                params[!fixed] <- p
                mu <- params[1]
                sigma <- params[2]
                a <- -0.5*length(data)*log(2*pi*sigma^2)
                b <- -0.5*sum((data-mu)^2) / (sigma^2)
                -(a + b)
        } 
}
```

*注意*：R中的优化函数都是最小化函数，需要使用负对数似然求最大化

* * *

```source-r
> set.seed(1); normals <- rnorm(100, 1, 2)
> nLL <- make.NegLogLik(normals)
> nLL
function(p) {
                params[!fixed] <- p
                mu <- params[1]
                sigma <- params[2]
                a <- -0.5*length(data)*log(2*pi*sigma^2)
                b <- -0.5*sum((data-mu)^2) / (sigma^2)
                -(a + b)
        }
<bytecode: 0x7feaed346eb0>
<environment: 0x7feae35d1bc8>
> ls(environment(nLL))
[1] "data"   "fixed"  "params"
```

* * *

## 参数估计

```source-r
> optim(c(mu = 0, sigma = 1), nLL)$par
      mu    sigma
1.218239 1.787343
```

当σ = 2时

```source-r
> nLL <- make.NegLogLik(normals, c(FALSE, 2))
> optimize(nLL, c(-1, 3))$minimum
[1] 1.217775
```

当μ = 1时

```source-r
> nLL <- make.NegLogLik(normals, c(1, FALSE))
> optimize(nLL, c(1e-6, 10))$minimum
[1] 1.800596
```

* * *

## 绘制最大似然图

```source-r
nLL <- make.NegLogLik(normals, c(1, FALSE))
x <- seq(1.7, 1.9, len = 100)
y <- sapply(x, nLL)
plot(x, exp(-(y - min(y))), type = "l")
```
![image.png](https://upload-images.jianshu.io/upload_images/7246523-042b6d5b51355cb6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```source-r
nLL <- make.NegLogLik(normals, c(FALSE, 2))
x <- seq(0.5, 1.5, len = 100)
y <- sapply(x, nLL)
plot(x, exp(-(y - min(y))), type = "l")
```
![image.png](https://upload-images.jianshu.io/upload_images/7246523-f3e18fa68648cb8b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* * *

## 小结

*   目标函数可以包含评估该函数的所有必须数据
*   对于交互式和探索的工作不需要提供太长的参数列表
*   代码可以更简洁

扩展阅读：
 Robert Gentleman and Ross Ihaka (2000). “Lexical Scope and Statistical Computing,” *JCGS*, 9, 491–508.
