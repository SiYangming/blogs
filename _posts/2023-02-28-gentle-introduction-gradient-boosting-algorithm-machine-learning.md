---
layout: post
title: "梯度提升的正式介绍"
date:   2023-02-28
tags: [转载,机器学习,AI,人工智能,python,xgboost]
comments: true
author: Jason Brownlee
translator: Si Yangming
toc : true
---

## 简介

梯度提升是建立预测模型的最强大技术之一。在本教程中，你将发现梯度提升机器学习 算法，并对它的来源和工作原理有一个正式的介绍。阅读本教程后，你将知道

* 从学习理论和AdaBoost看提升的起源。  
* 梯度提升是如何工作的，包括损失函数、弱学习者和添加模型。
* 如何用各种正则化方案在基础算法上提高性能。

用我的新书《[XGBoost With Python](https://machinelearningmastery.com/xgboost-with-python/)》来启动你的项目，包括分步骤的教程和所有例子的Python源代码文件。

让我们开始吧。

## 提升的起源

提升的想法来自于一个弱小的学习者是否可以被改造得更好。

迈克尔-卡恩斯(Michael Kearns)将这一目标表述为 " 假设提升问题 " ( Hypothesis Boosting Problem ) ， 并从实践的角度指出这一目标。

> > … an efficient algorithm for converting relatively poor hypotheses into very good hypotheses
> >
> > ..将相对较差的假说转化为非常好的假说的一种有效算法
>
> — [Thoughts on Hypothesis Boosting](https://www.cis.upenn.edu/~mkearns/papers/boostnote.pdf) [PDF], 1988

一个弱假说或弱学习者被定义为其表现至少比随机机会略好。

这些想法建立在Leslie Valiant关于无分布或概率近似正确（[Probably Approximately Correct](https://en.wikipedia.org/wiki/Probably_approximately_correct_learning), PAC）学习的工作之上，这是一个研究机器学习问题复杂性的框架。

假设提升是过滤观察结果的想法，留下那些弱学习者可以处理的观察结果，并专注于开发新的弱学习者来处理剩余的困难观察结果。

> > The idea is to use the weak learning method several times to get a succession of hypotheses, each one refocused on the examples that the previous ones found difficult and misclassified. … Note, however, it is not obvious at all how this can be done
> >
> > 我们的想法是多次使用弱学习方法来获得一系列的假设，每一个假设都重新集中在前一个假设发现困难和错误分类的例子上。[......]但是请注意，这一点并不明显 ，如何才能做到这一点呢?
>
> — [Probably Approximately Correct: Nature’s Algorithms for Learning and Prospering in a Complex World](https://amzn.to/3g4YDva), page 152, 2013

## AdaBoost是第一个提升算法

第一个在应用中获得巨大成功的提升实现是自适应提升（[Adaptive Boosting or AdaBoost](https://machinelearningmastery.com/boosting-and-adaboost-for-machine-learning/)），简称AdaBoost。

> > Boosting refers to this general problem of producing a very accurate prediction rule by combining rough and moderately inaccurate rules-of-thumb.
> >
> > 提升指的是这个一般的问题，即通过结合粗糙和适度不准确的经验法则来产生一个非常准确的预测规则。
>
> — [A decision-theoretic generalization of on-line learning and an application to boosting](http://www.face-rec.org/algorithms/Boosting-Ensemble/decision-theoretic_generalization.pdf) [PDF], 1995

AdaBoost中的弱学习器是具有单一分裂的决策树，由于其短小，被称为决策树桩。

AdaBoost的工作原理是对观察结果进行加权，对难以分类的实例给予更多的权重，而对那些已经处理得很好的实例给予较少的权重。新的弱学习器被依次加入，它们的训练重点是更难的模式。

> > This means that samples that are difficult to classify receive increasing larger weights until the algorithm identifies a model that correctly classifies these samples
> >
> > 这意味着难以分类的样本会得到越来越大的权重，直到算法确定了一个能正确分类这些样本的模型。
>
> — [Applied Predictive Modeling](https://amzn.to/3iFPHhq), 2013

这意味着难以分类的样本会得到越来越大的权重，直到算法确定了一个能正确分类这些样本的模型。

\- 应用预测模型, 2013. 

预测是通过对弱学习者的预测进行多数投票，并根据他们各自的准确性进行加权。AdaBoost算法最成功的形式是用于二元分类问题，被称为AdaBoost.M1。

你可以通过以下博客文章了解更多关于AdaBoost程序的知识：

- [Boosting and AdaBoost for Machine Learning](https://machinelearningmastery.com/boosting-and-adaboost-for-machine-learning/).

## AdaBoost的泛化为梯度提升法

AdaBoost和相关的算法首先被Breiman称为ARCing算法，在统计学框架中被重塑。

> > Arcing is an acronym for Adaptive Reweighting and Combining. Each step in an arcing algorithm consists of a weighted minimization followed by a recomputation of [the classifiers] and [weighted input].
> >
> > Arcing是Adaptive Reweighting and Combining的首字母缩写。弧形算法的每一步都包括加权最小化，然后对\[分类器\]和\[加权输入\]进行重新计算。
>
> — [Prediction Games and Arching Algorithms](https://www.stat.berkeley.edu/~breiman/games.pdf) [PDF], 1997

这个框架被Friedman进一步发展，称为梯度提升机。后来被称为梯度提升或梯度树提升。

该统计框架将提升作为一个数字优化问题，其目标是通过使用类似梯度下降的程序 增加弱学习者来最小化模型的损失。

这类算法被描述为一个阶段性的加法模型。这是因 为每次添加一个新的弱学习者，而模型中现有的弱学习者被冻结并保持不变。

> > Note that this stagewise strategy is different from stepwise approaches that readjust previously entered terms when new ones are added.
> >
> > 请注意，这种分阶段的策略与分步骤的方法不同，后者是在添加新术语时重新调整以前输入的术语。 
>
> — [Greedy Function Approximation: A Gradient Boosting Machine](https://statweb.stanford.edu/~jhf/ftp/trebst.pdf) [PDF], 1999

这种泛化允许使用任意可分的损失函数，将该技术扩展到二元分类问题之外，以支持回归、多类分类等。

## 梯度提升的工作原理

梯度提升涉及三个要素：

1. 一个要优化的损失函数。  
2. 弱学习预测分类器。  
3. 一个加法模型，增加弱学习分类器，使损失函数最小化。

### 1. 损失函数

使用的损失函数取决于被解决的问题的类型。

它必须是可微分的，而且支持许多标准的损失函数，也可以自定义损失函数。

例如，回归可以使用平方误差，分类可以使用对数损失。

梯度提升框架的一个好处是，不必为每个可能要使用的损失函数推导新的提升算法，相反，它是一个足够通用的框架，可以使用任何可分的损失函数。

### 2. 弱学习分类器

决策树被用作梯度提升中的弱学习分类器。

具体来说，回归树是用来输出真实的分割值的， 其输出可以加在一起，允许后续模型的输出被加入，并纠正预测中的残差。

树是以一种贪婪的方式构建的，根据纯度分数(如Gini)选择最佳分割 点，或使损失最小化。

最初，比如在AdaBoost的情况下，使用非常短的决策树，只有一个分割，称为决策树桩。较大的树一般可以使用4-8级。

以特定的方式约束弱学习者是很常见的，比如说最大的层数、节点、分裂或叶子 节点的数量。

这是为了确保学习者保持弱小，但仍能以贪婪的方式构建。

### 3. 加法模型

树是一个一个地添加的，模型中现有的树不被改变。

梯度下降程序被用来最小化添加树 时的误差。

传统上，梯度下降是用来最小化一组参数的，比如回归方程中的系数或神经网络中的权重。在计算误差或损失后，权重被更新以最小化该误差。

取而代之的是参数，我们有弱学习分类器的子模型，或者更确切地说，决策树。在计算了误差损失之后，为了执行梯度下降程序，我们必须在模型中加入一棵树，以减少损失 (即遵循梯度)。我们通过对树进行参数化，然后修改树的参数，并通过 (减少剩余误差损失) 向正确的方向移动。

一般来说，这种方法被称为函数梯度下降或带函数的梯度下降。

> > One way to produce a weighted combination of classifiers which optimizes [the cost] is by gradient descent in function space
> >
> > 产生优化\[成本\]的分类器加权组合的一种方法是通过函数空间中的梯度下降法  
>
> — [Boosting Algorithms as Gradient Descent in Function Space](http://papers.nips.cc/paper/1766-boosting-algorithms-as-gradient-descent.pdf) [PDF], 1999

然后，新树的输出被添加到现有树序列的输出中，以努力纠正或改善模型的最终输出。

一旦损失达到可接受的水平或在外部验证数据集上不再有改善，就会增加固定数量的树或停止训练。

## 对基本梯度提升的改进

梯度提升是一种贪婪的算法，可以迅速过度拟合训练数据集。它可以从正则化方法中受 益，这些方法对算法的各个部分进行惩罚，并通过减少过拟合来提高算法的性能。在这一节中，我们将探讨对基本梯度提升的4项改进。

1. Tree Constraints：树的约束。  
2. Shrinkage：收缩率。  
3. Random sampling：随机抽样。  
4. Penalized Learning：惩罚性学习。

### 树的限制条件

重要的是，弱的学习分类器要有技能，但仍然是薄弱的。有很多方法可以限制树的生长。

一个好的通用启发式方法是，**树的创建越受约束，模型中需要的树就越多，反之，单个树受约束越少，需要的树就越少**。

下面是一些可以对决策树的构建施加的约束。

* **树的数量**，一般来说，向模型中添加更多的树，可以非常缓慢地过度拟合。建议是不断增加树，直到观察到没有进一步的改善。
* **树木深度**，更深的树木是更复杂的树木，更短的树木是首选。一般来说，**4-8级会有更好的效果**
* **节点数或叶子数**，与深度一样，这可以约束树的大小，但如果使用其他约束条件，则不受对称结构的约束。
* 每次拆分的观察数对训练节点的训练数据量提出了一个**最小的约束**，然后才能考虑拆分。
* 对**误差损失的最小**改进是对添加到树上的任何分割的改进的一个约束。 

### 更新权重

每一棵树的预测结果都会依次加在一起。

每棵树对这个总和的贡献可以加权，以减慢算法的学习速度。这种加权被称为收缩率或学习率。

> > Each update is simply scaled by the value of the “learning rate parameter v”
> >
> > 每次更新都被 "学习率参数v "的值简单地缩放。  
>
> — [Greedy Function Approximation: A Gradient Boosting Machine](https://statweb.stanford.edu/~jhf/ftp/trebst.pdf) [PDF], 1999

其效果是，学习速度减慢，反过来需要更多的树被添加到模型中，反过来需要更长的时间来训练，在树的数量和学习速度之间提供一个配置权衡。

> > Decreasing the value of v [the learning rate] increases the best value for M [the number of trees].
> >
> > 降低v\[学习率\]的值会增加M\[树的数量\]的最佳值。  
>
> — [Greedy Function Approximation: A Gradient Boosting Machine](https://statweb.stanford.edu/~jhf/ftp/trebst.pdf) [PDF], 1999

在0.1到0.3范围内的小数值，以及小于0.1的数值是很常见的。 

> > Similar to a learning rate in stochastic optimization, shrinkage reduces the influence of each individual tree and leaves space for future trees to improve the model.
> >
> > 类似于随机优化中的学习率，**收缩减少了每个单独的树的影响**，并为未来的树留下空间以改善模型。
>
> — [Stochastic Gradient Boosting](https://statweb.stanford.edu/~jhf/ftp/stobst.pdf) [PDF], 1999

### 随机梯度提升法

bagging集成学习和随机森林的一个重要观点是允许从训练数据集的子样本中贪婪地创建决策树。 

这个优点也可以用来减少梯度提升模型中序列中的树之间的关联性。

这种提升的变体被称为随机梯度提升。

> > at each iteration a subsample of the training data is drawn at random (without replacement) from the full training dataset. The randomly selected subsample is then used, instead of the full sample, to fit the base learner.
> >
> > 在每个迭代中，从完整的训练数据集中随机抽取一个训练数据的子样本( 无替换)。然后使用随机选择的子样本，而不是全样本，来适应基础学习分类器。
>
> — [Stochastic Gradient Boosting](https://statweb.stanford.edu/~jhf/ftp/stobst.pdf) [PDF], 1999

可以使用的随机提升的几个变种：
* 在创建每一棵树之前，对行进行再抽样。
* 在创建每棵树之前，对列进行子采样
* 在考虑每次拆分之前，对列进行子采样。

一般来说，积极的下抽样，如只选择50%的数据，已被证明是有效的。 

> > According to user feedback, using column sub-sampling prevents over-fitting even more so than the traditional row sub-sampling
> >
> > 根据用户的反馈，使用子列采样甚至比传统的子行采样更能防止过度拟合
>
> — [XGBoost: A Scalable Tree Boosting System](https://arxiv.org/abs/1603.02754), 2016

### 惩罚性梯度提升法

除了参数化树的结构之外，还可以对其施加额外的约束。

像CART这样的经典决策树不 被用作弱学习分类器，而是使用一种被称为回归树的修改形式，在叶子节点(也称为终端节点)上有数值。在一些文献中，树叶中的数值可以被称为权重。

因此，树的叶子的权 重值可以使用流行的正则化函数进行正则化，例如:

-  $L^1$权重的正规化。    
-  $L^2$ 权重的正规化。

> > The additional regularization term helps to smooth the final learnt weights to avoid over-fitting. Intuitively, the regularized objective will tend to select a model employing simple and predictive functions.
> >
> > 额外的正则化项有助于平滑最终学习到的权重，避免过度拟合。直观地说， 正则化目标将倾向于选择一个采用简单和预测功能的模型。
>
> — [XGBoost: A Scalable Tree Boosting System](https://arxiv.org/abs/1603.02754), 2016

## 梯度提升学习资源

梯度提升是一种迷人的算法，我相信你一定想深入了解。

本博客列出了各种资源，你可以用来了解更多关于梯度提升算法的信息。

### 梯度提升视频

- [Gradient Boosting Machine Learning](https://www.youtube.com/watch?v=wPqtzj5VZus), Trevor Hastie, 2014
- [Gradient Boosting](https://www.youtube.com/watch?v=sRktKszFmSk), Alexander Ihler, 2012
- [GBM](https://www.youtube.com/watch?v=WZvPUGNJg18), John Mount, 2015
- [Learning: Boosting](https://www.youtube.com/watch?v=UHBmv7qCey4), MIT 6.034 Artificial Intelligence, 2010
- [xgboost: An R package for Fast and Accurate Gradient Boosting](https://www.youtube.com/watch?v=0IhraqUVJ_E), 2016
- [XGBoost: A Scalable Tree Boosting System](https://www.youtube.com/watch?v=Vly8xGnNiWs), Tianqi Chen, 2016

### 梯度提升图书

- Section 8.2.3 Boosting, page 321, [An Introduction to Statistical Learning: with Applications in R](https://amzn.to/3gYt0V9).
- Section 8.6 Boosting, page 203, [Applied Predictive Modeling](https://amzn.to/3iFPHhq).
- Section 14.5 Stochastic Gradient Boosting, page 390, [Applied Predictive Modeling](https://amzn.to/3iFPHhq).
- Section 16.4 Boosting, page 556, [Machine Learning: A Probabilistic Perspective](https://amzn.to/3iFRTWc)
- Chapter 10 Boosting and Additive Trees, page 337, [The Elements of Statistical Learning: Data Mining, Inference, and Prediction](https://amzn.to/31SA3bt)

### 梯度提升论文

- [Thoughts on Hypothesis Boosting](http://www.cis.upenn.edu/~mkearns/papers/boostnote.pdf) [PDF], Michael Kearns, 1988
- [A decision-theoretic generalization of on-line learning and an application to boosting](http://cns.bu.edu/~gsc/CN710/FreundSc95.pdf) [PDF], 1995
- [Arcing the edge](http://statistics.berkeley.edu/sites/default/files/tech-reports/486.pdf) [PDF], 1998
- [Stochastic Gradient Boosting](https://statweb.stanford.edu/~jhf/ftp/stobst.pdf) [PDF], 1999
- [Boosting Algorithms as Gradient Descent in Function Space](http://maths.dur.ac.uk/~dma6kp/pdf/face_recognition/Boosting/Mason99AnyboostLong.pdf) [PDF], 1999

### 梯度提升课件Slides

- [Introduction to Boosted Trees](http://homes.cs.washington.edu/~tqchen/pdf/BoostedTree.pdf), 2014
- [A Gentle Introduction to Gradient Boosting](http://www.chengli.io/tutorials/gradient_boosting.pdf), Cheng Li

### 梯度提升网页

- [Boosting (machine learning)](https://en.wikipedia.org/wiki/Boosting_(machine_learning))
- [Gradient boosting](https://en.wikipedia.org/wiki/Gradient_boosting)
- [Gradient Tree Boosting in scikit-learn](http://scikit-learn.org/stable/modules/ensemble.html#gradient-tree-boosting)

## 总结

在本文中，你发现了机器学习中用于预测性建模的梯度提升算法。

具体来说，你学会了：

* 学习理论和AdaBoost中提升的历史。
* 梯度提升算法如何与误差（损失）函数、弱（学习者）分类器和加法模型一起工作。
* 如何用正则化来提高梯度提升的性能。

你对梯度提升算法或这篇文章有什么问题吗？请在评论中提出你的问题，我将尽我所能回答。

## 原文链接

https://machinelearningmastery.com/gentle-introduction-gradient-boosting-algorithm-machine-learning/