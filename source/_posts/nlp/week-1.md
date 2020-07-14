# NLP文本分类引导

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

## 2. 去除停词 去除低频词

```py
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

## 3. TF-IDF算法实现

```py
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