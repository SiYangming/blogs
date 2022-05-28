---
layout: post
title: "R语言ggplot2包：qplot简介"
date:   2019-08-06
tags: [R语言,ggplot2,qplot]
comments: true
author: Si Yangming
toc : true
---
qplot是quick plot的缩写，与R系统的基本绘图函数plot类似。

```soure-r
# 查看qplot的帮助文档
?qplot
```
# diamonds数据集
diamonds是ggplot2中的内置数据集，内含54000颗钻石的价格和质量的信息。钻石质量的四个“C”：克拉重量（carat）、切工（cut）、颜色（color）和净度（clarity），五个物理指标：深度（depth）、钻面宽度（table）、x、y和z。
![钻石的测量示意图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240.png)

由于diamonds数据集没有经过很好的预处理，从该数据集中随机提取100个样本。

```source-r
# 让样本可重复
> library(ggplot2)
> set.seed(1410)
> dsmall <- diamonds[sample(nrow(diamonds), 100), ]
```

***
# qplot绘制散点图

> qplot(carat, price, data = diamonds)
![散点图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528104912670.png)
***
> qplot(log(carat), log(price), data = diamonds)
![取log后的散点图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528104921739.png)
***
> qplot(carat, x * y * z, data = diamonds)
![钻石的体积与重量关系](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528104931643.png)

qplot可以自动识别分配不同组别的颜色
> qplot(carat, price, data = dsmall, colour = color)
![不同颜色表示分组](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528104936644.png)
***
> qplot(carat, price, data = dsmall, shape = cut)
![不同形状表示分组](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528104941287.png)
# 调节图片的透明度
> qplot(carat, price, data = diamonds, alpha = I(1/10))
![alpha = I(1/10)](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528104947738.png)
***
> qplot(carat, price, data = diamonds, alpha = I(1/100))
![alpha = I(1/100)](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528104951011.png)
***
> qplot(carat, price, data = diamonds, alpha = I(1/200))
![alpha = I(1/200)](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528104959201.png)
***
* 二维的变量关系：
qplot的参数geom可以设置绘制图形的种类
geom = "point" 散点图
geom = "smooth" 平滑曲线，展示标准误差
geom = "boxplot" 箱线图
geom = "path"和geom = "line"可以在数据点之间连线
* 一维的变量分布
  * 连续变量
  geom = "histogram" 直方图
  geom = "freqpoly" 频率多边图
  geom = "density" 密度曲线
  * 离散变量
  geom = "bar"
# 添加平滑曲线
使用dsmall数据集绘图

> qplot(carat, price, data = dsmall, geom = c("point", "smooth"))
![dsmall平滑曲线与散点图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105004267.png)

使用diamonds数据集绘图

> qplot(carat, price, data = diamonds, geom = c("point", "smooth"))
![diamonds平滑曲线与散点图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105007882.png)

如果不希望显示标准误，设置se = FALSE

> qplot(carat, price, data = diamonds, geom = c("point", "smooth"), se = FALSE)
![不显示误差](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105011090.png)

## 当`geom = "smooth"`时
当样本较小时，默认使`"loess"`，可以通过`?loess`来查看算法的细节，调节span 参数调整曲线的平滑度。
***
> qplot(carat, price, data = dsmall, geom = c("point", "smooth"), span = 0.2)
![span = 0.2](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105014786.png)

> qplot(carat, price, data = dsmall, geom = c("point", "smooth"), span = 1)
![span = 1](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105017814.png)
***
当样本大于1000时，qplot会自动调用 "lm" 算法用于回归模型，"gam" 算法用于广义线性可加模型，"rlm"算法用于稳健回归（robust regression）。


# 箱线图与扰动点图
利用扰动点图和箱线图来考察以颜色为条件的每克拉价格的分布。从左到右，每克拉价格的跨度逐渐减少，但分布的中位数无明显变化。

> qplot(color, price/carat, data = diamonds, geom = "jitter")
![扰动点图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105021891.png)
***
> qplot(color, price/carat, data = diamonds, geom = "boxplot")
![箱线图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105025397.png)


