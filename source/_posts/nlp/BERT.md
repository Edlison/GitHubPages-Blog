# BERT 概述

BERT的全称是Bidirectional Encoder Representation from Transformers，即双向Transformer的Encoder，因为decoder是不能获要预测的信息的。模型的主要创新点都在pre-train方法上，即用了Masked LM和Next Sentence Prediction两种方法分别捕捉词语和句子级别的representation。

训练方法：
- 它在训练双向语言模型时以减小的概率把少量的词替成了Mask或另一个随机的单词。
- 增加了一个预测下一句的loss。

特点：
- 模型深，12层，中间层只有1024wide。Transformer的中间层有2048。（深而窄 比 浅而宽 的模型更好？）
- MLM(Masked Language Model)，同时利用左侧和右侧的词语。



----
Reference

BERT | Bidirectional Encoder Representation from Transformers
https://easyai.tech/ai-definition/bert/