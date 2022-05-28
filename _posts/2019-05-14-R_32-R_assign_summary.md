---
layout: post
title: "R语言32-R语法扩展: R赋值符小结"
date:   2019-05-14
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## ->和<-、->>和<<-的区别
* ->和<-、->>和<<-的区别是赋值方向，->、->>从左向右赋值，<-、<<-、=从右向左赋值
* ->、<-和->>、<<-的区别在于作用域范围，->>、<<-是全局作用域，而->、<-是声明局部（函数内部）作用域，下面举个例子
```R
# 声明一个函数
new_counter <- function() {
# 初始化两个变量值为0
  i <- 0
  j <- 0
function() {
# 分别使用<-和<<-进行自加运算
  i <<- i + 1
  j <- j + 1
# 打印两个变量
  print(i)
  print(j)
  }
}
```
通过该函数来探索<-和<<-的区别
```R
# 声明一个函数对象
> counter <- new_counter()
# 第一次执行，结果都为1
> counter()
[1] 1
[1] 1
# <<-赋值的变量值为2，<-赋值的变量值仍为1
> counter()
[1] 2
[1] 1
> counter()
[1] 3
[1] 1
> counter()
[1] 4
[1] 1
......
```
## <-和=的区别
* 在命令行赋值时没有太大区别
```R
> x <- cbind(a = 1:3, pi = pi)
> x
     a       pi
[1,] 1 3.141593
[2,] 2 3.141593
[3,] 3 3.141593
> x[1,1]<- 2
> x[2,1]=3
> x
     a       pi
[1,] 2 3.141593
[2,] 3 3.141593
[3,] 3 3.141593

```
* <-创建的变量的作用域是全局，而=仅仅在一个局部环境。
第一个例子：
```R
> rm(x)
> mean(x=1:10)
[1] 5.5
> x
Error: object 'x' not found
> mean(x <- 1:10)
[1] 5.5
> x
 [1]  1  2  3  4  5  6  7  8  9 10
```
第二个例子：
```R
# 删除当前环境下的nrow对象
> rm(nrow)
# 使用<-赋值声明一个矩阵
> matrix(1,nrow<-2)
     [,1]
[1,]    1
[2,]    1
# 查看当前的nrow值
> nrow
[1] 2
# 删除当前环境下的nrow对象
> rm(nrow)
# 使用=赋值声明一个矩阵
> matrix(1,nrow=2)
     [,1]
[1,]    1
[2,]    1
# 此时nrow没有被赋值，显示的是R语言自带的nrow函数
> nrow
standardGeneric for "nrow" defined from package "base"

function (x) 
standardGeneric("nrow")
<environment: 0x7fa98c7dc2a8>
Methods may be defined for arguments: x
Use  showMethods("nrow")  for currently available ones.
```
第三个例子：
```R
> l <- list(x<-9, y=10)
> x
[1] 9
> y
Error: object 'y' not found
# 可以通过$富豪引用局部变量
> l$y
[1] 10
```
* 当在函数中，将实参通过<-传递给形参时，会在当前环境中创建一个以形参为名的变量，前提是该形式参数在函数体内部被调用。
```R
# 将a赋值为1
> a <- 1
# 生成一个函数，设置该函数的形式参数名为a，
# 注意函数体内未调用形参啊
> f <- function(a) return(TRUE)
# 将a+1作为实际参数赋值给形式参数a
> f <- f(a <- a + 1); 
# a <- a + 1没有被执行，a的值并没有变化
> a
[1] 1
> f
[1] TRUE

#  声明一个函数，注意函数体内调用形式参数a
> f <- function(a) return(a)
# 将a+1作为实际参数赋值给形式参数a
> f <- f(a <- a + 1); 
# a <- a + 1被执行，a值改变
> a
[1] 2
```
## 总结
* 多数使用<-，但当给函数参数传值、通过函数创建不同对象及对行/列/属性命名的时候用=
* 使用list函数创建列表后，在函数参数传递时使用=赋值，可以使用生成的list对象$形参名查看
* 如果在函数传值使用->，可能会覆盖与当前环境中与行参同名的变量，还会造成当前环境变量的混乱
* <<-和>>-一般用不到


## 参考文献
- [https://stackoverflow.com/questions/6140694/is-there-a-technical-difference-between-and](https://stackoverflow.com/questions/6140694/is-there-a-technical-difference-between-and)
- [https://www.r-bloggers.com/assignment-operators-in-r-%E2%80%98%E2%80%99-vs-%E2%80%98-%E2%80%99/](https://www.r-bloggers.com/assignment-operators-in-r-%E2%80%98%E2%80%99-vs-%E2%80%98-%E2%80%99/)
- [https://renkun.me/2014/01/28/difference-between-assignment-operators-in-r/](https://renkun.me/2014/01/28/difference-between-assignment-operators-in-r/)
- [https://stackoverflow.com/questions/2271575/whats-the-difference-between-and-in-r](https://stackoverflow.com/questions/2271575/whats-the-difference-between-and-in-r)
- [https://www.cnblogs.com/xuanlvshu/p/5493222.html](https://www.cnblogs.com/xuanlvshu/p/5493222.html)
