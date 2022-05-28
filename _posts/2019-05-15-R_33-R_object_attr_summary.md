---
layout: post
title: "R语言33-R语法扩展: R对象属性的函数小结"
date:   2019-05-15
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

> 任何编程语言都会包含有两个最基本的概念：数据类型和数据结构
数据类型指的是数值、字符串、逻辑值及他们衍生出的各种复合类型
数据结构描述各种数据类型所组成的对象是如何组织的

数据类型和数据结构包含两个层次：物理层次和逻辑层次
* 类似于硬盘分区中的物理分区和逻辑分区的区别，对于一块硬盘，可以用磁盘管理工具可以分成多个分区来储存数据，这些分区被称为逻辑分区；但无论我们分成多少分区，他们都同属于一个物理分区
* 物理层次：计算机物理内存上是如何存储的数据类型和结构
* 逻辑层次：不同编程语言本身定义的基本数据类型和结构，以及用户自定义的各种数据类型，这是高级编程语言对底层计算机物理储存的一种高级封装，使得编程语言更加接近于自然语言。
* 在面向对象的编程中，被封装成各种类和对象。

## 查看数据类型class、mode和typeof
R的许多语法都是继承自S语言，R语言的开发时间也比较早，许多现在的程序概念在当时并没有被提出。
mode和storage.mode函数描述早期程序的数据类型，继承自S语言，更接近与内存储存的数据类型，storage.mode的描述更准确一些。mode和storage.mode的返回值基于typeof函数的结果。
typeof函数描述最新的、更准确的数据类型，其返回值的结果和storage.mode一样，在进行程序编译时比较重要。
class函数描述的是R语言定义的类属性，如向量、列表、数据框等，可以由用户自定义。类是面向对象编程中的概念，如Biostrings包中定义的一个DNAstring类。
*总结*：mode、storage.mode和typeof描述的数据类型存储的物理属性，对象所储存的数据类型改变时发生改变；class描述的是数据的存储的逻辑层次属性，默认与物理属性相同，可以由用户自定义修改，但不影响对象所储存的实际数据类型。
```R
> x<- 1
> mode(x)
[1] "numeric"
> class(x)
[1] "numeric"
> typeof(x)
[1] "double"
> class(x)<- "subclass"
> class(x)
[1] "subclass"
> mode(x)
[1] "numeric"
> typeof(x)
[1] "double"
# 如果用户修改的名称是R的内置数据类型名称，则数据类型发生改变
> class(x) <- "character"
> x
[1] "1"
> class(x)
[1] "character"
> mode(x)
[1] "character"
> typeof(x)
[1] "character"
```

