## Python 数据可视化分析

---

#### 介绍

在机器学习领域中，可视化是十分重要的。在开始一项新任务时，通过可视化手段探索数据能更好地帮助人们把握数据的要点。在分析模型表现和模型报告的结果时，可视化能使分析显得更加生动鲜明。有时候，为了理解复杂的模型，我们还可以将高维空间映射为视觉上更直观的二维或三维图形。

总而言之，可视化是一个相对快捷的从数据中挖掘信息的手段。本文将使用 Pandas、Matplotlib、seaborn 等流行的库，带你上手可视化。

#### 知识点

- 单变量可视化的常用方法
- 多变量可视化的常用方法
- t-SNE


### 数据集

首先使用 `import` 载入相关依赖。


```python
import numpy as np
import pandas as pd
import seaborn as sns
sns.set()
```

在第一篇文章中，我们使用的是某电信运营商的客户离网数据集，本次实验仍旧使用这个数据集。


```python
df = pd.read_csv('./data/telecom_churn.csv')
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Account length</th>
      <th>Area code</th>
      <th>International plan</th>
      <th>Voice mail plan</th>
      <th>Number vmail messages</th>
      <th>Total day minutes</th>
      <th>Total day calls</th>
      <th>Total day charge</th>
      <th>Total eve minutes</th>
      <th>Total eve calls</th>
      <th>Total eve charge</th>
      <th>Total night minutes</th>
      <th>Total night calls</th>
      <th>Total night charge</th>
      <th>Total intl minutes</th>
      <th>Total intl calls</th>
      <th>Total intl charge</th>
      <th>Customer service calls</th>
      <th>Churn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KS</td>
      <td>128</td>
      <td>415</td>
      <td>No</td>
      <td>Yes</td>
      <td>25</td>
      <td>265.1</td>
      <td>110</td>
      <td>45.07</td>
      <td>197.4</td>
      <td>99</td>
      <td>16.78</td>
      <td>244.7</td>
      <td>91</td>
      <td>11.01</td>
      <td>10.0</td>
      <td>3</td>
      <td>2.70</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>OH</td>
      <td>107</td>
      <td>415</td>
      <td>No</td>
      <td>Yes</td>
      <td>26</td>
      <td>161.6</td>
      <td>123</td>
      <td>27.47</td>
      <td>195.5</td>
      <td>103</td>
      <td>16.62</td>
      <td>254.4</td>
      <td>103</td>
      <td>11.45</td>
      <td>13.7</td>
      <td>3</td>
      <td>3.70</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NJ</td>
      <td>137</td>
      <td>415</td>
      <td>No</td>
      <td>No</td>
      <td>0</td>
      <td>243.4</td>
      <td>114</td>
      <td>41.38</td>
      <td>121.2</td>
      <td>110</td>
      <td>10.30</td>
      <td>162.6</td>
      <td>104</td>
      <td>7.32</td>
      <td>12.2</td>
      <td>5</td>
      <td>3.29</td>
      <td>0</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>OH</td>
      <td>84</td>
      <td>408</td>
      <td>Yes</td>
      <td>No</td>
      <td>0</td>
      <td>299.4</td>
      <td>71</td>
      <td>50.90</td>
      <td>61.9</td>
      <td>88</td>
      <td>5.26</td>
      <td>196.9</td>
      <td>89</td>
      <td>8.86</td>
      <td>6.6</td>
      <td>7</td>
      <td>1.78</td>
      <td>2</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>OK</td>
      <td>75</td>
      <td>415</td>
      <td>Yes</td>
      <td>No</td>
      <td>0</td>
      <td>166.7</td>
      <td>113</td>
      <td>28.34</td>
      <td>148.3</td>
      <td>122</td>
      <td>12.61</td>
      <td>186.9</td>
      <td>121</td>
      <td>8.41</td>
      <td>10.1</td>
      <td>3</td>
      <td>2.73</td>
      <td>3</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



最后一个数据列 Churn 离网率 是我们的目标特征，它是布尔变量，其中 True 表示公司最终丢失了此客户，False 表示客户被保留。稍后，将构建基于其他特征预测 Churn 特征的模型。 

### 单变量可视化

单变量（univariate）分析一次只关注一个变量。当我们独立地分析一个特征时，通常最关心的是该特征值的分布情况。下面考虑不同统计类型的变量，以及相应的可视化工具。

#### 数量特征

数量特征（quantitative feature）的值为有序数值。这些值可能是离散的，例如整数，也可能是连续的，例如实数。

#### 直方图和密度图

直方图依照相等的间隔将值分组为柱，它的形状可能包含了数据分布的一些信息，如高斯分布、指数分布等。当分布总体呈现规律性，但有个别异常值时，你可以通过直方图辨认出来。当你使用的机器学习方法预设了某一特定分布类型（通常是高斯分布）时，知道特征值的分布是非常重要的。

最简单的查看数值变量分布的方法是使用 DataFrame 的 [<i class="fa fa-external-link-square" aria-hidden="true"> `hist()`</i>](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.hist.html) 方法绘制直方图。


```python
features = ['Total day minutes', 'Total intl calls']
df[features].hist(figsize = (10, 4))
```




    array([[<matplotlib.axes._subplots.AxesSubplot object at 0x12e3076d0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x12f4cbe90>]],
          dtype=object)




![png](data/output_21_1.png)


上图表明，变量 Total day minutes 每日通话时长 呈高斯分布，而 Total intl calls 总国际呼叫数 显著右倾（它右侧的尾巴更长）。

密度图（density plots），也叫核密度图（ [<i class="fa fa-external-link-square" aria-hidden="true"> kernel density estimate</i>](https://en.wikipedia.org/wiki/Kernel_density_estimation)，KDE）是理解数值变量分布的另一个方法。它可以看成是直方图平滑（ [<i class="fa fa-external-link-square" aria-hidden="true"> smoothed</i>](https://en.wikipedia.org/wiki/Kernel_smoother) ）的版本。相比直方图，它的主要优势是不依赖于柱的尺寸，更加清晰。

让我们为上面两个变量创建密度图。


```python
df[features].plot(kind='density', subplots=True, layout=(1,2),
                 sharex=False, figsize=(10, 4), legend=False, title=features)
