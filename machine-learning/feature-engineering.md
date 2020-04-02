---
title: Feature Engineering
date: 2019-07-22
mathjax: true
categories:
  - ml
tag: 
  - ml
  - hexo-asset-image
---

Feature Engineering
-------------------------------

# 1. 数据预处理

**sklearn.preprocessing**

```python
from sklearn.preprocessing import StandardScaler
StandardScaler().fit_transform(iris.data)
```



| 类                  |                                                              | 功能               | 说明                                                         |
| ------------------- | ------------------------------------------------------------ | ------------------ | ------------------------------------------------------------ |
| StandardScaler      | $x'=\frac{x-\overline{x}}{\sigma}$                           | 标准化(无量纲化)   | 基于特征矩阵的列，将特征值转换至服从标准正态分布 $x ~ \mathcal{N}[0,1]$ |
| MinMaxScaler        | $x'=\frac{x-min}{max-min}$                                   | (区间缩放)无量纲化 | 基于最大最小值，将特征值转换到[0, 1]区间上                   |
| Normalizer          | $x'=\frac{x}{\sqrt{\sum_{j} x_j^2}}$                         | 归一化             | 基于特征矩阵的行，将样本向量转换为“单位向量”                 |
| Binarizer           | $x'=\begin{cases}1, x>threshhold \\ 0, x \leq threshold\end{cases}$ | 二值化             | 基于给定阈值，将定量特征按阈值划分                           |
| OneHotEncoder       | 000100,001000,...                                            | 哑编码(dummy)      | 将定性数据编码为定量数据                                     |
| Imputer             |                                                              | 缺失值计算(nan)    | 计算缺失值，缺失值可填充为均值等                             |
| PolynomialFeatures  | $(x_1,x_2)=(1,x_1^2,x_2^2,x_1x_2)$                           | 多项式数据转换     | 多项式数据转换                                               |
| FunctionTransformer | FunctionTransformer(log1p).fit_transform(iris.data)          | 自定义单元数据转换 | 使用单变元的函数来转换数据                                   |



# 2. 特征选择

| 类                | 所属方式 | 说明                                                   |
| ----------------- | -------- | ------------------------------------------------------ |
| VarianceThreshold | Filter   | 方差选择法                                             |
| SelectKBest       | Filter   | 可选关联系数、卡方校验、最大信息系数作为得分计算的方法 |
| RFE               | Wrapper  | 递归地训练基模型，将权值系数较小的特征从特征集合中消除 |
| SelectFromModel   | Embedded | 训练基模型，选择权值系数较高的特征                     |

# 3.降维

PCA是为了让映射后的样本具有最大的发散性；而LDA是为了让映射后的样本有最好的分类性能。所以说PCA是一种无监督的降维方法，而LDA是一种有监督的降维方法。

| 库            | 类   | 说明           |
| ------------- | ---- | -------------- |
| decomposition | PCA  | 主成分分析法   |
| lda           | LDA  | 线性判别分析法 |



References:

1. <https://www.cnblogs.com/jasonfreak/p/5448385.html>
2. <https://sklearn.apachecn.org/#/docs/>
3. 