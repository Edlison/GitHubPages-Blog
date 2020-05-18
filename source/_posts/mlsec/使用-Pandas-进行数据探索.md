## 使用 Pandas 进行数据探索

本次实验通过分析电信运营商的客户离网率数据集来熟悉 Pandas 数据探索的常用方法，并构建一个预测客户离网率的简单模型。

### Pandas 的主要方法

Pandas 是基于 NumPy 的一种工具，提供了大量数据探索的方法。Pandas 可以使用类似 SQL 的方式对 .csv、.tsv、.xlsx 等格式的数据进行处理分析。

Pandas 主要使用的数据结构是 Series 和 DataFrame 类。下面简要介绍下这两类：

- Series 是一种类似于一维数组的对象，它由一组数据（各种 NumPy 数据类型）及一组与之相关的数据标签（即索引）组成。
- DataFrame 是一个二维数据结构，即一张表格，其中每列数据的类型相同。你可以把它看成由 Series 实例构成的字典。

下面开始此次实验，我们将通过分析电信运营商的客户离网率数据集来展示 Pandas 的主要方法。

首先载入必要的库，即 NumPy 和 Pandas。

<i class="fa fa-arrow-circle-down" aria-hidden="true"> 教学代码：</i>


```python
import numpy as np
import pandas as pd
```

通过 `read_csv()` 方法读取数据，然后使用 `head()` 方法查看前 5 行数据。


```python
dataFrame = pd.read_csv('./data/telecom_churn.csv')
```

上图中的每行对应一位客户，每列对应客户的一个特征。

让我们查看一下该数据库的维度、特征名称和特征类型。


```python
dataFrame.shape
```




    (3333, 20)



上述结果表明，我们的列表包含 3333 行和 20 列。下面我们尝试打印列名。


```python
dataFrame.columns
```




    Index(['State', 'Account length', 'Area code', 'International plan',
           'Voice mail plan', 'Number vmail messages', 'Total day minutes',
           'Total day calls', 'Total day charge', 'Total eve minutes',
           'Total eve calls', 'Total eve charge', 'Total night minutes',
           'Total night calls', 'Total night charge', 'Total intl minutes',
           'Total intl calls', 'Total intl charge', 'Customer service calls',
           'Churn'],
          dtype='object')



我们还可以使用 `info()` 方法输出 DataFrame 的一些总体信息。


```python
dataFrame.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3333 entries, 0 to 3332
    Data columns (total 20 columns):
    State                     3333 non-null object
    Account length            3333 non-null int64
    Area code                 3333 non-null int64
    International plan        3333 non-null object
    Voice mail plan           3333 non-null object
    Number vmail messages     3333 non-null int64
    Total day minutes         3333 non-null float64
    Total day calls           3333 non-null int64
    Total day charge          3333 non-null float64
    Total eve minutes         3333 non-null float64
    Total eve calls           3333 non-null int64
    Total eve charge          3333 non-null float64
    Total night minutes       3333 non-null float64
    Total night calls         3333 non-null int64
    Total night charge        3333 non-null float64
    Total intl minutes        3333 non-null float64
    Total intl calls          3333 non-null int64
    Total intl charge         3333 non-null float64
    Customer service calls    3333 non-null int64
    Churn                     3333 non-null bool
    dtypes: bool(1), float64(8), int64(8), object(3)
    memory usage: 498.1+ KB


`bool`、`int64`、`float64` 和 `object` 是该数据库特征的数据类型。这一方法同时也会显示是否有缺失值，上述结果表明在该数据集中不存在缺失值，因为每列都包含 3333 个观测，和我们之前使用 `shape` 方法得到的数字是一致的。

`astype()` 方法可以更改列的类型，下列公式将 Churn 离网率 特征修改为 `int64` 类型。


```python
df = dataFrame
df.head(5)
df['Churn'] = df['Churn'].astype('int64')
df.head(5)
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
      <td>0</td>
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
      <td>0</td>
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
      <td>0</td>
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
      <td>0</td>
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
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



`describe()` 方法可以显示数值特征（`int64` 和 `float64`）的基本统计学特性，如未缺失值的数值、均值、标准差、范围、四分位数等。