```




    array([[<matplotlib.axes._subplots.AxesSubplot object at 0x133bcc050>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x1343f1ad0>]],
          dtype=object)




![png](data/output_25_1.png)



```python
sns.distplot(df['Total day calls'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x12e4f8810>




![png](data/output_26_1.png)


当然，还可以使用 seaborn 的 [<i class="fa fa-external-link-square" aria-hidden="true"> `distplot()`</i>](https://seaborn.pydata.org/generated/seaborn.distplot.html) 方法观测数值变量的分布。例如，Total day minutes 每日通话时长 的分布。默认情况下，该方法将同时显示直方图和密度图。


```python
sns.distplot(df['Total intl calls'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x12e5134d0>




![png](data/output_28_1.png)


上图中直方图的柱形高度已进行归一化处理，表示的是密度而不是样本数。

#### 箱型图

箱形图的主要组成部分是箱子（box），须（whisker）和一些单独的数据点（离群值），分别简单介绍如下：

- 箱子显示了分布的四分位距，它的长度由 $25th \, （\text{Q1，下四分位数}）$ 和 $75th \, （\text{Q3，上四分位数}）$ 决定，箱中的水平线表示中位数  （$50\%$）。
- 须是从箱子处延伸出来的线，它们表示数据点的总体散布，具体而言，是位于区间 $（\text{Q1} - 1.5 \cdot \text{IQR}, \text{Q3} + 1.5 \cdot \text{IQR}）$的数据点，其中 $\text{IQR} = \text{Q3} - \text{Q1}$，也就是四分位距。
- 离群值是须之外的数据点，它们作为单独的数据点，沿着中轴绘制。

使用 seaborn 的 `boxplot()` 方法绘制箱形图。


```python
sns.boxplot(df['Total intl calls'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x12e6e1750>




![png](data/output_34_1.png)


上图表明，在该数据集中，大量的国际呼叫是相当少见的。

#### 提琴形图

我们最后考虑的分布图形是提琴形图（violin plot）。提琴形图和箱形图的区别是，提琴形图聚焦于平滑后的整体分布，而箱形图显示了单独样本的特定统计数据。

使用 `violinplot()` 方法绘制提琴形图。下图左侧是箱形图，右侧是提琴形图。


```python
import matplotlib.pyplot as plt

_, ax = plt.subplots(1, 2, sharey=True, figsize=(10, 4))

sns.boxplot(df['Total intl calls'], ax=ax[0])
sns.violinplot(df['Total intl calls'], ax=ax[1])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1380219d0>




![png](data/output_39_1.png)


#### 数据描述


除图形工具外，还可以使用 DataFrame 的 [<i class="fa fa-external-link-square" aria-hidden="true"> `describe()`</i>](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.describe.html) 方法来获取分布的精确数值统计。


```python
df[features].describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total day minutes</th>
      <th>Total intl calls</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>3333.000000</td>
      <td>3333.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>179.775098</td>
      <td>4.479448</td>
    </tr>
    <tr>
      <th>std</th>
      <td>54.467389</td>
      <td>2.461214</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>143.700000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>179.400000</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>216.400000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>350.800000</td>
      <td>20.000000</td>
    </tr>
  </tbody>
</table>
</div>



`describe()` 的输出基本上是自解释性的，25%，50% 和 75% 是相应的百分数 [<i class="fa fa-external-link-square" aria-hidden="true"> percentiles</i>](https://en.wikipedia.org/wiki/Percentile)。

#### 类别特征和二元特征

类别特征（categorical features take）反映了样本的某个定性属性，它具有固定数目的值，每个值将一个观测数据分配到相应的组，这些组称为类别（category）。如果类别变量的值具有顺序，称为有序（ordinal）类别变量。

二元（binary）特征是类别特征的特例，其可能值有 2 个。

#### 频率表

让我们查看一下目标变量 Churn 离网率 的分布情况。首先，使用 [<i class="fa fa-external-link-square" aria-hidden="true"> `value_counts()`</i>](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.value_counts.html) 方法得到一张频率表。


```python
df['Churn'].value_counts()
```




    False    2850
    True      483
    Name: Churn, dtype: int64



上表显示，该数据集的 Churn 有 2850 个属于 False（Churn==0），有 483 个属于 True（Churn==1），数据集中忠实客户（Churn==0）和不忠实客户（Churn==1）的比例并不相等。我们将在以后的文章中看到，这种数据不平衡的情况会导致建立的分类模型存在一定的问题。在这种情况下，构建分类模型可能需要加重对「少数数据（在这里是 Churn==1）分类错误」这一情况的惩罚。

#### 条形图


频率表的图形化表示是条形图。创建条形图最简单的方法是使用 seaborn 的 [<i class="fa fa-external-link-square" aria-hidden="true"> `countplot()`</i>](https://seaborn.pydata.org/generated/seaborn.countplot.html) 函数。让我们来画出两个分类变量的分布。


```python
fig, ax = plt.subplots(1, 2, figsize=[10, 4])

sns.countplot(df['Churn'], ax=ax[0])
sns.countplot(df['Customer service calls'], ax=ax[1])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1382242d0>




![png](data/output_53_1.png)


条形图和直方图的区别如下：

- 直方图适合查看数值变量的分布，而条形图用于查看类别特征。
- 直方图的 X 轴是数值；条形图的 X 轴可能是任何类型，如数字、字符串、布尔值。
- 直方图的 X 轴是一个笛卡尔坐标轴；条形图的顺序则没有事先定义。

上左图清晰地表明了目标变量的失衡性。上右图则表明大部分客户最多打了 2-3 个客服电话就解决了他们的问题。不过，既然想要预测少数数据的分类（Churn==1），我们可能对少数不满意的客户的表现更感兴趣。所以让我们尝试一下更有趣的可视化方法：多变量可视化，看能否对预测有所帮助。

### 多变量可视化

多变量（multivariate）图形可以在单张图像中查看两个以上变量的联系，和单变量图形一样，可视化的类型取决于将要分析的变量的类型。

先来看看数量变量之间的相互作用。

#### 相关矩阵

相关矩阵可揭示数据集中的数值变量的相关性。这一信息很重要，因为有一些机器学习算法（比如，线性回归和逻辑回归）不能很好地处理高度相关的输入变量。

首先，我们使用 DataFrame 的 [<i class="fa fa-external-link-square" aria-hidden="true"> `corr()`</i>](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.corr.html) 方法计算出每对特征间的相关性。接着，我们将所得的相关矩阵（correlation matrix）传给 seaborn 的 [<i class="fa fa-external-link-square" aria-hidden="true"> `heatmap()`</i>](https://seaborn.pydata.org/generated/seaborn.heatmap.html)方法，该方法根据提供的数值，渲染出一个基于色彩编码的矩阵。


```python
numerical = list(set(df.columns) - set(['State', 'International plan', 'Voice mail plan', 'Area code', 'Churn', 'Customer service calls']))

corr = df[numerical].corr()
sns.heatmap(corr)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1388bad50>




![png](data/output_63_1.png)


上图中，Total day charge 日话费总额 是直接基于 Total day minutes 电话的分钟数 计算得到，它被称为因变量。除了 Total day charege 外，还有 3 个因变量：Total eve charge，Total night charge，Total intl charge。这 4 个因变量并不贡献任何额外信息，我们直接去除。


```python
numerical = list(set(numerical) - set(['Total day charge', 'Total eve charge', 'Total night charge', 'Total intl charge']))

corr = df[numerical].corr()
sns.heatmap(corr)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1396822d0>




![png](data/output_65_1.png)


#### 散点图

散点图（scatter plot）将两个数值变量的值显示为二维空间中的笛卡尔坐标（Cartesian coordinate）。通过 matplotlib 库的 [<i class="fa fa-external-link-square" aria-hidden="true"> `scatter()`</i>](https://matplotlib.org/devdocs/api/_as_gen/matplotlib.pyplot.scatter.html) 方法可以绘制散点图。


```python
plt.scatter(df['Total day minutes'], df['Total night minutes'])
```




    <matplotlib.collections.PathCollection at 0x139a17c10>




![png](data/output_67_1.png)



我们得到了两个正态分布变量的散点图，看起来这两个变量并不相关，因为上图的形状和轴是对齐的。

seaborn 库的 [<i class="fa fa-external-link-square" aria-hidden="true"> `jointplot()`</i>](https://seaborn.pydata.org/generated/seaborn.jointplot.html) 方法在绘制散点图的同时会绘制两张直方图，某些情形下它们可能会更有用。


```python
sns.jointplot(df['Total day minutes'], df['Total night minutes'])
```




    <seaborn.axisgrid.JointGrid at 0x139888cd0>




![png](data/output_70_1.png)


`jointplot()` 方法还可以绘制平滑过的散点直方图。


```python
sns.jointplot(df['Total day minutes'], df['Total night minutes'], kind='kde', color='g')
```




    <seaborn.axisgrid.JointGrid at 0x139dc1890>




![png](data/output_72_1.png)


上图基本上就是之前讨论过的核密度图的双变量版本。

#### 散点图矩阵

在某些情形下，我们可能想要绘制如下所示的散点图矩阵（scatterplot matrix）。它的对角线包含变量的分布，并且每对变量的散点图填充了矩阵的其余部分。


```python
# %config InlineBackend.figure_format = 'png'
sns.pairplot(df[numerical])
```




    <seaborn.axisgrid.PairGrid at 0x139cb5810>




![png](data/output_76_1.png)


#### 数量和类别

为了让图形更有趣一点，可以尝试从数值和类别特征的相互作用中得到预测 Churn 的新信息，更具体地，让我们看看输入变量和目标变量 Churn 的关系。使用 [<i class="fa fa-external-link-square" aria-hidden="true"> `lmplot()`</i>](https://seaborn.pydata.org/generated/seaborn.lmplot.html) 方法的 hue 参数来指定感兴趣的类别特征。


```python
sns.lmplot('Total day minutes', 'Total night minutes', data=df, hue='Churn', fit_reg=False)
```




    <seaborn.axisgrid.FacetGrid at 0x13e7df950>




![png](data/output_79_2.png)


看起来不忠实客户偏向右上角，也就是倾向于在白天和夜间打更多电话的客户。当然，这不是非常明显，我们也不会基于这一图形下任何确定性的结论。

现在，创建箱形图，以可视化忠实客户（Churn=0）和离网客户（Churn=1）这两个互斥分组中数值变量分布的统计数据。


```python
numerical.append('Customer service calls')
print(numerical)
fig, axes = plt.subplots(3, 4, figsize=[10, 7])
for index, feat in enumerate(numerical):
    ax = axes[int(index / 4), index % 4]
    sns.boxplot(df['Churn'], df[feat], ax=ax)
    ax.set_xlabel('')
    ax.set_ylabel(feat)
fig.tight_layout()
```

    ['Total day minutes', 'Total night minutes', 'Number vmail messages', 'Total eve calls', 'Account length', 'Total intl calls', 'Total eve minutes', 'Total night calls', 'Total day calls', 'Total intl minutes', 'Customer service calls', 'Customer service calls', 'Customer service calls', 'Customer service calls', 'Customer service calls', 'Customer service calls', 'Customer service calls', 'Customer service calls']



    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    <ipython-input-47-539701aff8ff> in <module>
          3 fig, axes = plt.subplots(3, 4, figsize=[10, 7])
          4 for index, feat in enumerate(numerical):
    ----> 5     ax = axes[int(index / 4), index % 4]
          6     sns.boxplot(df['Churn'], df[feat], ax=ax)
          7     ax.set_xlabel('')


    IndexError: index 3 is out of bounds for axis 0 with size 3



![png](data/output_82_2.png)


上面的图表表明，两组之间分歧最大的分布是这三个变量：Total day minutes 日通话分钟数、Customer service calls 客服呼叫数、Number vmail messages 语音邮件数。在后续的课程中，我们将学习如何使用随机森林（Random Forest）或梯度提升（Gradient Boosting）来判定特征对分类的重要性，届时可以清晰地看到，前两个特征对于离网预测模型而言确实非常重要。

创建箱型图和提琴形图，查看忠实客户和不忠实客户的日通话分钟数。


```python
_, axes = plt.subplots(2, 2, sharex=True, sharey=True, figsize=[10, 8])
sns.boxplot(x='Churn', y='Total day minutes', data=df, ax=axes[0][0])
sns.violinplot(x='Churn', y="Total day minutes", data=df, ax=axes[0][1])
sns.boxplot(x='Churn', y='Total night minutes', data=df, ax=axes[1][0])
sns.violinplot(x='Churn', y="Total night minutes", data=df, ax=axes[1][1])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x140f70290>




![png](data/output_85_1.png)


上图表明，不忠实客户倾向于打更多的电话。

我们还可以发现一个有趣的信息：平均而言，离网客户是通讯服务更活跃的用户。或许是他们对话费不满意，所以预防离网的一个可能措施是降低通话费。当然，公司需要进行额外的经济分析，以查明这样做是否真的有利。

当想要一次性分析两个类别维度下的数量变量时，可以用 seaborn 库的 [<i class="fa fa-external-link-square" aria-hidden="true"> `catplot()`</i>](https://seaborn.pydata.org/generated/seaborn.factorplot.html) 函数。例如，在同一图形中可视化 Total day minutes 日通话分钟数 和两个类别变量（Churn 和 Customer service calls）的相互作用。


```python
sns.catplot(x='Churn', y='Total day minutes', col='Customer service calls',
          data=df[df['Customer service calls'] < 8], kind='box', col_wrap=4, height=3, aspect=.8)
```




    <seaborn.axisgrid.FacetGrid at 0x140728250>




![png](data/output_89_1.png)


上图表明，从第 4 次客服呼叫开始，Total day minutes 日通话分钟数 可能不再是客户离网（Churn==1）的主要因素。也许，除了我们之前猜测的话费原因，还有其他问题导致客户对服务不满意，这可能会导致日通话分钟数更少。

#### 类别与类别

正如之前提到的，变量 Customer service calls 客服呼叫数 的重复值很多，因此，既可以看成数值变量，也可以看成有序类别变量。之前已通过计数图（count plot）查看过它的分布了，现在我们感兴趣的是这一有序特征和目标变量 Churn 离网率 之间的关系。

使用 `countplot()` 方法查看客服呼叫数的分布，这次传入 `hue=Churn` 参数，以便在图形中加入类别维度。


```python
sns.countplot(x='Customer service calls', hue='Churn', data=df[df["Customer service calls"] < 10])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x140cb7990>




![png](data/output_94_1.png)


上图表明，呼叫客服达到 4 次以上后，离网率显著增加了。

使用 `countplot()` 方法查看 Churn 离网率 和二元特征 International plan 国际套餐、Voice mail plan 语音邮件套餐 的关系。


```python
_, axes = plt.subplots(1, 2, sharey=True, figsize=(10, 4))

sns.countplot(x='International plan', hue='Churn', data=df, ax=axes[0])
sns.countplot(x='Voice mail plan', hue='Churn', data=df, ax=axes[1])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x142344290>




![png](data/output_97_1.png)


上图表明，开通国际套餐后，离网率会高很多，即 International plan 是否开通国际套餐 是一个重要的特征。我们在 Vocie mail plan 语音邮件套餐 特征上没有观察到类似的效果。

#### 交叉表

除了使用图形进行类别分析之外，还可以使用统计学的传统工具：交叉表（cross tabulation），即使用表格形式表示多个类别变量的频率分布。通过它可以查看某一列或某一行以了解某个变量在另一变量的作用下的分布情况。

通过交叉表查看 Churn 离网率 和类别变量 State 州 的关系。


```python
pd.crosstab(df['State'], df['Churn']).T
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>State</th>
      <th>AK</th>
      <th>AL</th>
      <th>AR</th>
      <th>AZ</th>
      <th>CA</th>
      <th>CO</th>
      <th>CT</th>
      <th>DC</th>
      <th>DE</th>
      <th>FL</th>
      <th>...</th>
      <th>SD</th>
      <th>TN</th>
      <th>TX</th>
      <th>UT</th>
      <th>VA</th>
      <th>VT</th>
      <th>WA</th>
      <th>WI</th>
      <th>WV</th>
      <th>WY</th>
    </tr>
    <tr>
      <th>Churn</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>False</th>
      <td>49</td>
      <td>72</td>
      <td>44</td>
      <td>60</td>
      <td>25</td>
      <td>57</td>
      <td>62</td>
      <td>49</td>
      <td>52</td>
      <td>55</td>
      <td>...</td>
      <td>52</td>
      <td>48</td>
      <td>54</td>
      <td>62</td>
      <td>72</td>
      <td>65</td>
      <td>52</td>
      <td>71</td>
      <td>96</td>
      <td>68</td>
    </tr>
    <tr>
      <th>True</th>
      <td>3</td>
      <td>8</td>
      <td>11</td>
      <td>4</td>
      <td>9</td>
      <td>9</td>
      <td>12</td>
      <td>5</td>
      <td>9</td>
      <td>8</td>
      <td>...</td>
      <td>8</td>
      <td>5</td>
      <td>18</td>
      <td>10</td>
      <td>5</td>
      <td>8</td>
      <td>14</td>
      <td>7</td>
      <td>10</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 51 columns</p>
</div>



上表显示，State 州 有 51 个不同的值，并且每个州只有 3 到 17 个客户抛弃了运营商。通过 `groupby()` 方法计算每个州的离网率，由高到低排列。


```python
df.groupby(['State'])['Churn'].agg([np.mean]).sort_values(by='mean', ascending=False).T
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>State</th>
      <th>NJ</th>
      <th>CA</th>
      <th>TX</th>
      <th>MD</th>
      <th>SC</th>
      <th>MI</th>
      <th>MS</th>
      <th>NV</th>
      <th>WA</th>
      <th>ME</th>
      <th>...</th>
      <th>RI</th>
      <th>WI</th>
      <th>IL</th>
      <th>NE</th>
      <th>LA</th>
      <th>IA</th>
      <th>VA</th>
      <th>AZ</th>
      <th>AK</th>
      <th>HI</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>mean</th>
      <td>0.264706</td>
      <td>0.264706</td>
      <td>0.25</td>
      <td>0.242857</td>
      <td>0.233333</td>
      <td>0.219178</td>
      <td>0.215385</td>
      <td>0.212121</td>
      <td>0.212121</td>
      <td>0.209677</td>
      <td>...</td>
      <td>0.092308</td>
      <td>0.089744</td>
      <td>0.086207</td>
      <td>0.081967</td>
      <td>0.078431</td>
      <td>0.068182</td>
      <td>0.064935</td>
      <td>0.0625</td>
      <td>0.057692</td>
      <td>0.056604</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 51 columns</p>
</div>



上表显示，新泽西和加利福尼亚的离网率超过了 25%，夏威夷和阿拉斯加的离网率则不到 6%。然而，这些结论是基于极少的样本得出的，可能仅适用于这一特定数据集，不太具有泛用性。

### 全局数据集可视化

上面我们一直在研究数据集的不同方面（facet），通过猜测有趣的特征并一次选择少量特征进行可视化。如果我们想一次性显示所有特征并仍然能够解释生成的可视化，该怎么办？

#### 降维

大多数现实世界的数据集有很多特征，每一个特征都可以被看成数据空间的一个维度。因此，我们经常需要处理高维数据集，然而可视化整个高维数据集相当难。为了从整体上查看一个数据集，需要在不损失很多数据信息的前提下，降低用于可视化的维度。这一任务被称为降维（dimensionality reduction）。降维是一个无监督学习（unsupervised learning）问题，因为它需要在不借助任何监督输入（如标签）的前提下，从数据自身得到新的低维特征。

主成分分析（Principal Component Analysis, PCA）是一个著名的降维方法，我们会在之后的课程中讨论它。但主成分分析的局限性在于，它是线性（linear）算法，这意味着对数据有某些特定的限制。

与线性方法相对的，有许多非线性方法，统称流形学习（Manifold Learning）。著名的流形学习方法之一是 t-SNE。

### 实验总结

本章节首先介绍了 Pandas、Matplotlib 和 seaborn 库的一些常用可视化方法，并对客户离网数据集进行了可视化分析和 t-SNE 降维。可视化是一个相对快捷的从数据中挖掘信息的手段，因此，学习这一技术并将其纳入你的日常机器学习工具箱，是很有必要的。
