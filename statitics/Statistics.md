---
title: Statistic
date: 2019-01-06 23:18:46
tags:
mathjax: true
---

## Beyes

$$P(H|D) = \frac{P(H) \cdot P(D|H)}{P(D)}$$

H: hyposis 假设事件，D: data 数据

- $P(H|D)$ : 后验概率, the probability of observing event H given that D is true
- $P(H)$ : 先验概率, the probability of observing event H
- $P(D|H)$: 似然度　
- $P(D)$ :   the probability of data D

例：在判断垃圾邮件的算法中:
  $P(H)$ : 所有邮件中，垃圾邮件的概率。
  $P(D)$ : 出现某个单词的概率。
  $P(D|H)$ : 垃圾邮件中，出现某个单词的概率。
  $P(H|D)$ : 出现某个单词的邮件，是垃圾邮件的概率。



## 概率是已知模型和参数，推数据。统计是已知数据，推模型和参数。

$P(x|\theta)$

如果$\theta$是已知确定的，$x$是变量，这个函数叫做概率函数(probability function)，它描述不同的样本点$x$出现概率是多少。

如果$x$是已知确定的，$\theta$是变量，这个函数叫做似然函数(likelihood function), 它描述对于不同的模型参数$\theta$，出现$x$这个样本点的概率是多少

## 判别模型和生成模型
- 判别模型，判别y类型，学习条件分布$P(y|x)$
- 生成模型，生成y分布，学习联合分布$P(x, y)$，使用贝叶斯定理得出结论$P(y|x)$

两个小朋友判断图片是的动物是狮子还是大象

1. A小朋友画出两只动物，表示图片和狮子更像，**生成模型**
2. B小朋友根据鼻子体型等特征，判断图片是狮子，**判别模型**



References:

1. https://blog.csdn.net/u011508640/article/details/72815981
2. 