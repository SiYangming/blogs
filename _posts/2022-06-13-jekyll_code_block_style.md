---
layout: post
title: "为jekyll中的代码块添加行号"
date:   2022-06-13
tags: [blog]
comments: true
author: Si Yangming
toc : true
---

## 1. 下载安装 `prism.js`

在[官网](https://prismjs.com/download.html)下载`prism.js`和`prism.css`，可以自定义选择主题、支持语言及插件，这里的主题是`Okaidia`。支持的语言没必要选太多，不然体积会比较大，并且最上面最好选择`Minified version`。
网页拉到底下载js和css文件，下载后分别复制到`js`和`css`文件夹即可。

## 2. js和css文件的引入

在`_include/head.html`中添加以下内容

```html
<!-- line number for code block -->
<script src="/js/prism.js"></script>
<link rel="stylesheet" href="/css/prism.css">
```

在`_include/footer.html`中添加：

```html
<!-- line number for code block -->
<script>
var pres = document.getElementsByTagName("pre");
for(var i = 0; i < pres.length; i++){
    var pre = pres[i];
    if(pre.childNodes[0].nodeName == "CODE"){
        pre.setAttribute("class","line-numbers");
    }
}
</script>
```

## 3. 使用

大功告成，只需在markdown文件中使用`~~~`或`````中就可以愉快的显示行号了。

如：

~~~markdown
```cpp
int main(){
    return 0;
}
```
~~~

效果如下：

```cpp
int main(){
    return 0;
}
```

## 注意

1. 对于没有指定语言的代码块，不显示效果，所以无论如何都要指定语言，随便指定都可以，会被解析成 plaintext。
2. 在 Github-Pages 对行内（inline）代码也添加样式，但在本地不会这样，可能是 jekyll 的版本问题。

## 参考文章

1. [使用prismjs实现Jekyll代码语法高亮并显示行号](https://blog.csdn.net/u013961139/article/details/78853544)
2. [为 jekyll 中的代码块添加行号](https://hugueliu.github.io/2019/12/28/add_line_number_for_code_block_in_jekyll/)