```python
df.describe()
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
      <th>Account length</th>
      <th>Area code</th>
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
      <th>count</th>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
      <td>3333.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>101.064806</td>
      <td>437.182418</td>
      <td>8.099010</td>
      <td>179.775098</td>
      <td>100.435644</td>
      <td>30.562307</td>
      <td>200.980348</td>
      <td>100.114311</td>
      <td>17.083540</td>
      <td>200.872037</td>
      <td>100.107711</td>
      <td>9.039325</td>
      <td>10.237294</td>
      <td>4.479448</td>
      <td>2.764581</td>
      <td>1.562856</td>
      <td>0.144914</td>
    </tr>
    <tr>
      <th>std</th>
      <td>39.822106</td>
      <td>42.371290</td>
      <td>13.688365</td>
      <td>54.467389</td>
      <td>20.069084</td>
      <td>9.259435</td>
      <td>50.713844</td>
      <td>19.922625</td>
      <td>4.310668</td>
      <td>50.573847</td>
      <td>19.568609</td>
      <td>2.275873</td>
      <td>2.791840</td>
      <td>2.461214</td>
      <td>0.753773</td>
      <td>1.315491</td>
      <td>0.352067</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>408.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>23.200000</td>
      <td>33.000000</td>
      <td>1.040000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>74.000000</td>
      <td>408.000000</td>
      <td>0.000000</td>
      <td>143.700000</td>
      <td>87.000000</td>
      <td>24.430000</td>
      <td>166.600000</td>
      <td>87.000000</td>
      <td>14.160000</td>
      <td>167.000000</td>
      <td>87.000000</td>
      <td>7.520000</td>
      <td>8.500000</td>
      <td>3.000000</td>
      <td>2.300000</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>101.000000</td>
      <td>415.000000</td>
      <td>0.000000</td>
      <td>179.400000</td>
      <td>101.000000</td>
      <td>30.500000</td>
      <td>201.400000</td>
      <td>100.000000</td>
      <td>17.120000</td>
      <td>201.200000</td>
      <td>100.000000</td>
      <td>9.050000</td>
      <td>10.300000</td>
      <td>4.000000</td>
      <td>2.780000</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>127.000000</td>
      <td>510.000000</td>
      <td>20.000000</td>
      <td>216.400000</td>
      <td>114.000000</td>
      <td>36.790000</td>
      <td>235.300000</td>
      <td>114.000000</td>
      <td>20.000000</td>
      <td>235.300000</td>
      <td>113.000000</td>
      <td>10.590000</td>
      <td>12.100000</td>
      <td>6.000000</td>
      <td>3.270000</td>
      <td>2.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>243.000000</td>
      <td>510.000000</td>
      <td>51.000000</td>
      <td>350.800000</td>
      <td>165.000000</td>
      <td>59.640000</td>
      <td>363.700000</td>
      <td>170.000000</td>
      <td>30.910000</td>
      <td>395.000000</td>
      <td>175.000000</td>
      <td>17.770000</td>
      <td>20.000000</td>
      <td>20.000000</td>
      <td>5.400000</td>
      <td>9.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



通过 include 参数显式指定包含的数据类型，可以查看非数值特征的统计数据。


```python
df.describe(include=['object', 'bool'])
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
      <th>International plan</th>
      <th>Voice mail plan</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>3333</td>
      <td>3333</td>
      <td>3333</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>51</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>top</th>
      <td>WV</td>
      <td>No</td>
      <td>No</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>106</td>
      <td>3010</td>
      <td>2411</td>
    </tr>
  </tbody>
</table>
</div>



`value_counts()` 方法可以查看类别（类型为 object ）和布尔值（类型为 bool ）特征。让我们看下 Churn 离网率 的分布。


```python
df['Churn'].value_counts()
```




    0    2850
    1     483
    Name: Churn, dtype: int64



上述结果表明，在 3333 位客户中， 2850 位是忠实客户，他们的 `Churn` 值为 0。调用 `value_counts()` 函数时，加上 `normalize=True` 参数可以显示比例。


```python
df['Churn'].value_counts(normalize=True)
```




    0    0.855086
    1    0.144914
    Name: Churn, dtype: float64



### 排序

DataFrame 可以根据某个变量的值（也就是列）排序。比如，根据每日消费额排序（设置 ascending=False 倒序排列）。