比较不同R对象的属性函数返回值的比较
```R
library(methods)
library(dplyr)
library(xml2)
library(Biostrings)

setClass("dummy", representation(x="numeric", y="numeric"))

types <- list(
  "logical vector" = logical(),
  "integer vector" = integer(),
  "numeric vector" = numeric(),
  "complex vector" = complex(),
  "character vector" = character(),
  "raw vector" = raw(),
  factor = factor(),
  "logical matrix" = matrix(logical()),
  "numeric matrix" = matrix(numeric()),
  "logical array" = array(logical(8), c(2, 2, 2)),
  "numeric array" = array(numeric(8), c(2, 2, 2)),
  list = list(),
  pairlist = .Options,
  "data frame" = data.frame(),
  "closure function" = identity,
  "builtin function" = `+`,
  "special function" = `if`,
  environment = new.env(),
  null = NULL,
  formula = y ~ x,
  expression = expression(),
  call = call("identity"),
  name = as.name("x"),
  "paren in expression" = expression((1))[[1]],
  "brace in expression" = expression({1})[[1]],
  "S3 lm object" = lm(dist ~ speed, cars),
  "S4 dummy object" = new("dummy", x = 1:10, y = rnorm(10)),
  "external pointer" = read_xml("<foo><bar /></foo>")$node,
  "DNAString" = DNAString(),
  "RNAString" = RNAString(),
  "AAString" = AAString()
)

type_info <- Map(
  function(x, nm)
  {
    tibble(
      "spoken type" = nm,
      class = class(x),
      mode  = mode(x),
      typeof = typeof(x),
      storage.mode = storage.mode(x)
    )
  },
  types,
  names(types)
) %>% bind_rows

knitr::kable(type_info)
```
输出：
```R
|spoken type         |class       |mode        |typeof      |storage.mode |
|:-------------------|:-----------|:-----------|:-----------|:------------|
|logical vector      |logical     |logical     |logical     |logical      |
|integer vector      |integer     |numeric     |integer     |integer      |
|numeric vector      |numeric     |numeric     |double      |double       |
|complex vector      |complex     |complex     |complex     |complex      |
|character vector    |character   |character   |character   |character    |
|raw vector          |raw         |raw         |raw         |raw          |
|factor              |factor      |numeric     |integer     |integer      |
|logical matrix      |matrix      |logical     |logical     |logical      |
|numeric matrix      |matrix      |numeric     |double      |double       |
|logical array       |array       |logical     |logical     |logical      |
|numeric array       |array       |numeric     |double      |double       |
|list                |list        |list        |list        |list         |
|pairlist            |pairlist    |pairlist    |pairlist    |pairlist     |
|data frame          |data.frame  |list        |list        |list         |
|closure function    |function    |function    |closure     |function     |
|builtin function    |function    |function    |builtin     |function     |
|special function    |function    |function    |special     |function     |
|environment         |environment |environment |environment |environment  |
|null                |NULL        |NULL        |NULL        |NULL         |
|formula             |formula     |call        |language    |language     |
|expression          |expression  |expression  |expression  |expression   |
|call                |call        |call        |language    |language     |
|name                |name        |name        |symbol      |symbol       |
|paren in expression |(           |(           |language    |language     |
|brace in expression |{           |call        |language    |language     |
|S3 lm object        |lm          |list        |list        |list         |
|S4 dummy object     |dummy       |S4          |S4          |S4           |
|external pointer    |externalptr |externalptr |externalptr |externalptr  |
|DNAString           |DNAString   |S4          |S4          |S4           |
|RNAString           |RNAString   |S4          |S4          |S4           |
|AAString            |AAString    |S4          |S4          |S4           |
```
## 查看数据结构的函数--str和attributes函数
str和summary函数类似，str是structure的缩写，主要是查看当前对象的数据结构的描述，无法修改对象的数据结构，当对象储存的数据结构被修改时，str函数的返回值发生变化。
attributes描述的是R对象逻辑层次的数据结构，可以修改，可以由用户自定义，修改之后立即生效，但不影响R对象储存在内存的内容，但会影响对象的数据结构，str的函数返回值会发生改变，mostattributes可以设置更复杂的数据结构。
```R
## 查看R语言内置的数据结构
> x <- cbind(a = 1:3, pi = pi)
> x
     a       pi
[1,] 1 3.141593
[2,] 2 3.141593
[3,] 3 3.141593
# 使用attributes查看对象x的逻辑数据结构
> attributes(x)
$dim
[1] 3 2

$dimnames
$dimnames[[1]]
NULL

$dimnames[[2]]
[1] "a"  "pi"

# 使用str查看对象x的物理数据结构
> str(x)
 num [1:3, 1:2] 1 2 3 3.14 3.14 ...
 - attr(*, "dimnames")=List of 2
  ..$ : NULL
  ..$ : chr [1:2] "a" "pi"
# 使用attributes修改对象x的数据结构
> attributes(x) <- NULL
> x
[1] 1.000000 2.000000 3.000000 3.141593 3.141593 3.141593
# 修改后，逻辑的数据结构变成NULL
> attributes(x)
NULL
# str函数返回逻辑层级的数据结构消失时，数据在物理储存的数据结构
> str(x)
 num [1:6] 1 2 3 3.14 3.14 ...
# 使用mostattributes函数还原对象x原来的数据结构
> mostattributes(x) <- list(dim = 3:2, dimnames = list(NULL,c("a","pi")), names = paste(1:6))
> x
     a       pi
[1,] 1 3.141593
[2,] 2 3.141593
[3,] 3 3.141593
> attributes(x)
$dim
[1] 3 2

$dimnames
$dimnames[[1]]
NULL

$dimnames[[2]]
[1] "a"  "pi"


> str(x)
 num [1:3, 1:2] 1 2 3 3.14 3.14 ...
 - attr(*, "dimnames")=List of 2
  ..$ : NULL
  ..$ : chr [1:2] "a" "pi"

# 查看一个函数的数据结构
> str(numeric)
function (length = 0L) 
```
## is()及is类函数的简单讲解
is.*()的一类函数可以判断对象是否是某一种特定的数据类型或数据结构
is()可以查看该对象所属的所有数据类型或数据结构
```R
> x <- cbind(a = 1:3, pi = pi)
# 使用is.*()类函数判断对象是否属于该数据类型或数据结构，并返回逻辑值
> is.numeric(x)
[1] TRUE
> is.character(x)
[1] FALSE
> is.vector(x)
[1] FALSE
> is.matrix(x)
[1] TRUE
> is.array(x)
[1] TRUE
> is.data.frame(x)
[1] FALSE
# 查看对象x所属的全部类型
> is(x)
[1] "matrix"           "array"            "structure"        "vector"          
[5] "vector_OR_factor"
```
可以通过编写一个函数来查看一个对象的数据类型或数据结构

