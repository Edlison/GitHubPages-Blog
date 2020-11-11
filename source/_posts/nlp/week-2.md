# Week 2
深度学习与神经网络基础，pytorch基础。

## 神经网络训练过程

- 将DataSet生成iter(train_iter, test_iter), 每个iter由X,y构成 (X:Sample的所有特征[samples * features], y:真实标签).
- 计算y_hat (y_hat:通过构造的神经网络模型得到的预测值).
- 计算损失函数 (均方差, 交叉熵).
- 对所有params梯度清零.
- loss.backward反向传播, 对损失函数求梯度, 也就是得所有的到权重和偏置的梯度param.grad.
- params带入梯度下降算法, 更新params.
- 每个epoch后, 将测试数据带入训练后的网络模型中计算准确率.

## 注意
**1. W初始化**  
首先介绍一下我们不应该做的事情（即初始化为0）。需要注意的是我们并不知道在训练神经网络中每一个权重最后的值，但是如果进行了恰当的数据归一化后，我们可以有理由认为有一半的权重是正的，另一半是负的。令所有权重都初始化为0这个一个听起来还蛮合理的想法也许是一个我们假设中最好的一个假设了。但结果正确是一个错误(的想法)，因为如果神经网络计算出来的输出值都一个样，那么反向传播算法计算出来的梯度值一样，并且参数更新值也一样(w=w−α∗dw)。更一般地说，如果权重初始化为同一个值，网络就不可能不对称(即是对称的)。

**2. 梯度清零**  
每次反向传播前需要将params的梯度清零，否则每次计算的梯度为上一个梯度的累加。

**3. 反向传播**  
计算的是给定图的叶子结点的梯度，对Loss函数进行反向传播计算所得即为所有的W与b。

**4. python方法**  
method()代表执行完该函数  
method仅函数对象

**5. 二分类问题不小心将最后一层设为10维**  
最后训练出来的权重使得结果在0, 1上的概率更大，不排除最后的结果为2 ~ 9。

**6. 网络的层次**  
每一层为激活函数

层数-1 为连接层 连接层对应[W, b]

X = [batch_size * features]  

- 第一层输入为([-1, features])  

- 矩阵计算: [-1, features] * [W1(input_num, hidden_num)] + b1  

- 隐藏层输入为(input_num, hidden_num)  

- 矩阵计算: [-1, hidden_num] * [W2(hidden_num, output_num)] + b2  

- 输出层为(hidden_num, output_num)  

- 最终矩阵格式: [-1, output_num]

**7. 特殊第一层input输入层**  
输入的特征可能type为Long, 一定要转为float.  
batch_size与input_num无关, 但features一定与input_num相等.

**8. X.permute**  
model的forward必须要X.permute(1, 0)  
`LSTM(batch_first=True)`没用？？？

**9. python路径问题**  
相对路径取决于调用该函数的python文件位置，不是子函数所在python文件相对于文件的位置。

**10. pytorch使用GPU计算**  
- 训练及评价集需要X.cuda()将数据移至GPU  
- model与loss函数需要model.cuda(), loss.cuda()移至GPU

**11. 导出数据**  
TestLoader导出预测结果时，一定不能将导入的数据Shuffle!!!

**12. pad**  
`<pad>`对应一个随机生成的向量

**13. torch.no_grad()**  
- requires_grad=True 要求计算梯度
- requires_grad=False 不要求计算梯度
- with torch.no_grad()或者@torch.no_grad()中的数据不需要计算梯度，也不会进行反向传播

即使一个tensor（命名为x）的requires_grad = True，由x得到的新tensor（命名为w-标量）requires_grad也为False，且grad_fn也为None,即不会对w求导。
```py
x = torch.randn(10, 5, requires_grad = True)
y = torch.randn(10, 5, requires_grad = True)
z = torch.randn(10, 5, requires_grad = True)
with torch.no_grad():
    w = x + y + z
    print(w.requires_grad)
    print(w.grad_fn)
    print(w.requires_grad)
False
None
False
```

**14. train()/eval()**  
- 在训练模型时，前面加上`model.train()`  
启用 BatchNormalization 和 Dropout

- 在测试模型时，前面加上`model.eval()`  
不启用 BatchNormalization 和 Dropout  
即使不训练，它也会改变权值。这是model中含有batch normalization层所带来的的性质。


## 问题
最后一层输出不加激活函数 含义(最后预测值是数值最大的 因此没有影响)？？？

隐藏层没有激活函数 含义？？？

`LSTM(batch_first=True)`没用？？？

embedding_layer(X)中X.shape为(batch,seq)或(seq,batch)没影响？？？
若batch_first了embedding后要(batch,seq)后传入rnn？？？

lstm门使用sigmoid代表门开程度，而不是四舍五入为0或1？？？



## 词向量框架

- 拿到每个样本
- 每个样本分词
- 构建词典{word: index}
- 对每个Sample标准化处理， 长度不足补`<pad>`
- 将每个Sample中的word对应index，没有的对应`<pad>`的index
- 获取预训练的词向量