```python
df.sort_values(by='Total day charge', ascending=False).head(5)
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
      <th>365</th>
      <td>CO</td>
      <td>154</td>
      <td>415</td>
      <td>No</td>
      <td>No</td>
      <td>0</td>
      <td>350.8</td>
      <td>75</td>
      <td>59.64</td>
      <td>216.5</td>
      <td>94</td>
      <td>18.40</td>
      <td>253.9</td>
      <td>100</td>
      <td>11.43</td>
      <td>10.1</td>
      <td>9</td>
      <td>2.73</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>985</th>
      <td>NY</td>
      <td>64</td>
      <td>415</td>
      <td>Yes</td>
      <td>No</td>
      <td>0</td>
      <td>346.8</td>
      <td>55</td>
      <td>58.96</td>
      <td>249.5</td>
      <td>79</td>
      <td>21.21</td>
      <td>275.4</td>
      <td>102</td>
      <td>12.39</td>
      <td>13.3</td>
      <td>9</td>
      <td>3.59</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2594</th>
      <td>OH</td>
      <td>115</td>
      <td>510</td>
      <td>Yes</td>
      <td>No</td>
      <td>0</td>
      <td>345.3</td>
      <td>81</td>
      <td>58.70</td>
      <td>203.4</td>
      <td>106</td>
      <td>17.29</td>
      <td>217.5</td>
      <td>107</td>
      <td>9.79</td>
      <td>11.8</td>
      <td>8</td>
      <td>3.19</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>156</th>
      <td>OH</td>
      <td>83</td>
      <td>415</td>
      <td>No</td>
      <td>No</td>
      <td>0</td>
      <td>337.4</td>
      <td>120</td>
      <td>57.36</td>
      <td>227.4</td>
      <td>116</td>
      <td>19.33</td>
      <td>153.9</td>
      <td>114</td>
      <td>6.93</td>
      <td>15.8</td>
      <td>7</td>
      <td>4.27</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>605</th>
      <td>MO</td>
      <td>112</td>
      <td>415</td>
      <td>No</td>
      <td>No</td>
      <td>0</td>
      <td>335.5</td>
      <td>77</td>
      <td>57.04</td>
      <td>212.5</td>
      <td>109</td>
      <td>18.06</td>
      <td>265.0</td>
      <td>132</td>
      <td>11.93</td>
      <td>12.7</td>
      <td>8</td>
      <td>3.43</td>
      <td>2</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



此外，还可以根据多个列的数值排序。下面函数实现的功能为：先按 Churn 离网率 升序排列，再按 Total day charge 每日总话费 降序排列，优先级 Churn > Tatal day charge。


```python
df.sort_values(by=['Churn', 'Total day charge'], ascending=[True, False]).head()
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
      <th>688</th>
      <td>MN</td>
      <td>13</td>
      <td>510</td>
      <td>No</td>
      <td>Yes</td>
      <td>21</td>
      <td>315.6</td>
      <td>105</td>
      <td>53.65</td>
      <td>208.9</td>
      <td>71</td>
      <td>17.76</td>
      <td>260.1</td>
      <td>123</td>
      <td>11.70</td>
      <td>12.1</td>
      <td>3</td>
      <td>3.27</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2259</th>
      <td>NC</td>
      <td>210</td>
      <td>415</td>
      <td>No</td>
      <td>Yes</td>
      <td>31</td>
      <td>313.8</td>
      <td>87</td>
      <td>53.35</td>
      <td>147.7</td>
      <td>103</td>
      <td>12.55</td>
      <td>192.7</td>
      <td>97</td>
      <td>8.67</td>
      <td>10.1</td>
      <td>7</td>
      <td>2.73</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>534</th>
      <td>LA</td>
      <td>67</td>
      <td>510</td>
      <td>No</td>
      <td>No</td>
      <td>0</td>
      <td>310.4</td>
      <td>97</td>
      <td>52.77</td>
      <td>66.5</td>
      <td>123</td>
      <td>5.65</td>
      <td>246.5</td>
      <td>99</td>
      <td>11.09</td>
      <td>9.2</td>
      <td>10</td>
      <td>2.48</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>575</th>
      <td>SD</td>
      <td>114</td>
      <td>415</td>
      <td>No</td>
      <td>Yes</td>
      <td>36</td>
      <td>309.9</td>
      <td>90</td>
      <td>52.68</td>
      <td>200.3</td>
      <td>89</td>
      <td>17.03</td>
      <td>183.5</td>
      <td>105</td>
      <td>8.26</td>
      <td>14.2</td>
      <td>2</td>
      <td>3.83</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2858</th>
      <td>AL</td>
      <td>141</td>
      <td>510</td>
      <td>No</td>
      <td>Yes</td>
      <td>28</td>
      <td>308.0</td>
      <td>123</td>
      <td>52.36</td>
      <td>247.8</td>
      <td>128</td>
      <td>21.06</td>
      <td>152.9</td>
      <td>103</td>
      <td>6.88</td>
      <td>7.4</td>
      <td>3</td>
      <td>2.00</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### 索引和获取数据

DataFrame 可以以不同的方式进行索引。

使用 `DataFrame['Name']` 可以得到一个单独的列。比如，离网率有多高？


```python
df['Churn'].mean()
```




    0.14491449144914492




对一家公司而言，14.5% 的离网率是一个很糟糕的数据，这么高的离网率可能导致公司破产。

布尔值索引同样很方便，语法是 `df[P(df['Name'])]`，P 是在检查 Name 列每个元素时所使用的逻辑条件。这一索引的输出是 DataFrame 的 Name 列中满足 P 条件的行。

让我们使用布尔值索引来回答这样以下问题：离网用户的数值变量的均值是多少？