```R
what.is <- function(x, show.all=FALSE) {

  # set the warn option to -1 to temporarily ignore warnings
  op <- options("warn")
  options(warn = -1)
  on.exit(options(op))

  list.fun <- grep(methods(is), pattern = "<-", invert = TRUE, value = TRUE)
  result <- data.frame(test=character(), value=character(), 
                       warning=character(), stringsAsFactors = FALSE)

  # loop over all "is.(...)" functions and store the results
  for(fun in list.fun) {
    res <- try(eval(call(fun,x)),silent=TRUE)
    if(class(res)=="try-error") {
      next() # ignore tests that yield an error
    } else if (length(res)>1) {
      warn <- "*Applies only to the first element of the provided object"
      value <- paste(res,"*",sep="")
    } else {
      warn <- ""
      value <- res
    }
    result[nrow(result)+1,] <- list(fun, value, warn)
  }

  # sort the results
  result <- result[order(result$value,decreasing = TRUE),]
  rownames(result) <- NULL

  if(show.all)
    return(result)
  else
    return(result[which(result$value=="TRUE"),])
}
```
查看函数的输出
```R
> what.is(1)
        test value warning
1  is.atomic  TRUE        
2  is.double  TRUE        
3  is.finite  TRUE        
4 is.numeric  TRUE        
5  is.vector  TRUE        
> what.is(x)
        test value warning
2   is.array  TRUE        
3  is.atomic  TRUE        
4  is.double  TRUE        
5  is.matrix  TRUE        
6 is.numeric  TRUE 
> what.is(numeric)
          test value warning
1  is.function  TRUE        
2 is.recursive  TRUE 
```
以上是本人对R语言对象属性的函数的区别和应用的一些粗浅的理解，由于每个人背景不同，理解也不同，希望大家一起来完善，一起学好R语言！！
## 参考链接
* [https://stat.ethz.ch/R-manual/R-devel/doc/manual/R-lang.html](https://stat.ethz.ch/R-manual/R-devel/doc/manual/R-lang.html)
* [https://stackoverflow.com/questions/6258004/types-and-classes-of-variables](https://stackoverflow.com/questions/6258004/types-and-classes-of-variables)
* [https://stackoverflow.com/questions/8855589/a-comprehensive-survey-of-the-types-of-things-in-r-mode-and-class-and-type](https://stackoverflow.com/questions/8855589/a-comprehensive-survey-of-the-types-of-things-in-r-mode-and-class-and-type)

课程分享
生信技能树全球公益巡讲
（https://mp.weixin.qq.com/s/E9ykuIbc-2Ja9HOY0bn_6g）
B站公益74小时生信工程师教学视频合辑
（https://mp.weixin.qq.com/s/IyFK7l_WBAiUgqQi8O7Hxw）
招学徒：
（https://mp.weixin.qq.com/s/KgbilzXnFjbKKunuw7NVfw）
