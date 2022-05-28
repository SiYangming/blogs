---
layout: post
title: "R语言18-R语法: 控制结构"
date:   2019-04-30
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

R中的控制结构允许用户在不同情况下设置程序的执行流程。常见的结构包括：
*   `if`, `else`: 判断条件
*   `for`: 执行固定次数的循环
*   `while`: 当 *while* 的条件为真时，执行循环
*   `repeat`: 执行无限次循环
*   `break`: 退出整个循环
*   `next`: 跳过本次循环
*   `return`: 退出函数
大多数控制结构一般不在R的交互命令对话中使用，主要应用在自定义函数或较长的表达式中。

* * *

## 控制结构：if

```source-r
if(<condition>) {
        ## do something
} else {
        ## do something else
}
if(<condition1>) {
        ## do something
} else if(<condition2>)  {
        ## do something different
} else {
        ## do something different
}
```
下面是一些有效的 if/else 结构示例。

```source-r
if(x > 3) {
        y <- 10
} else {
        y <- 0
}
```
等同于
```source-r
y <- if(x > 3) {
        10
} else { 
        0
}
```
else语句不是必须的。

```source-r
if(<condition1>) {

}

if(<condition2>) {

}
```

* * *

## for
`for` 循环接收可迭代的变量，一般是来自序列或向量的连续值。for循环经常使用的循环元素来自于列表、向量等R对象。
```source-r
for(i in 1:10) {
         print(i)
}
```
该循环将变量 `i` 作为循环变量，循环依次赋值1, 2, 3, ..., 10，然后退出循环。
***
下面几个循环的功能相同。
```source-r
x <- c("a", "b", "c", "d")

for(i in 1:4) {
        print(x[i])
}

for(i in seq_along(x)) {
        print(x[i])
}

for(letter in x) {
        print(letter)
}

for(i in 1:4) print(x[i])
```

* * *

## 嵌套循环
`for` 循环可以嵌套使用。
```source-r
x <- matrix(1:6, 2, 3)

for(i in seq_len(nrow(x))) {
        for(j in seq_len(ncol(x))) {
                print(x[i, j])
        }   
}
```
慎重使用嵌套循环，嵌套超过2-3层后会难以阅读和理解，也会拖慢程序的运行速度。

* * *

## while

while循环开始前先测试循环条件。如果值为真，执行循环体。当循环体运行完，再次测试循环条件，如此循环下去。
```source-r
count <- 0
while(count < 10) {
        print(count)
        count <- count + 1
}
```
while循环如果没有正确编写，可能会陷入无限循环（死循环），使用时要注意。
***
有时需要同时满足多个判断条件进行循环。
```source-r
z <- 5

while(z >= 3 && z <= 10) {
        print(z)
        # 随机生成0或1
        coin <- rbinom(1, 1, 0.5) 

        if(coin == 1) { 
                z <- z + 1
        } else {
                z <- z - 1
        } 
}
```
判断条件是从左到右进行判断。

* * *

## repeat
repeat初始化一个无限循环，有特殊用途，通常不应用在数据统计分析中，退出`repeat`循环的唯一方法是`break`。
以下循环是个repeat的无限循环，运行后需要强制终止。
```source-r
x0 <- 1
tol <- 0

repeat {
        x1 <- rbinom(1, 1, 0.5) 

        if(abs(x1 - x0) < tol) {
                break
        } else {
                x0 <- x1
        } 
}
```
上面的循环对于初学者不友好，容易陷入死循环。最好设置迭代次数限制（例如使用for循环），然后打印是否得到预期结果。

* * *

## next, return

`next`用来跳过本次循环。

```source-r
for(i in 1:100) {
        if(i <= 20) {
                ## 跳过前20次循环
                next 
        }
        ## Do something here
}
```

`return` 表示一个函数的结果同时返回函数的运算结果。

* * *

## 小结
*    R语言中`if`、`while`和`for` 循环和判断结构。
*   避免死循环，即使在理论上是合理的。
*   控制结构语句对R编程十分有用；对于R的交互式命令行操作，*apply家族的函数更高效。