```python
df[df['Churn'] == 1].mean()
```




    Account length            102.664596
    Area code                 437.817805
    Number vmail messages       5.115942
    Total day minutes         206.914079
    Total day calls           101.335404
    Total day charge           35.175921
    Total eve minutes         212.410145
    Total eve calls           100.561077
    Total eve charge           18.054969
    Total night minutes       205.231677
    Total night calls         100.399586
    Total night charge          9.235528
    Total intl minutes         10.700000
    Total intl calls            4.163561
    Total intl charge           2.889545
    Customer service calls      2.229814
    Churn                       1.000000
    dtype: float64



离网用户在白天打电话的总时长的均值是多少？


```python
df[df['Churn'] == 1]['Total day minutes'].mean()
```




    206.91407867494823



未使用国际套餐（`International plan == NO`）的忠实用户（`Churn == 0`）所打的最长的国际长途是多久？


```python
df[(df['Churn'] == 1) & (df['International plan'] == 'No')]['Total intl minutes'].max()
df[(df['Churn'] == 0) & (df['International plan'] == 'No')]['Total intl minutes'].max()
```




    18.9



DataFrame 可以通过列名、行名、行号进行索引。`loc` 方法为通过名称索引，`iloc` 方法为通过数字索引。

通过 `loc` 方法输出 0 至 5 行、State 州 至 Area code 区号 的数据。


```python
df.loc[0:5, ['State','Area code']]
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
      <th>Area code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KS</td>
      <td>415</td>
    </tr>
    <tr>
      <th>1</th>
      <td>OH</td>
      <td>415</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NJ</td>
      <td>415</td>
    </tr>
    <tr>
      <th>3</th>
      <td>OH</td>
      <td>408</td>
    </tr>
    <tr>
      <th>4</th>
      <td>OK</td>
      <td>415</td>
    </tr>
    <tr>
      <th>5</th>
      <td>AL</td>
      <td>510</td>
    </tr>
  </tbody>
</table>
</div>



通过 `ilo` 方法输出前 5 行的前 3 列数据（和典型的 Python 切片一样，不含最大值）。


```python
df.iloc[0:5, 0:3]
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KS</td>
      <td>128</td>
      <td>415</td>
    </tr>
    <tr>
      <th>1</th>
      <td>OH</td>
      <td>107</td>
      <td>415</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NJ</td>
      <td>137</td>
      <td>415</td>
    </tr>
    <tr>
      <th>3</th>
      <td>OH</td>
      <td>84</td>
      <td>408</td>
    </tr>
    <tr>
      <th>4</th>
      <td>OK</td>
      <td>75</td>
      <td>415</td>
    </tr>
  </tbody>
</table>
</div>



`df[:1]` 和 `df[-1:]` 可以得到 DataFrame 的首行和末行。


