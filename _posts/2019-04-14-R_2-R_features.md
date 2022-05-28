---
layout: post
title: "R语言2-R简介：R语言定义与特征"
date:   2019-04-14
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

> R是一个开放的统计编程环境，是一门用于统计计算和作图的语言。
R is a language and environment for statistical computing and graphics.
http://www.r-project.org/about.html

## R特征
*   语法接近S语言，方便S_PLUS用户使用。
*   语义表面上与S类似，但实际上是不同的。 
*   全平台/系统运行。 (包括PlayStation 3)
*   定期更新（每年修复bug）；开发积极。
*   极其精简，就像一个软件；函数封装在不同的扩展包中。
*   绘图能力十分强大，强于大多数统计学软件。
*   便于交互工作，但包含开发新工具的强大编程语言
*   积极活跃的用户社区；R-help和R-devel mailing lists、Stack Overflow
*   免费

## 免费软件
With *free software*, you are granted
*   The freedom to run the program, for any purpose (freedom 0).
*   The freedom to study how the program works, and adapt it to your needs (freedom 1). Access to the source code is a precondition for this.
*   The freedom to redistribute copies so you can help your neighbor (freedom 2).
*   The freedom to improve the program, and release your improvements to the public, so that the whole community benefits (freedom 3). Access to the source code is a precondition for this.
[http://www.fsf.org](http://www.fsf.org/)

* * *

## R缺点
*   基于40年前的技术开发。
*   对动态或3D图形支持少（“old days”版本后有所提升）。
*   函数开发处于用户需求和贡献。如果没人和你有相同需求，那么这可能就是你的工作（或者付费找人开发）。
*   对象必须储存在物理内存中，但现在有很多新技术可以实现。
*   不能应对所有可能的情况 (这也是所有软件的弊端)。


## R系统的设计
R系统分成两个概念模块：
1.  从CRAN下载的“base” R系统。 
2.  其他。
R函数分别储存在不同的*packages*中。
*   “base” R系统包括：  **base** package是运行R所必须的，包含了多多树基础函数。
*    “base”系统中的其他packages：**utils**, **stats**, **datasets**, **graphics**, **grDevices**, **grid**, **methods**, **tools**, **parallel**, **compiler**, **splines**, **tcltk**, **stats4**。
*  建议安装的packages: **boot**, **class**, **cluster**, **codetools**, **foreign**, **KernSmooth**, **lattice**, **mgcv**, **nlme**, **rpart**, **survival**, **MASS**, **spatial**, **nnet**, **Matrix**.

还有很多其他的扩展包：

*   14086 CRAN packages.
*   Bioconductor project packages ([http://bioconductor.org](http://bioconductor.org/)).
*   还有一些自己开发的扩展包存放在github或其他代码托管平台，甚至是个人网站上，没有办法可靠统计这些包的数量。
