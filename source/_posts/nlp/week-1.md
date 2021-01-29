---
title: NLP文本分类引导
categories:
- Notebook
tags:
- python
- nlp
---

# NLP文本分类引导

## en

## 1. 数据读取  

### python原生读取txt文件
```py
f = open(filedir, mode='r')
```

- `f.read()`一次读取全部数据。
- `f.readline()`分行读取，一次读取一行。
- `f.readlines()`读取全部数据，保留`\n`换行符。

### pandas读取txt文件
pd.read_table(filedir, splitsign, header=指定第几行为列名, names=如果没有列名手动指定['x', 'y'])

## 2. 数据预处理

### 分词

### 去除停词 去除低频词
```python
    def optimize_words_dict(self, data, stop_words, threshold):
        freq_dict = {}
        for line in data:  # 去除停词 + 计算词频
            for word in line:
                if word in stop_words:
                    continue
                if word not in freq_dict:
                    freq_dict[word] = 1
                else:
                    freq_dict[word] += 1
        words_list = []
        values = sorted(list(set(freq_dict.values())), reverse=True)
        for w in values:  # 通过阈值筛选词表 算法可以优化？？？
            if w < threshold:
                continue
            for k, v in freq_dict.items():
                if v == w:
                    words_list.append((k, v))
        return words_list
```

### 通过TF-IDF算法筛选单词

```python
    def tf_idf(self, data):
        doc_num = len(data)
        df = {}
        for sample in data:
            for word in set(sample):  # 避免一个单词在一个Sample里出现多次
                if word not in df:
                    df[word] = 1.0
                else:
                    df[word] += 1.0
        for word in df:  # 计算document frequency 文档总数/单词出现过的文档数
            df[word] = log10(doc_num / df[word])
        res = {}
        index = 0
        for sample in data:
            res[index] = {}
            tf = {}
            for word in sample:  # 计算term frequency 一个样本里单词的频率
                if word not in tf:
                    tf[word] = 1.0
                else:
                    tf[word] += 1.0
            sample_len = len(sample)
            for word in sample:  # 计算tf * idf 一个样本中各单词的tf_idf
                tf_idf = tf[word] / sample_len * df[word]
                res[index][word] = tf_idf
            index += 1

        return res  # res格式为每一个样本中每一个单词对应的tf_idf值
```

**在预处理时，因为是要为生成词典做准备，所以TF-IDF应该是以整个corpus计算TF，而提取特征时建立词袋模型中的TF-IDF方法则是对每个Sample的单词计算TF-IDF值。**

## 3. 建立词典

词典 = {‘单词’:索引}

## 4. 提取特征

### 词袋模型（BOW）

假设建立的词典有1000维，那么我们将为数据集中的每一个Sample建立一个1000维的向量。

词袋模型有三种形式：
- 对于每个Smaple单词是否出现
- 对于每个Sample单词出现次数
- 对于每个Smaple单词的TF-IDF值

**在预处理时，因为是要为生成词典做准备，所以TF-IDF应该是以整个corpus计算TF，而提取特征时建立词袋模型中的TF-IDF方法则是对每个Sample的单词计算TF-IDF值。**

例：
词典：1000. [北京，天气，真，好，北京] = [0, 1, 0, ...,0], 或 [0, 2, 0, ...,0]，或 [0, 0.4, 0, ..., 0]

#### 独热编码（One-Hot）

对于一个特征有N个状态则可以用N位来表示

例：Feature_2有2个状态

Sample_1 = [0, 1], Sample_2 = [1, 0] ...

例：Feature_1有4个状态

Sample_1 = [0, 0, 1, 0], Sample_2 = [0, 1, 0, 0] ...

当一个样本有多个状态时

Sample_1 = [0, 1, 0, 0, 1, 0], Sample_2 = [1, 0, 0, 1, 0, 0] ...

----
一点想法
不确定应该建立多大的词典时，可以考虑Corpus的大小或是每个Sample中Word的个数
词典的大小就与Corpus或Sample中Word的个数成某个比例

## zh

## 1. 数据处理

- 去除空格
- 符号转换（省略号->句号）？
- 去除无意义符号
- 繁体转换简体