```python
df[-2:]
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
      <th>3331</th>
      <td>CT</td>
      <td>184</td>
      <td>510</td>
      <td>Yes</td>
      <td>No</td>
      <td>0</td>
      <td>213.8</td>
      <td>105</td>
      <td>36.35</td>
      <td>159.6</td>
      <td>84</td>
      <td>13.57</td>
      <td>139.2</td>
      <td>137</td>
      <td>6.26</td>
      <td>5.0</td>
      <td>10</td>
      <td>1.35</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3332</th>
      <td>TN</td>
      <td>74</td>
      <td>415</td>
      <td>No</td>
      <td>Yes</td>
      <td>25</td>
      <td>234.4</td>
      <td>113</td>
      <td>39.85</td>
      <td>265.9</td>
      <td>82</td>
      <td>22.60</td>
      <td>241.4</td>
      <td>77</td>
      <td>10.86</td>
      <td>13.7</td>
      <td>4</td>
      <td>3.70</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### 应用函数到单元格、列、行

下面通过 `apply()` 方法应用函数 `max` 至每一列，即输出每列的最大值。


```python
df.apply(np.max)
```




    State                        WY
    Account length              243
    Area code                   510
    International plan          Yes
    Voice mail plan             Yes
    Number vmail messages        51
    Total day minutes         350.8
    Total day calls             165
    Total day charge          59.64
    Total eve minutes         363.7
    Total eve calls             170
    Total eve charge          30.91
    Total night minutes         395
    Total night calls           175
    Total night charge        17.77
    Total intl minutes           20
    Total intl calls             20
    Total intl charge           5.4
    Customer service calls        9
    Churn                         1
    dtype: object



`apply()` 方法也可以应用函数至每一行，指定 axis=1 即可。在这种情况下，使用 `lambda` 函数十分方便。比如，下面函数选中了所有以 W 开头的州。


```python
df[df['State'].apply(lambda state: state[0] == 'W')].head()
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
      <th>9</th>
      <td>WV</td>
      <td>141</td>
      <td>415</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>37</td>
      <td>258.6</td>
      <td>84</td>
      <td>43.96</td>
      <td>222.0</td>
      <td>111</td>
      <td>18.87</td>
      <td>326.4</td>
      <td>97</td>
      <td>14.69</td>
      <td>11.2</td>
      <td>5</td>
      <td>3.02</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>WY</td>
      <td>57</td>
      <td>408</td>
      <td>No</td>
      <td>Yes</td>
      <td>39</td>
      <td>213.0</td>
      <td>115</td>
      <td>36.21</td>
      <td>191.1</td>
      <td>112</td>
      <td>16.24</td>
      <td>182.7</td>
      <td>115</td>
      <td>8.22</td>
      <td>9.5</td>
      <td>3</td>
      <td>2.57</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>44</th>
      <td>WI</td>
      <td>64</td>
      <td>510</td>
      <td>No</td>
      <td>No</td>
      <td>0</td>
      <td>154.0</td>
      <td>67</td>
      <td>26.18</td>
      <td>225.8</td>
      <td>118</td>
      <td>19.19</td>
      <td>265.3</td>
      <td>86</td>
      <td>11.94</td>
      <td>3.5</td>
      <td>3</td>
      <td>0.95</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>49</th>
      <td>WY</td>
      <td>97</td>
      <td>415</td>
      <td>No</td>
      <td>Yes</td>
      <td>24</td>
      <td>133.2</td>
      <td>135</td>
      <td>22.64</td>
      <td>217.2</td>
      <td>58</td>
      <td>18.46</td>
      <td>70.6</td>
      <td>79</td>
      <td>3.18</td>
      <td>11.0</td>
      <td>3</td>
      <td>2.97</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>54</th>
      <td>WY</td>
      <td>87</td>
      <td>415</td>
      <td>No</td>
      <td>No</td>
      <td>0</td>
      <td>151.0</td>
      <td>83</td>
      <td>25.67</td>
      <td>219.7</td>
      <td>116</td>
      <td>18.67</td>
      <td>203.9</td>
      <td>127</td>
      <td>9.18</td>
      <td>9.7</td>
      <td>3</td>
      <td>2.62</td>
      <td>5</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



`map()` 方法可以通过一个 {old_value:new_value} 形式的字典替换某一列中的值。


```python
d = {'No': False, 'Yes': True}
df['International plan'] = df['International plan'].map(d)
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
      <td>False</td>
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
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>OH</td>
      <td>107</td>
      <td>415</td>
      <td>False</td>
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
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NJ</td>
      <td>137</td>
      <td>415</td>
      <td>False</td>
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
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>OH</td>
      <td>84</td>
      <td>408</td>
      <td>True</td>
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
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>OK</td>
      <td>75</td>
      <td>415</td>
      <td>True</td>
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
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



当然，使用 `repalce()` 方法一样可以达到替换的目的。


```python
df = df.replace({'Voice mail plan': d})
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
      <td>False</td>
      <td>True</td>
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
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>OH</td>
      <td>107</td>
      <td>415</td>
      <td>False</td>
      <td>True</td>
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
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NJ</td>
      <td>137</td>
      <td>415</td>
      <td>False</td>
      <td>False</td>
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
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>OH</td>
      <td>84</td>
      <td>408</td>
      <td>True</td>
      <td>False</td>
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
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>OK</td>
      <td>75</td>
      <td>415</td>
      <td>True</td>
      <td>False</td>
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
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### 增减 DataFrame 的行列

在 DataFrame 中新增列有很多方法，比如，使用 `insert()`方法添加列，为所有用户计算总的 Total calls 电话量。