## 改变 alpha 的取值
可以查看数据集中的地方。然而，箱线图依然是一个更好的选择。
> qplot(color, price/carat, data = diamonds, geom = "jitter", alpha = I(1/5))
![image.png](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105029096.png)
***
> qplot(color, price/carat, data = diamonds, geom = "jitter", alpha = I(1/50))
![image.png](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105033360.png)
***
> qplot(color, price/carat, data = diamonds, geom = "jitter", alpha = I(1/200))
![image.png](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105038810.png)


# 直方图与密度曲线图
展示钻石重量的分布。左图使用的是 geom='histogram' 右图使用的是
# geom=' density'。
> qplot(carat, data = diamonds, geom = "histogram")
![钻石重量的分布直方图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105044616.png)

> qplot(carat, data = diamonds, geom = "density")
![钻石重量的分布密度曲线图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105051991.png)


## 调整直方图的组距。
> qplot(carat, data = diamonds, geom = "histogram", binwidth = 1, xlim = c(0, 3))
![binwidth = 1](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105055452.png)

> qplot(carat, data = diamonds, geom = "histogram", binwidth = 0.1, xlim = c(0, 3))
![binwidth = 0.1](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105058779.png)

> qplot(carat, data = diamonds, geom = "histogram", binwidth = 0.01, xlim = c(0, 3))
![binwidth = 0.01](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105101792.png)

## 分组比较变量

> qplot(carat, data = diamonds, geom = "density", colour = color)
![分组密度分布](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105106789.png)

> qplot(carat, data = diamonds, geom = "histogram", fill = color)
![分组直方图分布](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105110635.png)


# 钻石颜色的条形图
钻石颜色分组的计数
> qplot(color, data = diamonds, geom = "bar")
![钻石颜色分组的计数](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105114355.png)
按 weight=carat进行加权，展示了每种颜色的钻石的总重量
> qplot(color, data = diamonds, geom = "bar", weight = carat) + scale_y_continuous("carat")
![按 weight=carat进行加权](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105118818.png)

# 时间序列的线条图和路径图
## 使用 geom = 'line' 绘制线条图
失业人口的比例
> qplot(date, unemploy/pop, data = economics, geom = "line")
![失业人口的比例](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105123358.png)

失业星期数的中位数
> qplot(date, uempmed, data = economics, geom = "line")
![失业星期数的中位数](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105127261.png)
## 使用geom = "path"绘制路径图
重叠在一起的的散点图和路径图
> qplot(unemploy/pop, uempmed, data = economics, geom = c("point", "path"))
![散点图和路径图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105131267.png)


只有路径图，其中年份用颜色进行了展示。
> year <- function(x) as.POSIXlt(x)$year + 1900
qplot(unemploy/pop, uempmed, data = economics, geom = "path", colour = year(date))
![路径图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105135758.png)


# 在一个面板上绘制多个图形

展示以颜色为条件的重量的直方图的频数
> qplot(carat, data = diamonds, facets = color ~ ., geom = "histogram", binwidth = 0.1, 
    xlim = c(0, 3))
![多个频数直方图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105141935.png)
展示以颜色为条件的重量的直方图的频率，频率比较不同分组的分布时，不受样本量大小的影响，规模不同，也可以比较。
> qplot(carat, ..density.., data = diamonds, facets = color ~ ., geom = "histogram", binwidth = 0.1, xlim = c(0, 3))
![多个频率直方图](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105147000.png)
高质量的钻石 (颜色 D) 在小尺寸上的分布是偏斜的，而随着质量的下降，重量的分布会变得越来越平坦。

# 其他参数
* main：设置图形的标题
* xlab, ylab：设置x轴、y轴的标签文字
> qplot(carat, price, data = dsmall, xlab = "Price ($)", ylab = "Weight (carats)", main = "Price-weight relationship")
![设置图形的标题](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105156424.png)

* xlim, ylim：设置x轴和y轴的显示区间
> qplot(carat, price/carat, data = dsmall, ylab = expression(frac(price, carat)), xlab = "Weight (carats)", main = "Small diamonds", xlim = c(0.2, 1))
![设置x轴和y轴的显示区间](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105200419.png)

* log：字符型向量，显示对哪一坐标轴取log，log = "x"对x轴取对数，log = "xy"对x轴和y轴都取对数
> qplot(carat, price, data = dsmall, log = "xy") 
![对x轴和y轴都取对数](https://raw.githubusercontent.com/SiYangming/blogs/master/images/2019-08-06-qplot_intro/1240-20220528105208540.png)
