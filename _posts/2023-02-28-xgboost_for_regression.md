---
layout: post
title: "使用XGBoost进行回归分析"
date:   2019-05-15
tags: [转载,机器学习,AI,人工智能,python,xgboost]
comments: true
author: Jason Brownlee
translator: Si Yangming
toc : true
---

## 引言

极限梯度提升（eXtreme Gradient Boosting, XGBoost）是一个开源库，它提供了梯度提升算法的高效实现。

在其开发和最初发布后不久，XGBoost就成为了常用的方法，并且经常成为机器学习竞赛中一系列问题的获胜方案的关键组成部分。

回归预测建模问题涉及预测一个数值，如美元数额或高度。XGBoost可以直接用于回归预测建模。

在本教程中，你将发现如何在Python中开发和评估XGBoost回归模型。

完成本教程后，你将知道。

* XGBoost是梯度提升的有效实现，可用于回归预测建模。
* 如何使用重复k-fold交叉验证的最佳实践技术来评估一个XGBoost回归模型。
* 如何拟合一个最终模型，并使用它对新数据进行预测。

让我们开始吧。

![XGBoost for Regression](https://machinelearningmastery.com/wp-content/uploads/2021/05/XGBoost-for-Regression.jpg)

## 教程概述

本教程分为三个部分：

1. 极限梯度提升
2. XGBoost回归API
3. XGBoost回归实例

## 极端梯度提升

梯度提升（Gradient Boosting）是指一类集成（ensemble）机器学习算法，可用于分类或回归预测建模问题。

集成算法是由决策树模型构建的。决策树被一个一个地添加到集合体中，并适合于纠正先前模型的预测错误。这是一种被称为 "提升 "的集合机器学习模型。

模型使用任何任意可微分的损失函数和梯度下降优化算法进行拟合。这使得该技术被命名为 "梯度提升"，因为损失梯度在模型拟合过程中被最小化，就像一个神经网络。

关于梯度提升的更多信息，请参见教程：

- [A Gentle Introduction to the Gradient Boosting Algorithm for Machine Learning](https://machinelearningmastery.com/gentle-introduction-gradient-boosting-algorithm-machine-learning/)

极限梯度提升算法，简称XGBoost，是梯度提升算法的有效开源实现。因此，XGBoost是一种算法，一个开源项目，以及一个Python库。

它最初是由陈天琪开发的，并由[陈天琪](https://www.linkedin.com/in/tianqi-chen-679a9856/)和[Carlos Guestrin](https://homes.cs.washington.edu/~guestrin/index.html)在2016年发表的题为 "[XGBoost: A Scalable Tree Boosting System](https://arxiv.org/abs/1603.02754)"。

它被设计成既有计算效率（如快速执行），又有很高的效率，也许比其他开源的实现方式更有效。

使用XGBoost的两个主要原因是执行速度和模型性能。

在分类和回归预测建模问题上，XGBoost在结构化或表格化的数据集上占主导地位。证据是，它是Kaggle竞争性数据科学平台上比赛获胜者的首选算法。

> *Among the 29 challenge winning solutions 3 published at Kaggle’s blog during 2015, 17 solutions used XGBoost. […] The success of the system was also witnessed in KDDCup 2015, where XGBoost was used by every winning team in the top-10.*
>
> 在2015年期间Kaggle的博客上公布的29个挑战赛获胜方案3中，17个方案使用了XGBoost。[...]该系统的成功也在KDDCup 2015中得到了见证，在前十名中，每个获胜团队都使用了XGBoost。
>
> — [XGBoost: A Scalable Tree Boosting System](https://arxiv.org/abs/1603.02754), 2016.

现在我们已经熟悉了XGBoost是什么以及为什么它很重要，让我们仔细看看我们如何在回归预测模型项目中使用它。

## XGBoost回归API

XGBoost可以作为一个独立的库来安装，XGBoost模型可以使用scikit-learn API来开发。

第一步是安装XGBoost库，如果它还没有安装的话。这可以通过大多数平台上的pip python软件包管理器来实现；例如:

```shell
sudo pip install xgboost
```

然后，你可以通过运行下面的脚本来确认XGBoost库已经正确安装并可以使用。

```python
# check xgboost version
# 检查xgboost版本
import xgboost
print(xgboost.__version__)
```

运行该脚本将打印你所安装的XGBoost库的版本。

你的版本应该是相同的或者更高。如果不是，你必须升级你的XGBoost库的版本。

```
1.1.1
```

你有可能在最新版本的库中遇到问题。这不是你的错。

有时，最新版本的库提出了额外的要求，或者可能不太稳定。

如果你在运行上述脚本时确实出现了错误，我建议将其降级到1.0.1（或更低）版本。这可以通过在pip命令中指定要安装的版本来实现，如下所示。

```shell
sudo pip install xgboost==1.0.1
```

如果你需要针对你的开发环境的具体说明，请参见该教程：

- XGBoost安装指南：[XGBoost Installation Guide](https://xgboost.readthedocs.io/en/latest/build.html)


XGBoost库有自己的自定义API，尽管我们将通过scikit-learn包装类使用该方法。[XGBRegressor](https://xgboost.readthedocs.io/en/latest/python/python_api.html#xgboost.XGBRegressor)和[XGBClassifier](https://xgboost.readthedocs.io/en/latest/python/python_api.html#xgboost.XGBClassifier)。这将使我们能够使用scikit-learn机器学习库的全套工具来准备数据和评估模型。

一个XGBoost回归模型可以通过创建一个XGBRegressor类的实例来定义；例如：

```python
...
# create an xgboost regression model
# 创建一个XGBboost回归模型
model = XGBRegressor()
```

你可以向类的构造函数指定超参数值来配置模型。

也许最常配置的超参数是以下这些。

* n_estimators：集合中的树的数量，经常增加，直到看不到进一步的改进。
* max_depth：每个树的最大深度，通常在1到10之间。
* eta：用于加权每个模型的学习率，通常设置为小值，如0.3、0.1、0.01，或更小。
* subsample：每个树中使用的样本（行）数量，设置为0到1之间的数值，通常1.0表示使用所有样本。
* colsample_bytree：每棵树中使用的特征（列）的数量，设置为0到1之间的数值，通常1.0表示使用所有特征。

例如：

```python
...
# create an xgboost regression model
# 创建一个xgboost回归模型
model = XGBRegressor(n_estimators=1000, max_depth=7, eta=0.1, subsample=0.7, colsample_bytree=0.8)
```

好的超参数值可以通过对给定数据集的试验和错误来找到，或者通过系统的实验，如使用网格搜索在一定范围内的值。

在构建模型的过程中使用了随机性。这意味着，每次在相同的数据上运行算法，都可能产生一个略有不同的模型。

当使用具有随机学习算法的机器学习算法时，通过在多次运行或重复交叉验证中平均其性能来评估它们是一个好的做法。在拟合最终模型时，最好是增加树的数量，直到模型的方差在重复评估中减少，或者拟合多个最终模型并对其预测进行平均。

让我们来看看如何为回归开发一个XGBoost集成学习模型。

## XGBoost回归实例

在这一节中，我们将看看我们如何为一个标准的回归预测模型数据集开发一个XGBoost模型。

首先，让我们介绍一个标准的回归数据集。

我们将使用housing数据集。

housing数据集是一个标准的机器学习数据集，包括506行数据，有13个数字输入变量和一个数字目标变量。

使用重复分层的10倍交叉验证的测试法，一个较差的模型可以达到大约6.6的平均绝对误差（[mean absolute error (MAE)](https://machinelearningmastery.com/regression-metrics-for-machine-learning/) ）。一个表现出色的模型在同一测试标准上可以达到约1.9的平均绝对误差。这提供了这个数据集的预期性能的界限。

该数据集涉及预测美国波士顿市郊区的房价，并给出了该房屋的详细信息。

- [Housing Dataset (housing.csv)](https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv)
- [Housing Description (housing.names)](https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.names)

不需要下载数据集；我们将自动下载它作为我们工作实例的一部分。

下面的例子将数据集下载并加载为Pandas DataFrame，并展示了数据集的维度（行列数）和前五行的数据。

```python
# load and summarize the housing dataset
# 加载并汇总住房数据集
from pandas import read_csv
from matplotlib import pyplot
# load dataset
# 加载数据集
url = 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv'
dataframe = read_csv(url, header=None)
# summarize shape
# 汇总出数据框维度（行列数）
print(dataframe.shape)
# summarize first few lines
# 展示前几行数据
print(dataframe.head())
```

运行这个例子确认了506行数据和13个输入变量以及一个数字目标变量（共14个）。我们还可以看到，所有的输入变量都是数字的。

```
(506, 14)
        0     1     2   3      4      5   ...  8      9     10      11    12    13
0  0.00632  18.0  2.31   0  0.538  6.575  ...   1  296.0  15.3  396.90  4.98  24.0
1  0.02731   0.0  7.07   0  0.469  6.421  ...   2  242.0  17.8  396.90  9.14  21.6
2  0.02729   0.0  7.07   0  0.469  7.185  ...   2  242.0  17.8  392.83  4.03  34.7
3  0.03237   0.0  2.18   0  0.458  6.998  ...   3  222.0  18.7  394.63  2.94  33.4
4  0.06905   0.0  2.18   0  0.458  7.147  ...   3  222.0  18.7  396.90  5.33  36.2

[5 rows x 14 columns]
```

接下来，让我们在问题上评估一个带有默认超参数的回归XGBoost模型。

首先，我们可以把加载的数据集分成输入和输出两列，用于训练和评估一个预测模型。

```python
...
# split data into input and output columns
# 把数据分成输入和输出两列
# X, y = data[:, :-1], data[:, -1]
X, y = data.iloc[:, :-1], data.iloc[:, -1]
```

接下来，我们可以用默认配置创建一个模型的实例。

```python
...
# define model
# 定义模型
model = XGBRegressor()
```

我们将使用重复k-fold交叉验证的方法来评估该模型的最优表现，包含3次重复和10次折叠。

这可以通过使用[RepeatedKFold](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RepeatedKFold.html)类来配置评估程序，并调用[cross_val_score()](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html)来使用该程序评估模型并收集模型得分。

模型的性能将使用平均平方误差（MAE）进行评估。注意，在scikit-learn库中，MAE是负数，这样它就可以被最大化了。因此，我们可以忽略符号，假设所有的误差都是正的。

```python
...
# define model evaluation method
# 定义模型评估方法
cv = RepeatedKFold(n_splits=10, n_repeats=3, random_state=1)
# evaluate model
# 评估模型
scores = cross_val_score(model, X, y, scoring='neg_mean_absolute_error', cv=cv, n_jobs=-1)
```

一旦评估完毕，我们就可以报告该模型在用于预测该问题的新数据时的预测性能。

在这种情况下，由于分数是负的，我们可以使用NumPy的[absolute()](https://numpy.org/doc/stable/reference/generated/numpy.absolute.html)函数来使分数变成正的。

然后，我们使用分数分布的平均值和标准差来报告性能的统计汇总，这是另一个好的做法。

```python
...
# force scores to be positive
# 强制将分数变成正数
scores = absolute(scores)
print('Mean MAE: %.3f (%.3f)' % (scores.mean(), scores.std()) )
```

把这些联系起来，在housing回归预测模型问题上评估一个XGBoost模型的完整例子列举如下。

```python
# evaluate an xgboost regression model on the housing dataset
# 在housing数据集上评估一个XGBoost回归模型
from numpy import absolute
from pandas import read_csv
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import RepeatedKFold
from xgboost import XGBRegressor
# load the dataset
# 加载数据集
url = 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv'
dataframe = read_csv(url, header=None)
data = dataframe.values
# split data into input and output columns
# 将数据分成输入和输出两列
X, y = data[:, :-1], data[:, -1]
# define model
# 定义模型
model = XGBRegressor()
# define model evaluation method
# 定义模型评估方法
cv = RepeatedKFold(n_splits=10, n_repeats=3, random_state=1)
# evaluate model
# 评估模型
scores = cross_val_score(model, X, y, scoring='neg_mean_absolute_error', cv=cv, n_jobs=-1)
# force scores to be positive
# 强制分数为正数
scores = absolute(scores)
print('Mean MAE: %.3f (%.3f)' % (scores.mean(), scores.std()) )
```

运行这个例子可以评估housing数据集上的XGBoost回归算法，并报告10倍交叉验证的三次重复的平均MAE。

注意：鉴于算法或评估程序的随机性，或者数字精度的差异，你的结果可能会有所不同（[results may vary](https://machinelearningmastery.com/different-results-each-time-in-machine-learning/)）。考虑将这个例子运行几次，并比较平均结果。

在这个例子中，我们可以看到，该模型取得了约2.1的MAE。

这是一个很好的分数，比基线好，意味着模型有技巧，接近1.9的最佳分数。

```
Mean MAE: 2.109 (0.320)
```

我们可以决定使用XGBoost回归模型作为我们的最终模型，并对新数据进行预测。

这可以通过在所有可用的数据上拟合模型并调用predict()函数，传入新的数据行来实现。

比如说:

```python
...
# make a prediction
# 做一个预测
yhat = model.predict(new_data)
```

我们可以用一个完整的例子来证明这一点，列举如下。

```python
# fit a final xgboost model on the housing dataset and make a prediction
# 在housing数据集上拟合一个最终的xgboost模型并进行预测
from numpy import asarray
from pandas import read_csv
from xgboost import XGBRegressor
# load the dataset
# 加载数据集
url = 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv'
dataframe = read_csv(url, header=None)
data = dataframe.values
# split dataset into input and output columns
# 将数据集分成输入和输出两列
X, y = data[:, :-1], data[:, -1]
# define model
# 定义模型
model = XGBRegressor()
# fit model
# 拟合模型
model.fit(X, y)
# define new data# 
# 定义新的数据
row = [0.00632,18.00,2.310,0,0.5380,6.5750,65.20,4.0900,1,296.0,15.30,396.90,4.98]
new_data = asarray([row])
# make a prediction
# 在新数据上进行预测
yhat = model.predict(new_data)
# summarize prediction
# 汇总预测结果
print('Predicted: %.3f' % yhat)
```

运行这个例子可以拟合模型并对新的数据行进行预测。

注意：鉴于算法或评估程序的随机性，或者数字精度的差异，你的结果可能会有所不同（[results may vary](https://machinelearningmastery.com/different-results-each-time-in-machine-learning/) ）。考虑将这个例子运行几次，并比较平均结果。

在这个例子中，我们可以看到，模型预测的数值大约是24。

## 进阶阅读

如果你想深入了解，本节提供了更多关于该主题的资源。

### 教程

- [Extreme Gradient Boosting (XGBoost) Ensemble in Python](https://machinelearningmastery.com/extreme-gradient-boosting-ensemble-in-python/)
- [Gradient Boosting with Scikit-Learn, XGBoost, LightGBM, and CatBoost](https://machinelearningmastery.com/gradient-boosting-with-scikit-learn-xgboost-lightgbm-and-catboost/)
- [Best Results for Standard Machine Learning Datasets](https://machinelearningmastery.com/results-for-standard-classification-and-regression-machine-learning-datasets/)
- [How to Use XGBoost for Time Series Forecasting](https://machinelearningmastery.com/xgboost-for-time-series-forecasting/)

### 论文

- [XGBoost: A Scalable Tree Boosting System](https://arxiv.org/abs/1603.02754), 2016.

### APIs

- [XGBoost Installation Guide](https://xgboost.readthedocs.io/en/latest/build.html)
- [xgboost.XGBRegressor API](https://xgboost.readthedocs.io/en/latest/python/python_api.html#xgboost.XGBRegressor).
- [sklearn.model_selection.RepeatedKFold API](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RepeatedKFold.html).
- [sklearn.model_selection.cross_val_score API](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html).

## 总结

在本教程中，你发现了如何在Python中开发和评估XGBoost回归模型。

具体来说，你学到了：

- XGBoost是梯度提升的有效实现，可用于回归预测建模。
- 如何使用重复k-fold交叉验证的最佳实践技术来评估一个XGBoost回归模型。
- 如何拟合一个最终模型，并使用它对新数据进行预测。

## 原文链接

https://machinelearningmastery.com/xgboost-for-regression/