```python
total_calls = df['Total day calls'] + df['Total eve calls'] + \
df['Total night calls'] + df['Total intl calls']

df.insert(loc=len(df.columns), column='Total calls 2', value=total_calls)

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
      <th>...</th>
      <th>Total night minutes</th>
      <th>Total night calls</th>
      <th>Total night charge</th>
      <th>Total intl minutes</th>
      <th>Total intl calls</th>
      <th>Total intl charge</th>
      <th>Customer service calls</th>
      <th>Churn</th>
      <th>Total calls</th>
      <th>Total calls 2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KS</td>
      <td>128</td>
      <td>415</td>
      <td>False</td>
      <td>True</td>
      <td>25</td>
      <td>265.1</td>
      <td>110</td>
      <td>45.07</td>
      <td>197.4</td>
      <td>...</td>
      <td>244.7</td>
      <td>91</td>
      <td>11.01</td>
      <td>10.0</td>
      <td>3</td>
      <td>2.70</td>
      <td>1</td>
      <td>0</td>
      <td>303</td>
      <td>303</td>
    </tr>
    <tr>
      <th>1</th>
      <td>OH</td>
      <td>107</td>
      <td>415</td>
      <td>False</td>
      <td>True</td>
      <td>26</td>
      <td>161.6</td>
      <td>123</td>
      <td>27.47</td>
      <td>195.5</td>
      <td>...</td>
      <td>254.4</td>
      <td>103</td>
      <td>11.45</td>
      <td>13.7</td>
      <td>3</td>
      <td>3.70</td>
      <td>1</td>
      <td>0</td>
      <td>332</td>
      <td>332</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NJ</td>
      <td>137</td>
      <td>415</td>
      <td>False</td>
      <td>False</td>
      <td>0</td>
      <td>243.4</td>
      <td>114</td>
      <td>41.38</td>
      <td>121.2</td>
      <td>...</td>
      <td>162.6</td>
      <td>104</td>
      <td>7.32</td>
      <td>12.2</td>
      <td>5</td>
      <td>3.29</td>
      <td>0</td>
      <td>0</td>
      <td>333</td>
      <td>333</td>
    </tr>
    <tr>
      <th>3</th>
      <td>OH</td>
      <td>84</td>
      <td>408</td>
      <td>True</td>
      <td>False</td>
      <td>0</td>
      <td>299.4</td>
      <td>71</td>
      <td>50.90</td>
      <td>61.9</td>
      <td>...</td>
      <td>196.9</td>
      <td>89</td>
      <td>8.86</td>
      <td>6.6</td>
      <td>7</td>
      <td>1.78</td>
      <td>2</td>
      <td>0</td>
      <td>255</td>
      <td>255</td>
    </tr>
    <tr>
      <th>4</th>
      <td>OK</td>
      <td>75</td>
      <td>415</td>
      <td>True</td>
      <td>False</td>
      <td>0</td>
      <td>166.7</td>
      <td>113</td>
      <td>28.34</td>
      <td>148.3</td>
      <td>...</td>
      <td>186.9</td>
      <td>121</td>
      <td>8.41</td>
      <td>10.1</td>
      <td>3</td>
      <td>2.73</td>
      <td>3</td>
      <td>0</td>
      <td>359</td>
      <td>359</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



上面的代码创建了一个中间 Series 实例，即 tatal_calls，其实可以在不创造这个实例的情况下直接添加列。


```python
df['Total charge'] = df['Total day charge'] + df['Total eve charge'] + \
df['Total night charge'] + df['Total intl charge']
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
      <th>...</th>
      <th>Total night calls</th>
      <th>Total night charge</th>
      <th>Total intl minutes</th>
      <th>Total intl calls</th>
      <th>Total intl charge</th>
      <th>Customer service calls</th>
      <th>Churn</th>
      <th>Total calls</th>
      <th>Total calls 2</th>
      <th>Total charge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KS</td>
      <td>128</td>
      <td>415</td>
      <td>False</td>
      <td>True</td>
      <td>25</td>
      <td>265.1</td>
      <td>110</td>
      <td>45.07</td>
      <td>197.4</td>
      <td>...</td>
      <td>91</td>
      <td>11.01</td>
      <td>10.0</td>
      <td>3</td>
      <td>2.70</td>
      <td>1</td>
      <td>0</td>
      <td>303</td>
      <td>303</td>
      <td>75.56</td>
    </tr>
    <tr>
      <th>1</th>
      <td>OH</td>
      <td>107</td>
      <td>415</td>
      <td>False</td>
      <td>True</td>
      <td>26</td>
      <td>161.6</td>
      <td>123</td>
      <td>27.47</td>
      <td>195.5</td>
      <td>...</td>
      <td>103</td>
      <td>11.45</td>
      <td>13.7</td>
      <td>3</td>
      <td>3.70</td>
      <td>1</td>
      <td>0</td>
      <td>332</td>
      <td>332</td>
      <td>59.24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NJ</td>
      <td>137</td>
      <td>415</td>
      <td>False</td>
      <td>False</td>
      <td>0</td>
      <td>243.4</td>
      <td>114</td>
      <td>41.38</td>
      <td>121.2</td>
      <td>...</td>
      <td>104</td>
      <td>7.32</td>
      <td>12.2</td>
      <td>5</td>
      <td>3.29</td>
      <td>0</td>
      <td>0</td>
      <td>333</td>
      <td>333</td>
      <td>62.29</td>
    </tr>
    <tr>
      <th>3</th>
      <td>OH</td>
      <td>84</td>
      <td>408</td>
      <td>True</td>
      <td>False</td>
      <td>0</td>
      <td>299.4</td>
      <td>71</td>
      <td>50.90</td>
      <td>61.9</td>
      <td>...</td>
      <td>89</td>
      <td>8.86</td>
      <td>6.6</td>
      <td>7</td>
      <td>1.78</td>
      <td>2</td>
      <td>0</td>
      <td>255</td>
      <td>255</td>
      <td>66.80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>OK</td>
      <td>75</td>
      <td>415</td>
      <td>True</td>
      <td>False</td>
      <td>0</td>
      <td>166.7</td>
      <td>113</td>
      <td>28.34</td>
      <td>148.3</td>
      <td>...</td>
      <td>121</td>
      <td>8.41</td>
      <td>10.1</td>
      <td>3</td>
      <td>2.73</td>
      <td>3</td>
      <td>0</td>
      <td>359</td>
      <td>359</td>
      <td>52.09</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 23 columns</p>
