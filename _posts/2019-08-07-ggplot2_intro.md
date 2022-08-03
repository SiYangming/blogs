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
  - [http://had.co.nz/ggplot2/](http://had.co.nz/ggplot2/)
  - ggplot2绘图书：[https://ggplot2-book.org/](https://ggplot2-book.org/)
  
- Rstudio的参考卡片：[https://www.rstudio.com/resources/cheatsheets/](https://www.rstudio.com/resources/cheatsheets/)
- 发邮件到作者邮箱hadley@rice.edu
- ggplot2绘图代码示例（Statistical tools for high-throughput data analysis）：[http://sthda.com/english/](http://sthda.com/english/)

## R可视化教程

- R图形示例：[https://r-graph-gallery.com/index.html](https://r-graph-gallery.com/index.html)
- Data Visualization with ggplot2：[https://viz-ggplot2.rsquaredacademy.com/](https://viz-ggplot2.rsquaredacademy.com/)
  - [https://github.com/rsquaredacademy-education/viz-ggplot2/](https://github.com/rsquaredacademy-education/viz-ggplot2/)
- R Graphics：[https://zh.3lib.net/book/600913/34aec1](https://zh.3lib.net/book/600913/34aec1)
- R Graphics Cookbook ：[https://r-graphics.org/](https://r-graphics.org/)

- Lattice: Multivariate Data Visualization with R：[https://zh.3lib.net/book/1240019/03a5c5](https://zh.3lib.net/book/1240019/03a5c5)

- ggplot2:Elegant Graphics for Data Analysis：[https://ggplot2-book.org/](https://ggplot2-book.org/)

- Data Mining with Rattle and R：[https://zh.3lib.net/book/1170958/55bd1b](https://zh.3lib.net/book/1170958/55bd1b)

- [Interactive and Dynamic Graphics for Data Analysis With R and GGobi](https://zh.3lib.net/book/824162/475630)

* 现代统计图形：[https://bookdown.org/xiangyun/msg/](https://bookdown.org/xiangyun/msg/)
* The Grammar of Graphics, Second Edition：[https://zh.book4you.org/book/1049726/5f16e5](https://zh.book4you.org/book/1049726/5f16e5)

## 基于ggplot2的绘图扩展包

基于ggplot2的扩展包：[https://exts.ggplot2.tidyverse.org/gallery/](https://exts.ggplot2.tidyverse.org/gallery/)

ggplot2自从2007年推出以来，成为世界范围内下载最频繁、使用最广泛的R包之一。许多人包括ggplot2的创建人Hadley Wickham将这一成功归功于ggplot2背后的哲学。这个软件包的灵感来源于Leland Wilkinson编写的图形语法一书，在此书中将graphs 分解成scales和layers，并将原始数据与表现形式分离开。

### gganimate

作者： David Robinson

网址 ：[https://www.rdocumentation.org/packages/gganimate](https://www.rdocumentation.org/packages/gganimate)或 [https://github.com/thomasp85/gganimate](https://github.com/thomasp85/gganimate)（新版）

简介： gganimate可以使图片以更加生动形象的动图展示出来，可以直观展示数据的动态变化过程，最后我们可以将动画保存为GIF、视频或动画网页，以便在RStudio或笔记本之外使用。如下面这个例子以动态图展现了历年来诺贝尔获奖者出生地的变化情况，利用gganimate可视化全球范围R-Ladies（R社区性别多样性组织）发展情况一文中有更详细的事例展示如何使用此包。

### ggthemes

作者： Jeffrey B. Arnold

网址： [https://www.rdocumentation.org/packages/ggthemes](https://www.rdocumentation.org/packages/ggthemes)

简介： ggthemes主要作用是提供一些额外的themes、geoms、scales可以让我们快速画出不同主题、背景和配色方案的图片。学术图表基本配色方法

### ggpubr

作者： Alboukadel Kassambara

网址 ：[https://www.rdocumentation.org/packages/ggpubr](https://www.rdocumentation.org/packages/ggpubr)

简介： 要通过ggplot2定制一套图形，尤其是适用于杂志期刊等出版物的图形，对于那些没有深入了解ggplot2的人来说就有点困难了，而ggpubr可轻松绘制出符合出版物要求的图形。

### patchwork

作者： Thomas Pedersen

网址： [https://www.rdocumentation.org/packages/patchwork](https://www.rdocumentation.org/packages/patchwork)

简介： 平常我们绘制图形的时候常常要将几幅图形组合在一起，而ggplot2本身没有强大的拼图语法，这时利用patchwork扩展包，使用几个简单的如/、+、*、^等符号就可以轻松实现拼图这件事。还有其它包也可以做类似事情，具体见ggplot2学习笔记之图形排列。

### ggridges

作者： Claus O. Wilke

网址： [https://www.rdocumentation.org/packages/ggridges](https://www.rdocumentation.org/packages/ggridges)

简介： ggridges包主要用来绘制山峦图。尤其是针对时间或者空间分布可视化具有十分好的效果。

### ggdendro

作者： Andrie de Vries

网址： [https://www.rdocumentation.org/packages/ggdendro](https://www.rdocumentation.org/packages/ggdendro)

简介： ggdendro有几个函数可用来提取树状图数据，可以保存或操作数据本身。旋转你的树状图、删除网格背景、倒转scale，画三角线段，创建diana和Agnes聚类图，等等。结合dendextend和ape包来完全控制你的树状图。

### ggmap

作者： David Kahle

网址： [https://www.rdocumentation.org/packages/ggmap](https://www.rdocumentation.org/packages/ggmap)

简介： ggmap包整合了四种地图资源，分别是Google、OpenStreetMaps、Stamen，它使gplot2的所有geoms都可以用于地图可视化，可以在地图上绘制等高线图或散点图。

### ggrepel

作者： Kamil Slowikowski

网址： [https://cran.r-project.org/web/packages/ggrepel](https://cran.r-project.org/web/packages/ggrepel)

简介： 当我们在图形中添加标签时，标签之间很容易相互重叠，ggrepel包可以解决这个问题，具体见ggrepel使用。

### ggcorrplot

作者： Alboukadel Kassambara

网址： [https://github.com/kassambara/ggcorrplot]( https://github.com/kassambara/ggcorrplot)

简介： ggcorrart是受corrplot包的启发，但它的构建是为了与ggplot2一起使用，这就意味着有很多东西可以让我们控制矩阵的外观，从改变颜色、形状或大小(如下面的圆形矩阵)，到添加系数标签，根据层次聚类重新排列矩阵等等，具体见 ggcorplot使用。

### ggradar

作者： Ricardo Bion

网址： [https://github.com/ricardo-bion/ggradar]( https://github.com/ricardo-bion/ggradar)

简介： 雷达图又叫戴布拉图、蜘蛛网图，通常在财务报表分析中使用较多。但在描述性统计分析中，雷达图正在被越来越多的人使用，适用于显示三个或更多的维度的变量。

### GGally

作者： Barret Schloerke

网址： [http://ggobi.github.io/ggally/]( http://ggobi.github.io/ggally/)

简介： GGally汇集了几个有用的可视化功能来扩展ggplot2，包括配对图矩阵，散点图矩阵，平行坐标图，生存图，以及绘制网络的几个函数。可以使用GGally快速绘制模型的系数，或者在地图上绘制网络，如下面的图片所示。

### ggiraph

**作者：**David Gohel

网址： [http://davidgohel.github.io/ggiraph]( http://davidgohel.github.io/ggiraph)

简介： ggiraph可以给图片添加高级交互或动画，可以扩展现有的ggplot2条形图、散点图、方框图、地图等，并在悬停时显示数据信息(例如数据值或标签)，如下图所示。

### ggExtra：为ggplot2图形四周添加直方图

网址：[https://github.com/daattali/ggExtra](https://github.com/daattali/ggExtra)

### 网络图

- ggraph：[https://ggraph.data-imaginist.com/](https://ggraph.data-imaginist.com/)
- tidygraph：[https://tidygraph.data-imaginist.com/](https://tidygraph.data-imaginist.com/)



## 生物绘图

### gggenes：基因结构图

* [https://wilkox.org/gggenes/](https://wilkox.org/gggenes/)
* [https://github.com/wilkox/gggenes/](https://github.com/wilkox/gggenes/)

### ggseqlogo：序列logos图

* [https://github.com/omarwagih/ggseqlogo](https://github.com/omarwagih/ggseqlogo)
* 教程：[https://omarwagih.github.io/ggseqlogo/](https://omarwagih.github.io/ggseqlogo/)
* [https://www.bioconductor.org/packages/release/bioc/html/seqLogo.html](https://www.bioconductor.org/packages/release/bioc/html/seqLogo.html)

### ggtranscript：转录本结构图

* [https://dzhang32.github.io/ggtranscript/](https://dzhang32.github.io/ggtranscript/)
* [https://github.com/dzhang32/ggtranscript/](https://github.com/dzhang32/ggtranscript/)

## 拼图软件包

cowplot：[https://wilkelab.org/cowplot/index.html](https://wilkelab.org/cowplot/index.html)

patchwork：[https://patchwork.data-imaginist.com/index.html](https://patchwork.data-imaginist.com/index.html)

## 圈图

[https://gjabel.wordpress.com/2014/06/05/world-cup-players-representation-by-league-system/](https://gjabel.wordpress.com/2014/06/05/world-cup-players-representation-by-league-system/)

circlize（Bioniformatics (2014):btu393.）：[https://github.com/jokergoo/circlize](https://github.com/jokergoo/circlize)

OmicCircos（Cancer informatics 13(2014):13.）：

RCircos（BMC bioinformatics 14(2013):244.APA）：

## 在线画图工具

HiPlot：[https://hiplot-academic.com/](https://hiplot-academic.com/)

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

