# Attention Is All You Need

## Abstract

我们提出一种全新的简洁的网络架构--Transfomer，它完全基于Attention机制，舍弃了循环与卷积。

## 1 Introduction

Transformer的并行性很好的解决了RNN模型的序列化输入输出。

## 2 Background

## 3 Model Architecture

大部分序列神经网络模型都具有encoder-decoder结构。

### 3.1 Encoder and Decoder Stacks

encoder由6个相同的层构成，每一层有两个子层。第一个是一个多Attention机制，第二个是一个简单的全连接feed-forward网络。两个子层中间部署一个参差网络，后接一个正则化。

decoder也是由6个相同的层构成。除了两个子层外中间还加入了一个第三子层Multi-head Attention.

### 3.2 Attention

Attention的核心内容是为输入向量的每个单词(512d)学习一个权重。在self-attention中，每个单词有3个不同的向量，Query，Key，Value，长度均是64. 他们通过3个不同的权值矩阵由嵌入向量X乘以三个不同的权值矩阵WQ，WK，WV(512 * 64)得到。

Attention计算步骤：
- 将输入单词转换成词向量
- 根据词向量得到q, k, v三个向量
- 为每个向量计算一个score: score = q * kT
- 为了梯度的稳定，Transformer使用了score归一化，即除以根号dk
- 对score过一遍softmax函数
- softmax点乘value得到每个输入向量的评分v
- 想家之后得到最终的输出结果z=∑v

### 3.3 Multi-Head Attention

Multi-Head Attention相当于h个不同的self-attention的集成。

Multi-Head输出分三步
- 将X分别输入到8个self-attention中得到8个加权后的特征矩阵Z
- 将8个Z按列拼接成一个大的特征矩阵
- 将特征矩阵经过一层fc后得到输出Z

### 3.4 损失层

解码器解码后，解码的特征向量通过一层激活函数为softmax的全连接层之后得到反应每个单词概率的输出向量。此时通过CTC等损失函数训练模型。

## 4 位置编码

Transfomer无法捕捉循序序列的能力。论文在编码词向量时引入了位置编码的特征，位置编码会在词向量中加入单词的位置信息。

在上式中， [公式] 表示单词的位置， [公式] 表示单词的维度。关于位置编码的实现可在Google开源的算法中get_timing_signal_1d()函数找到对应的代码。

作者这么设计的原因是考虑到在NLP任务重，除了单词的绝对位置，单词的相对位置也非常重要。根据公式 [公式] 以及[公式] ，这表明位置 [公式] 的位置向量可以表示为位置 [公式] 的特征向量的线性变化，这为模型捕捉单词之间的相对位置关系提供了非常大的便利。

## 问题

位置编码对词袋模型等具有普适性？
encoder/decoder通过self-attention层所得到的向量即理解为加密/解密？
得到最后的Z之后怎么用？