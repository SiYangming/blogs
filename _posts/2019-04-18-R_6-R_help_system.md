---
layout: post
title: "R语言6-R简介：R帮助系统"
date:   2019-04-18
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 在线帮助系统

* R官网：[https://www.r-project.org/search.html](https://www.r-project.org/search.html)
* R Package Documentation：A comprehensive index of R packages and documentation from CRAN, Bioconductor, GitHub and R-Forge. [https://rdrr.io/](https://rdrr.io/)
* Rseek: Search several R-related sites and can easily be added to the toolbar of popular browsers. [https://rseek.org/](https://rseek.org/)
* Searchable mail archives: The three mailing lists are provided by Robert King at the University of Newcastle, Australia. [https://tolstoy.newcastle.edu.au/R/](https://tolstoy.newcastle.edu.au/R/)
* R mail list: [https://www.r-project.org/mail.html](https://www.r-project.org/mail.html)
* Nabble R Forum: An innovative search engine for R messages [http://r.789695.n4.nabble.com/](http://r.789695.n4.nabble.com/)
* Bioconductor: Search tools for the analysis and comprehension of high-throughput genomic data. [http://www.bioconductor.org/help/index.html](http://www.bioconductor.org/help/index.html)
* [RDocumentation](https://www.rdocumentation.org/) Search all 17,201 CRAN, Bioconductor and GitHub packages.


*   [R Enterprise Training](https://www.datacamp.com/groups/business)
*   [R package](https://github.com/datacamp/Rdocumentation)
*   [Leaderboard](https://www.rdocumentation.org/trends)
* https://www.rdocumentation.org/login?rdr=%2F

## 本地帮助系统
```R
#### 关于R的基础知识 ####
help.start()


#### 关于R中的函数或关键字符 ####
## 查看sum函数的帮助文档
help(sum)
?sum
## 查看heatmap函数的帮助文档
help(heatmap)
?heatmap
## 查找所有包含histogram函数的扩展包的帮助文档
help("histogram",try.all.packages=T)
## 查找lattice包中的histogram帮助文档
help("histogram",package="lattice")

## R site search
Search help files, manuals, and mailing list archives.
## 搜索http://search.r-project.org网站的帮助页面
RSiteSearch("matlab")

## 找出名字中含有“plot”的函数
apropos("plot")
apropos("plot", mod="function")
## 列出所有在帮助页面含有字符“plot”的函数帮助文档
help.search("plot")
## 得到名为“plot”函数所在的程序包
find("plot")
## 得到名为“plot”函数的自变量列表
args("plot")

## 展示mean函数使用示例
example(mean)
## 显示hist函数使用示例
example("hist")
## 展示数据处理和绘图实例
example("example")
## 展示R语言的各种类型的绘图示例
demo(graphics)
## R 3D绘图示例
demo(persp)

## 查看Biostrings软件包自带的帮助文档
browseVignettes(package = "Biostrings")
system.file(package = "Biostrings")
## 将软件包Biostrings中Rnw格式转换成纯R代码格式
library("tools")
vigSrc = list.files(pattern = "Rnw$",
                    system.file("doc", package = "Biostrings"),
                    full.names = TRUE)
vigSrc
for (v in vigSrc) Stangle(v)
```
