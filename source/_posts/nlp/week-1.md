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

