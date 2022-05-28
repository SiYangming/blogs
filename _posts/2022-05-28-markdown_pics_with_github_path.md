---
layout: post
title: "解决github pages构建博客markdown文件图片不显示"
date:   2022-05-28
tags: [markdown,github,blog]
comments: true
author: Si Yangming
toc : true
---

这种情况主要是提供的图片路径github无法正确识别，正确的图片路径如下：

# 两种方式

## 项目绝对路径

https://raw.githubusercontent.com/用户名/项目名称/master/图片文件夹/xxx.png

例如：[https://raw.githubusercontent.com/SiYangming/blogs/master/images/avatar.jpg](https://raw.githubusercontent.com/SiYangming/blogs/master/images/avatar.jpg)

## 博客绝对路径

https://githubpages地址/图片文件夹/xxx.png

例如：[https://siyangming.github.io/blogs/images/avatar.jpg](https://siyangming.github.io/blogs/images/avatar.jpg)

参考资料：[https://blog.csdn.net/qq_30684181/article/details/93681734](https://blog.csdn.net/qq_30684181/article/details/93681734)