</div>



使用 `drop()` 方法删除列和行。


```python
df1 = df.drop([1, 2])
df1.head()
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
      <th>...</th>
      <th>Total night minutes</th>
      <th>Total night calls</th>
      <th>Total night charge</th>
      <th>Total intl minutes</th>
      <th>Total intl calls</th>
      <th>Total intl charge</th>
      <th>Customer service calls</th>
      <th>Churn</th>
      <th>Total calls</th>
      <th>Total charge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KS</td>
      <td>128</td>
      <td>415</td>
      <td>False</td>
      <td>True</td>
      <td>25</td>
      <td>265.1</td>
      <td>110</td>
      <td>45.07</td>
      <td>197.4</td>
      <td>...</td>
      <td>244.7</td>
      <td>91</td>
      <td>11.01</td>
      <td>10.0</td>
      <td>3</td>
      <td>2.70</td>
      <td>1</td>
      <td>0</td>
      <td>303</td>
      <td>75.56</td>
    </tr>
    <tr>
      <th>3</th>
      <td>OH</td>
      <td>84</td>
      <td>408</td>
      <td>True</td>
      <td>False</td>
      <td>0</td>
      <td>299.4</td>
      <td>71</td>
      <td>50.90</td>
      <td>61.9</td>
      <td>...</td>
      <td>196.9</td>
      <td>89</td>
      <td>8.86</td>
      <td>6.6</td>
      <td>7</td>
      <td>1.78</td>
      <td>2</td>
      <td>0</td>
      <td>255</td>
      <td>66.80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>OK</td>
      <td>75</td>
      <td>415</td>
      <td>True</td>
      <td>False</td>
      <td>0</td>
      <td>166.7</td>
      <td>113</td>
      <td>28.34</td>
      <td>148.3</td>
      <td>...</td>
      <td>186.9</td>
      <td>121</td>
      <td>8.41</td>
      <td>10.1</td>
      <td>3</td>
      <td>2.73</td>
      <td>3</td>
      <td>0</td>
      <td>359</td>
      <td>52.09</td>
    </tr>
    <tr>
      <th>5</th>
      <td>AL</td>
      <td>118</td>
      <td>510</td>
      <td>True</td>
      <td>False</td>
      <td>0</td>
      <td>223.4</td>
      <td>98</td>
      <td>37.98</td>
      <td>220.6</td>
      <td>...</td>
      <td>203.9</td>
      <td>118</td>
      <td>9.18</td>
      <td>6.3</td>
      <td>6</td>
      <td>1.70</td>
      <td>0</td>
      <td>0</td>
      <td>323</td>
      <td>67.61</td>
    </tr>
    <tr>
      <th>6</th>
      <td>MA</td>
      <td>121</td>
      <td>510</td>
      <td>False</td>
      <td>True</td>
      <td>24</td>
      <td>218.2</td>
      <td>88</td>
      <td>37.09</td>
      <td>348.5</td>
      <td>...</td>
      <td>212.6</td>
      <td>118</td>
      <td>9.57</td>
      <td>7.5</td>
      <td>7</td>
      <td>2.03</td>
      <td>3</td>
      <td>0</td>
      <td>321</td>
      <td>78.31</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



对上述代码的部分解释：

- 将相应的索引 `['Total charge', 'Total calls']` 和 `axis` 参数（1 表示删除列，0 表示删除行，默认值为 0）传给 `drop`。
- `inplace` 参数表示是否修改原始 DataFrame （False 表示不修改现有 DataFrame，返回一个新 DataFrame，True 表示修改当前 DataFrame）。

## 预测离网率

首先，通过 `crosstab()` 方法构建一个交叉表来查看 International plan 国际套餐 变量和 Churn 离网率 的相关性，同时使用 `countplot()` 方法构建计数直方图来可视化结果。


```python
# 加载模块，配置绘图
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
sns.countplot(x='International plan', hue='Churn', data=df)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11f0c8450>




![png](data/output_75_1.png)


上图表明，开通了国际套餐的用户的离网率要高很多，这是一个很有趣的观测结果。也许，国际电话高昂的话费让客户很不满意。

同理，查看 Customer service calls 客服呼叫 变量与 Chunrn 离网率 的相关性，并可视化结果。


