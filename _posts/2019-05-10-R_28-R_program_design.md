---
layout: post
title: "R语言28-R语法: 程序优化"
date:   2019-05-10
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 编写R代码

1. 使用文本文件或编辑器 Always use text files / text editor
2. 适当的缩进
3. 限制代码的宽度
4. 限制自定义函数的长度

* * *

## 缩进

*  缩进提高代码的可读性
*  限制代码的长度，避免过多的嵌套和太长的函数
*  最少缩进4个空格，8个更完美

## 程序设计

*   检查程序代码不同部分的执行时间对于代码优化很有用。
*   通常代码运行一次会表现良好，但如果你将其放如1000次循环中，运行效率是否受到影响，这时分析运行时间很有用。

## 优化代码

*   影响代码执行速度的最大因素是代码花费最多时间的部分
*   不通过程序执行时间分析很难完成

> We should forget about small efficiencies, say about 97% of the time: premature optimization is the root of all evil（过早最优化是万恶的根源）

--Donald Knuth

* * *

## 优化的一般目标

*   先设计后优化
*   过早优化是万恶的根源
*   操作（收集数据），不要猜测
*   科学家的基本素养

## 使用 `system.time()`

*   接收任意R表达是作为输入，返回运行该表达式所需要的时间
*   计算执行表达所需要的时间（秒）
    *   如果有错误，返回报错前的时间
*   返回 `proc_time` 类的对象
    *   **user time**: CPU时间
    *   **elapsed time**: 总流逝时间
*   对于直接的计算任务，user time和elapsed time很接近
*   如果CPU花费大量的等待时间，elapsed time会比user time大很多
*   如果机器拥有多线程elapsted time会比user time小很多
    *   多线程BLAS库 (vecLib/Accelerate, ATLAS, ACML, MKL)
    *   多线程包**parallel** 

```R
> system.time(readLines("http://www.baidu.com"))
   user  system elapsed 
  0.026   0.010   1.231 

> hilbert <- function(n) { 
        i <- 1:n
        1 / outer(i - 1, i, "+")
}
> x <- hilbert(1000)
> system.time(svd(x))
   user  system elapsed 
  2.722   0.029   2.792 
```

* * *

## 计算更长表达式的运行时间
```R
> system.time({
    n <- 1000
    r <- numeric(n)
    for (i in 1:n) {
        x <- rnorm(n)
        r[i] <- mean(x)
    }
})
   user  system elapsed 
  0.079   0.005   0.086 
```

## `system.time()`的局限

 `system.time()` 允许测试特定的函数或代码块的运行时间，这是在知道问题在哪的时候，但如果不知道问题在哪呢？

* * *

## R 分析器

`Rprof()` 函数可以启动分析，`summaryRprof()` 函数可以总结 `Rprof()` 的输出结果，不要同时使用 `system.time()` 和 `Rprof()` 函数

*   Rprof() 有规律地追踪各个函数的调用，采样收集每个函数所花费的时间
*   默认采样间隔为0.02秒
*   如果代码已经运行很快，你就不需要使用这些分析函数

## R分析的原始输出
```R
## lm(y ~ x)

sample.interval=10000
"list" "eval" "eval" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"list" "eval" "eval" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"list" "eval" "eval" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"list" "eval" "eval" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"na.omit" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"na.omit" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"na.omit" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"na.omit" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"na.omit" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"na.omit" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"na.omit" "model.frame.default" "model.frame" "eval" "eval" "lm" 
"lm.fit" "lm" 
"lm.fit" "lm" 
"lm.fit" "lm" 
```

* * *

## 使用 `summaryRprof()`

*   The `summaryRprof()` function tabulates the R profiler output and calculates how much time is spend in which function
*   There are two methods for normalizing the data
*   "by.total" divides the time spend in each function by the total run time
*   "by.self" does the same but first subtracts out time spent in functions above in the call stack

## Summary

*   `Rprof()` runs the profiler for performance of analysis of R code

*   `summaryRprof()` summarizes the output of `Rprof()` and gives percent of time spent in each function (with two types of normalization)
