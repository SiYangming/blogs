---
layout: post
title: "R语言ggplot2绘图扩展包简介和学习资源"
date:   2019-08-05
tags: [R语言,ggplot2,可视化]
comments: true
author: Si Yangming
toc : true
---

ggplot2是RStudio首席科学家Hadley开发用于绘图的R扩展包，ggplot2背后有一套完整的图形语法支持，与传统的纸笔绘图不同，这是一种思维上的提升。

## ggplot2图形语言的一些概念
一张统计图形就是从数据到`几何对象`（geometric object，缩写为geom）的`图形属性`（aesthetic attributes，缩写为aes，包括颜色、形状、大小等）的一个`映射`（mapping）。
`几何对象` (geom) ：点、线、条形、多边形等图形元素
`统计变换` (statistical transformation，缩写为stats) ：对数据的某种汇总
`标度` (scale) ：将数据的取值映射到图形空间，颜色、大小、形状等
`坐标系` (coordinate system，缩写为coord) ：数据是如何映射到图形所在的平面，常见的坐标系有笛卡尔坐标系和极坐标系，笛卡尔坐标系下的条形图对应极坐标系下的饼图，条形图的高对应饼图的角度，通过ggplot2后的图形语法可以很简单地进行转换
`分面` (facet)：如何将数据分解为各个子集，及如何对子集作图展示
`+`：ggplot2中的特殊操作符
## ggplot2帮助系统
- 作者的网页：[http://hadley.nz/](http://hadley.nz/)
- 作者的github：[https://github.com/hadley](https://github.com/hadley)
- google的ggplot2讨论组：[https://groups.google.com/group/ggplot2](https://groups.google.com/group/ggplot2)
- ggplot2的主题系统：[https://github.com/wch/ggplot2/wiki/New-theme-system](https://github.com/wch/ggplot2/wiki/New-theme-system)
- 统计之都ggplot2的GitHub：[https://github.com/cosname/ggplot2-translation](https://github.com/cosname/ggplot2-translation)
- ggplot2官方帮助文档：[https://cran.r-project.org/web/packages/ggplot2/](https://cran.r-project.org/web/packages/ggplot2/)
- ggplot2函数文档：[https://ggplot2.tidyverse.org/reference/](https://ggplot2.tidyverse.org/reference/)
- 程序解答论坛：[http://stackoverflow.com/](http://stackoverflow.com/)
- ggplot2英文版书籍：[https://github.com/hadley/ggplot2-book](https://github.com/hadley/ggplot2-book)
  [http://had.co.nz/ggplot2/](http://had.co.nz/ggplot2/)
- Rstudio的参考卡片：[https://www.rstudio.com/resources/cheatsheets/](https://www.rstudio.com/resources/cheatsheets/)
- 发邮件到作者邮箱hadley@rice.edu
- ggplot2绘图代码示例：http://sthda.com/english/
- 基于ggplot2的扩展包：https://exts.ggplot2.tidyverse.org/gallery/


## 图例向导函数
guide_legend() # 排版
guide_colorbar() # 渐变色

## ggplot2与R中其他绘图扩展包
* 基础图形系统：纸笔模型，在最顶端绘图，不能修改或删除
* 网格图形系统 (grid) ：只能绘制基本图形，无法用于绘制统计图形
* lattice：基于网格图形系统改进，但难以扩展，没有完整的模型支持
* ggplot2：静态绘图系统的集大成者，无法绘制动态图形（GGobi: rggobi绘制动态图形）
* vcd, plotix, gplots：特殊图形
* R官网绘图包汇总：[https://cran.r-project.org/web/views/Graphics.html](https://cran.r-project.org/web/views/Graphics.html)

## ggplot2扩展包的安装
```
install.packages("ggplot2")
```

