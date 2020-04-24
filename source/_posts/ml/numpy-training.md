# Numpy训练实例

## 绘制Sigmoid函数与ReLU函数
```python
import numpy as np
import matplotlib.pyplot as plt

# 设置图片大小
plt.figure(figsize=(8, 3))

# x为1维数组
x = np.arange(-10, 10, 0.1)

# Sigmoid函数
s = 1. / (1 + np.exp(-x))

# ReLU函数
y = np.clip(x, a_min=0., a_max=None)

# 绘制Sigmoid函数
f = plt.subplot(121)
plt.plot(x, s, color='r')
plt.text(-5, 0.9, r'$y=\sigma(x)$', fontsize=13)
currentAxis = plt.gca()
currentAxis.xaxis.set_label_text('x', fontsize=15)
currentAxis.yaxis.set_label_text('y', fontsize=15)

# 绘制ReLU函数
f = plt.subplot(122)
plt.plot(x, y, color='g')
plt.text(-3, 9, r'$y=ReLU(x)$', fontsize=13)
currentAxis = plt.gca()
currentAxis.xaxis.set_label_text('x', fontsize=15)
currentAxis.yaxis.set_label_text('y', fontsize=15)

plt.show()
```