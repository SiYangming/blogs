---
layout: post
title: "解决Jekyll搭建Github Pages博客的表格和公式显示问题"
date:   2022-05-28
tags: [blog,markdown]
comments: true
author: Si Yangming
toc : true
---

# 表格显示问题

直接在_layout文件中的header.html中写上下面的代码：

```yaml
<style>
  table{
    border-left:1px solid #000000;border-top:1px solid #000000;
    width: 100%;
    word-wrap:break-word; word-break:break-all;
  }
  table th{
  text-align:center;
  }
  table th,td{
    border-right:1px solid #000000;border-bottom:1px solid #000000;
  }
</style>
```

# 公式显示问题

直接在_layout文件中的header.html中写上下面的代码：

```yaml
<!-- 数学公式 -->
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
      inlineMath: [['$','$']]
    }
  });
</script>
```

# 参考资料

* 表格显示：[https://blog.csdn.net/sdujava2011/article/details/83692576](https://blog.csdn.net/sdujava2011/article/details/83692576)
* 公式显示：https://www.zhihu.com/question/62114522/answer/312834856
* 
