# Transfomer 概述       

和经典的 seq2seq 模型一样，Transformer 模型中也采用了 encoer-decoder 架构。上图的左半边用 NX 框出来的，就代表一层 encoder，其中论文里面的 encoder 一共有6层这样的结构。上图的右半边用 NX 框出来的，则代表一层 decoder，同样也有6层。

定义输入序列首先经过 word embedding，再和 positional encoding 相加后，输入到 encoder 中。输出序列经过的处理和输入序列一样，然后输入到 decoder。

最后，decoder 的输出经过一个线性层，再接 Softmax。

于上便是 Transformer 的整体框架，下面先来介绍 encoder 和 decoder。

**Encoder**

由 6 层相同的层组成，每一层分别由两部分组成：

- 第一部分是 multi-head self-attention

- 第二部分是 position-wise feed-forward network，是一个全连接层

两个部分，都有一个残差连接(residual connection)，然后接着一个 Layer Normalization。

**Decoder**

和 encoder 类似，decoder 也是由6个相同的层组成，每一层由三个部分组成：

- 第一个部分是 multi-head self-attention mechanism
- 第二部分是 multi-head context-attention mechanism
- 第三部分是一个 position-wise feed-forward network和encoder 一样，上面三个部分的每一个部分，都有一个残差连接，后接一个 Layer Normalization。

decoder 和 encoder 不同的地方在 multi-head context-attention mechanism

**Attention**

Attention用一句话来描述，就是encoder层的输出经过加权平均后再输入到decoder层中。它主要应用在seq2seq模型中，这个加权可以用矩阵来表示，也叫Attention矩阵。它表示对于某个时刻的输出y，他在输入x上各个部分的注意力。这个注意力就是加权。

**Self-Attention**

attention有两个隐状态，hi和st。前者是输入序列第i个位置产生的隐状态，后者是输出序列在第t个位置产生的隐状态。self-attention就是，输出序列就是输入序列。自己计算自己的attention得分。

**Context-Attention**

context-attention是encoder和decoder之间的attention，是两个不同序列之间的attention，与来源于自身的self-attention相区别。

计算attention权重的方法：
- additive attention
- local-base
- general
- dot-product
- scaled dot-product

**Scaled Dot-Product Attention**

通过query和key的相似性程度来确定value的权重分布。

![img-scaled-dot-attention](https://pic4.zhimg.com/80/v2-e2bc3a470b359e8bdce750843140897e_720w.jpg)



----
Reference

Transformer
https://easyai.tech/ai-definition/transformer/

Attention
https://easyai.tech/ai-definition/attention/
