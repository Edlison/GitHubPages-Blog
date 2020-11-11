# Encoder-Decoder 和 Seq2Seq 概述

## Encoder-Decoder

**将现实问题转化为数学问题 通过求解数学问题 从而解决现实问题**

![img-encoder](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-10-28-encoder.png)
![img-decoder](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-10-28-decoder.png)
![img-en-de](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-10-28-Encoder-Decoder.png)

**缺点：**

Encoder 和 Decoder之间只有一个向量C来传递信息，且C的长度固定。

当输入信息太长时，会丢掉一些信息。

**Attention机制就是为了解决信息过长，信息丢失的问题。**

Attention模型的特点是Encoder不再将整个输入序列编码为固定长度的中间向量C，而是编码成一个向量的序列。

![img-attention](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-10-28-attention.png)



## Seq2Seq

**输入一个序列 输出另一个序列 **

这种结构最重要的地方在于输入序列和输出序列的长度是可变的。



## Encoder-Decoder and Seq2Seq

- Seq2Seq属于Encoder-Decoder的大范畴
- Seq2Seq更强调目的，Encoder-Decoder更强调方法




Rreference

Encoder-Decoder 和 Seq2Seq

https://easyai.tech/ai-definition/encoder-decoder-seq2seq/