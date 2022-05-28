---
layout: post
title: "R语言26-R语法: 异常处理"
date:   2019-05-08
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

有时R会返回一些信息

*   `message`: `message` 函数生成的一般提示或诊断信息
*   `warning`: 有些代码书写不恰当，但不会对结果造成太大影响（可忽略）；函数执行期间的提示信息；通过 `warning` 函数生成警告信息
*   `error`: 程序运行出现错误，运行终止，通过 `stop` 函数输出
*   `condition`: 选项，用于不能预先知道某些结果时；程序员可以创建自定义条件

Warning

```R
> log(-1)
[1] NaN
Warning message:
In log(-1) : NaNs produced

```

* * *

```R
> printmessage <- function(x) {
        if(x > 0)
                print("x is greater than zero")
        else
                print("x is less than or equal to zero")
        invisible(x)
}

> printmessage(1)
[1] "x is greater than zero"

> printmessage(NA)
Error in if (x > 0) print("x is greater than zero") else print("x is less than or equal to zero") : 
  missing value where TRUE/FALSE needed

> printmessage2 <- function(x) {
        if(is.na(x))
                print("x is a missing value!")
        else if(x > 0)
                print("x is greater than zero")
        else
                print("x is less than or equal to zero")
        invisible(x)
}

> x <- log(-1)
Warning message:
In log(-1) : NaNs produced

> printmessage2(x)
[1] "x is a missing value!"
```

* * *

处理函数的报错

*   注意输入与函数的调用
*   哪些输出，信息或结果是正确的
*   注意当前的输出
*   当前输出与预期结果的差异
*   一开始预想的预期结果是否正确
*   什么情况下会再次出现该问题

## R中的Debug调试工具

R自带的调试函数：
*   `traceback`: 当error产生时，调用该函数，输出调用日志
*   `debug`: 将一个函数标记为 “debug” 模式，允许一行一行运行函数就行调试
*   `browser`: 挂起一个函数的运行（无论该函数是否被调用），将该函数加入调试模式
*   `trace`: 将调试代码插入函数的指定位置
*   `recover`: 调整报错的信息，更快地定位函数

这些函数是专门为交互式编程调试设计，将这些调试工具封装为函数。含有一些更为直接的方法，通过调用print/cat函数输出提示信息。

* * *

## traceback

```R
> mean(x)
Error in mean(x) : object 'x' not found
> traceback()
1: mean(x)
```

* * *

```R
> lm(y ~ x)
Error in eval(predvars, data, env) : object 'y' not found
> traceback()
7: eval(predvars, data, env)
6: eval(predvars, data, env)
5: model.frame.default(formula = y ~ x, drop.unused.levels = TRUE)
4: stats::model.frame(formula = y ~ x, drop.unused.levels = TRUE)
3: eval(mf, parent.frame())
2: eval(mf, parent.frame())
1: lm(y ~ x)
```

* * *

## 调试

```R
> debug(lm)
> lm(y ~ x)
debugging in: lm(y ~ x)
debug: {
    ret.x <- x
    ret.y <- y
    cl <- match.call()
    mf <- match.call(expand.dots = FALSE)
    m <- match(c("formula", "data", "subset", "weights", "na.action", 
        "offset"), names(mf), 0L)
    mf <- mf[c(1L, m)]
    mf$drop.unused.levels <- TRUE
    mf[[1L]] <- quote(stats::model.frame)
    mf <- eval(mf, parent.frame())
    if (method == "model.frame") 
        return(mf)
    else if (method != "qr") 
        warning(gettextf("method = '%s' is not supported. Using 'qr'", 
            method), domain = NA)
    mt <- attr(mf, "terms")
    y <- model.response(mf, "numeric")
    w <- as.vector(model.weights(mf))
    if (!is.null(w) && !is.numeric(w)) 
        stop("'weights' must be a numeric vector")
    offset <- as.vector(model.offset(mf))
    if (!is.null(offset)) {
        if (length(offset) != NROW(y)) 
            stop(gettextf("number of offsets is %d, should equal %d (number of observations)", 
                length(offset), NROW(y)), domain = NA)
    }
    if (is.empty.model(mt)) {
        x <- NULL
        z <- list(coefficients = if (is.matrix(y)) matrix(NA_real_, 
            0, ncol(y)) else numeric(), residuals = y, fitted.values = 0 * 
            y, weights = w, rank = 0L, df.residual = if (!is.null(w)) sum(w != 
            0) else if (is.matrix(y)) nrow(y) else length(y))
        if (!is.null(offset)) {
            z$fitted.values <- offset
            z$residuals <- y - offset
        }
    }
    else {
        x <- model.matrix(mt, mf, contrasts)
        z <- if (is.null(w)) 
            lm.fit(x, y, offset = offset, singular.ok = singular.ok, 
                ...)
        else lm.wfit(x, y, w, offset = offset, singular.ok = singular.ok, 
            ...)
    }
    class(z) <- c(if (is.matrix(y)) "mlm", "lm")
    z$na.action <- attr(mf, "na.action")
    z$offset <- offset
    z$contrasts <- attr(x, "contrasts")
    z$xlevels <- .getXlevels(mt, mf)
    z$call <- cl
    z$terms <- mt
    if (model) 
        z$model <- mf
    if (ret.x) 
        z$x <- x
    if (ret.y) 
        z$y <- y
    if (!qr) 
        z$qr <- NULL
    z
}
Browse[2]> 
Browse[2]> n
debug: ret.x <- x
Browse[2]> ret.x <- x
Browse[2]> n
debug: ret.y <- y
Browse[2]> ret.y <- y
Browse[2]> n
debug: cl <- match.call()
Browse[2]> cl <- match.call()
Browse[2]> n
debug: mf <- match.call(expand.dots = FALSE)
Browse[2]> mf <- match.call(expand.dots = FALSE)
Browse[2]> n
debug: m <- match(c("formula", "data", "subset", "weights", "na.action", 
    "offset"), names(mf), 0L)
# 退出调试模式
Browse[2]> Q
```

* * *

## recover

```R
> options(error = recover)
> read.csv("nosuchfile")
Error in file(file, "rt") : cannot open the connection
In addition: Warning message:
In file(file, "rt") :
  cannot open file 'nosuchfile': No such file or directory

Enter a frame number, or 0 to exit   

1: read.csv("nosuchfile")
2: read.table(file = file, header = header, sep = sep, quote = quote, dec = dec, f
3: file(file, "rt")

Selection: 
Selection: 0
> 
```

* * *

## 小结

*   R有三个主要的显示错误、提示信息的函数，分别为：`message`, `warning`, `error`
    *   程序员的世界只有 `error` ，只有 `error` 需要调试
*   在分析报错的函数时，首先确认问题的可重复性，明确期望结果、输出与期望结果的差异
*   交互式调试工具 `traceback`, `debug`, `browser`, `trace` 和 `recover` 可以帮助判断函数中错误代码
*   调试工具不能取代思考
