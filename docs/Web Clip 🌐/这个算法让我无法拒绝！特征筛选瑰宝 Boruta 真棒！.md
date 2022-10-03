> created: 2022-01-10T21:38:28
> tags: [特征工程,算法,Python] [[数据分析 - 特征工程]] 
> source: [原文地址](https://zhuanlan.zhihu.com/p/392325156)
> author: 

---

# 这个算法让我无法拒绝！特征筛选瑰宝 Boruta 真棒！ - 知乎

作者：杰少 链接：[这个算法让我无法拒绝--特征筛选瑰宝Boruta。](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/0XbrbrkviVLcDycjHo953)

> 欢迎关注 [@Python与数据挖掘](https://www.zhihu.com/people/59915ce281b6623b6d399c8e4c469a1b)，专注Python、数据分析、数据挖掘、好玩工具！

Boruta 算法是目前非常流行的一种特征筛选方法，其核心是基于两个思想：shadow features和binomial distribution。

它是一个非常聪明的算法，可以追溯到 2010 年，Boruta 可以自动在数据集上执行特征选择。作为 R 的一个包而诞生。目前 Python 的 Boruta 版本是 BorutaPy，[https://github.com/scikit-learn\-contrib/boruta\_py](https://link.zhihu.com/?target=https%3A//github.com/scikit-learn-contrib/boruta_py)。

本文就一起来学习了解一下 Boruta 算法，我们开始吧！

## 1、shadow features

在Boruta中，特征之间并不会相互竞争，相反地，它们会和他们的随机版本相互竞争。实践中，假设我们的特征是X，

-   我们对X中的每个特征进行随机shuffle，这些shuffle过后的特征被称为**shadow features**;

这个时候，我们将shadow dataframe附加到原始dataframe以获得一个新的dataframe（我们称之为X\_boruta），它的特征列数是原先X的两倍。然后我们使用一个模型对其进行训练，得到每个特征的特征重要性。

-   一个特征如果是有用的话，那么它的特征重要性肯定要比它的随机版本要好。

这个时候我们只需要将原始特征中特征重要性高于**随机版本中最高的特征重要性分数**的那些原始特征进行保留即可。但这可能会存在一个问题，这个模型或许训练的不是很好，特征重要性不是很准，于是我们就需要第二个策略。

## 2、binomial distribution

这一步相信大家并不陌生，核心就是**不断迭代**，如果一次存在侥幸，那么我们就迭代多次，比如20次，100次，然后取特重要性的均值，在进行比较筛选。这是最基础的想法，在Boruta中，**特征的最大不确定性水平以50%的概率表示**，就像扔硬币一样。由于每个独立的实验可以给出一个二元结果（命中或不命中），一系列的n个试验遵循二项分布，在python中，二项式分布的概率mass函数可以通过下面方式计算得到：

```
import scipy as sp
trials = 20
pmf = [sp.stats.binom.pmf(x, trials, .5) for x in range(trials + 1)]
```

于是我们得到：

![](https://pic2.zhimg.com/v2-036cb6fa49ad184f10a093939e30dfdd_b.jpg)

其中：

-   红色区域是拒绝区域：这块区域的特征被认为是噪声，因此它们可以被直接丢弃；
-   蓝色区域是犹豫区域：Boruta对这个区域的特征难以抉择；
-   绿色区域是可接受区域：这里的特征被认为是预测性的，一般可以被保留。

## 案例1

```
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from boruta import BorutaPy

# load X and y
# NOTE BorutaPy accepts numpy arrays only, hence the .values attribute
X = pd.read_csv('examples/test_X.csv', index_col=0).values
y = pd.read_csv('examples/test_y.csv', header=None, index_col=0).values
y = y.ravel()

# define random forest classifier, with utilising all cores and
# sampling in proportion to y labels
rf = RandomForestClassifier(n_jobs=-1, class_weight='balanced', max_depth=5)

# define Boruta feature selection method
feat_selector = BorutaPy(rf, n_estimators='auto', verbose=2, random_state=1)

# find all relevant features - 5 features should be selected
feat_selector.fit(X, y)

# check selected features - first 5 features are selected
feat_selector.support_

# check ranking of features
feat_selector.ranking_

# call transform() on X to filter it down to selected features
X_filtered = feat_selector.transform(X)
```

## 案例2

```
from boruta import BorutaPy
from sklearn.ensemble import RandomForestRegressor
import numpy as np
 
rf = RandomForestRegressor(n_jobs = -1, max_depth = 5)
boruta = BorutaPy(
   estimator = rf, 
   n_estimators = 'auto',
   max_iter = 100 # number of trials to perform
)

# 模型训练
boruta.fit(np.array(X), np.array(y))
# 输出结果
green_area = X.columns[boruta.support_].to_list()
blue_area  = X.columns[boruta.support_weak_].to_list()
print('features in the green area:', green_area)
print('features in the blue area:', blue_area)
```

## 参考文献

1.  Kursa M., Rudnicki W., "Feature Selection with the Boruta Package" Journal of Statistical Software, Vol. 36, Issue 11, Sep 2010
2.  [https://github.com/scikit-learn\-contrib/boruta\_py](https://link.zhihu.com/?target=https%3A//github.com/scikit-learn-contrib/boruta_py)
3.  Boruta Explained Exactly How You Wished Someone Explained to You

当然，如果你是刚入门编程的小白，需要夯实基础知识，不用想了，这个Python入门课非常适合你。

## 文章推荐

[太棒了！FaceBook 开源全网第一个时序王器 Kats ！](https://zhuanlan.zhihu.com/p/391897734)

[微软又放大招！网友说：这也太猛了...](https://zhuanlan.zhihu.com/p/389377080)

[太棒了！FaceBook 开源全网第一个时序王器 Kats ！](https://zhuanlan.zhihu.com/p/391897734)

[【建议收藏】必知必会！Python 中最流行的十个标准库！](https://zhuanlan.zhihu.com/p/391518725)

[4 款 Python 数据探索性分析(EDA)工具包，总有一款适合你](https://zhuanlan.zhihu.com/p/372676897)

[时间序列预测的7种Python工具包，总有一款适合你！](https://zhuanlan.zhihu.com/p/385094015)

[超级干货！史上最全数据分析学习路线(附资源下载)](https://zhuanlan.zhihu.com/p/383053860)

[深度盘点 | 整理了47个Python人工智能库](https://zhuanlan.zhihu.com/p/391638964)

[造数能力堪称一绝！Python 中神奇的第三方库 Faker 真棒！](https://zhuanlan.zhihu.com/p/390671216)

___

**整理不易，有所收获，点个赞和爱心**❤️，**更多精彩欢迎关注**