```python
pd.crosstab(df['Churn'], df['Customer service calls'], margins=True)
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
      <th>Customer service calls</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>All</th>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>605</td>
      <td>1059</td>
      <td>672</td>
      <td>385</td>
      <td>90</td>
      <td>26</td>
      <td>8</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
      <td>2850</td>
    </tr>
    <tr>
      <th>1</th>
      <td>92</td>
      <td>122</td>
      <td>87</td>
      <td>44</td>
      <td>76</td>
      <td>40</td>
      <td>14</td>
      <td>5</td>
      <td>1</td>
      <td>2</td>
      <td>483</td>
    </tr>
    <tr>
      <th>All</th>
      <td>697</td>
      <td>1181</td>
      <td>759</td>
      <td>429</td>
      <td>166</td>
      <td>66</td>
      <td>22</td>
      <td>9</td>
      <td>2</td>
      <td>2</td>
      <td>3333</td>
    </tr>
  </tbody>
</table>
</div>




```python
sns.countplot(x='Customer service calls', hue='Churn', data=df)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11cb70d50>




![png](data/output_79_1.png)


上图表明，在客服呼叫 4 次之后，客户的离网率显著提升。

为了更好的突出 Customer service call 客服呼叫 和 Churn 离网率 的关系，可以给 DataFrame 添加一个二元属性 Many_service_calls，即客户呼叫超过 3 次（Customer service calls > 3）。看下它与离网率的相关性，并可视化结果。


```python
df['Many_service_calls'] = (df['Customer service calls'] > 3).astype('int')

pd.crosstab(df['Many_service_calls'], df['Churn'], margins=True)
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
      <th>Churn</th>
      <th>0</th>
      <th>1</th>
      <th>All</th>
    </tr>
    <tr>
      <th>Many_service_calls</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2721</td>
      <td>345</td>
      <td>3066</td>
    </tr>
    <tr>
      <th>1</th>
      <td>129</td>
      <td>138</td>
      <td>267</td>
    </tr>
    <tr>
      <th>All</th>
      <td>2850</td>
      <td>483</td>
      <td>3333</td>
    </tr>
  </tbody>
</table>
</div>




```python
sns.countplot(x='Many_service_calls', hue='Churn', data=df)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11ce1c850>




![png](data/output_83_1.png)


现在我们可以创建另一张交叉表，将 Churn 离网率 与 International plan 国际套餐 及新创建的 Many_service_calls 多次客服呼叫 关联起来。




```python
pd.crosstab(df['Many_service_calls'] & df['International plan'], df['Churn'])
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
      <th>Churn</th>
      <th>0</th>
      <th>1</th>
    </tr>
    <tr>
      <th>row_0</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>False</th>
      <td>2841</td>
      <td>464</td>
    </tr>
    <tr>
      <th>True</th>
      <td>9</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>



上表表明，在客服呼叫次数超过 3 次并且已办理 International Plan 国际套餐 的情况下，预测一名客户不忠诚的准确率（Accuracy）可以达到 85.8％，计算公式如下：

$$准确率（Accuracy）=\frac{TP+TN}{TP+TN+FP+FN}=\frac{2841+19}{2841+9+19+464}\times100\%$$

其中，TP 表示将 True 预测为 True 的数量，TN 表示将 Flase 预测为 Flase 的数量，FP 表示将 Flase 预测为 True 的数量，FN 表示将 True 预测为 Flase 的数量。

复习一下本次实验的内容：

- 样本中忠实客户的份额为 85.5%。这意味着最简单的预测「忠实客户」的模型有 85.5% 的概率猜对。也就是说，后续模型的准确率（Accuracy）不应该比这个数字少，并且很有希望显著高于这个数字。
- 基于一个简单的「（客服呼叫次数 > 3） & （国际套餐 = True） => Churn = 1, else Churn = 0」规则的预测模型，可以得到 85.8% 的准确率。以后我们将讨论决策树，看看如何仅仅基于输入数据自动找出类似的规则，而不需要我们手工设定。我们没有应用机器学习方法就得到了两个准确率（85.5% 和 85.8%），它们可作为后续其他模型的基线。如果经过大量的努力，我们仅将准确率提高了 0.5%，那么我们努力的方向可能出现了偏差，因为仅仅使用一个包含两个限制规则的简单模型就已提升了 0.3% 的准确率。
- 在训练复杂模型之前，建议预处理一下数据，绘制一些图表，做一些简单的假设。此外，在实际任务上应用机器学习时，通常从简单的方案开始，接着尝试更复杂的方案。

### 实验总结

本次实验使用 Pandas 对数据进行了一定程度的分析和探索，交叉表、透视表等方法的运用将使你在数据探索过程中事半功倍。
