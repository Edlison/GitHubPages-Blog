---
title: ClusterGAN代码实现
catagories: 
- Paper
tags: 
- python
---

# ClusterGAN-Pytorch Implement

## 跑通源代码

https://github.com/zhampel/clusterGAN

### cuDNN error: CUDNN_STATUS_EXECUTION_FAILED

可能由于Python, Pytorch, cuda, cudnn版本之间匹配问题导致。

**方案1:**

直接关闭cudnn加速

```python
torch.backends.cudnn.enabled = False
```

### matplotlib报错no display name and no $DISPLAY environment variable.

在matplotlib**首次**import之后进行配置

```python
matplotlib.use('Agg')
```



# 理解源代码

跑完一个epoch需要每一次迭代一个batch直到迭代全部的数据集。而参数的更新需要在每个batch迭代完后更新。

