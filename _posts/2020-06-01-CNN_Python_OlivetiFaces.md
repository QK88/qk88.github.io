---
layout:     post
title:      CNN_Python_OlivetiFaces
subtitle:   
date:       2020-06-01
author:     QK
catalog: true
tags:
    - CNN
    - neural network
    - opencv
    - pytorch
    - python

---

<!-- MarkdownTOC -->

利用经典数据集OlivetiFaces来训练卷积神经网络，为了提高神经网络适用性，对图像进行旋转镜像缩放等操作以扩充数据集以训练神经网络



## 神经网络

​	为什么不选择其他的？

​	机器学习的类型与很多种，最经典简单的当属回归函数，近些年随着深度学习大火，神经网络又进入人们的视野。分类算法里LR（逻辑线性回归）或者linear SVM等经典算法多用于线性分割，即为找到一条最合理的分界线将数据分割开来

<img src="{{sit.url}}/img/CNN\linear.png" style="zoom:50%;" />

​	对于非线性可分的样本，可以加一些kernel核函数或者特征的映射（如维度变化）使其成为一个曲线或者一个曲面将样本分开。但是由于数据不可控性，可能会出现诸多问题。

​	为什么选择神经网络？

​	线性函数分割能力有限，因此一种非线性的解决方式被人们所需求。——神经网络的出现很好的解决了这一问题，用一种可以处理线性问题的神经元使用激活函数激活（一种非线性操作），多层叠加理论上只要有足够的计算量可以在一定空间内分割任意边界。

<img src="{{sit.url}}/img/CNN\network.png" style="zoom:40%;" />

## CNN(卷积神经网络)

​	卷积神经网络依旧是层级网络， 但层的功能和形式做了变化。层级结构可参照下图调整

<img src="{{sit.url}}/img/CNN\cnn.png" style="zoom:40%;" />

**层级结构**

​	其层级结构包括：数据输入层/ Input layer，卷积计算层 / CONV layer， ReLU 激励层 / ReLU layer，池化层 / Pooling layer，全连接层 / FC layer

有3种常见的图像数据处理方式

1. 数据输入层

```
去均值：把输入数据各个维度都中心化到0，也就是算出所有样本的平均值，再让所有样本减去这个均值

归一化：幅度归一化到同样的范围，比如把样本数据压缩到0-1

PCA/白化：用PCA降维，白化是对数据每个特征轴上的幅度归一化

CNN在图像上的处理往往只有去均值。
```

2. 卷积层

   到了卷积层，它不再是全连接了，而是局部关联。每个神经元看做一个filter。通过对窗口(receptive field)做滑动操作，filter对局部数据计算

<div align='center'>
<img src="{{sit.url}}/img/CNN\conv.png" style="zoom:60%;" />
</div>

3. 激活层

   把卷积层输出结果做非线性映射，此为非线性操作，常见的激活函数有Sigmoid，Tanh(双曲正切)，ReLU，Leaky ReLU，ELU，Maxout

4. 池化层

   它的位置一般是夹在连续的卷积层中间，作用是压缩数据和参数的量，减小过拟合

   <img src="{{sit.url}}/img/CNN\pool.png" alt="p" style="zoom:80%;" />

5. 全连接层 / FC layer

   两层之间所有神经元都有权重连接，通常全连接层在卷积神经网络尾部。业界人解释放一个FC layer的主要目的是最大可能的利用现在经过窗口滑动和池化后保留下的少量的信息还原原来的输入信息
   
6. CNN的一般结构可归结为：

   1）INPUT 

   2) [[CONV -> RELU]\*N -> POOL]\*M  

    3) [FC -> RELU]\*K 或FC

### 训练注意事项

**1.用Mini-batch SGD对神经网络做训练的过程如下：** 

**repeat**：

①  采样一batch 数据

②前向计算得到损失 loss

③  反向传播计算梯度

④  用这部分梯度迭代更新权重参数

**2.权重初始化**

1）用均值为0的高斯函数，随机取一些点去初始化W。这种初始化方法对于层次不深的神经网络 OK，深层网络容易带来整个网络( 激活传递)的不对称性

2）当激励函数是sigmoid函数时，输入层神经元个数为input，输出层为output，则输出层的W可为input和output之间的一个数/input的平方根

3）当激励函数变成当今最为流行ReLu函数时，以上方式又失效了，除以（input/2）的平方根是可以的

 关于Batch Normalization：通常在全连接层后 ， 激励层前做如下操作

![img]({{sit.url}}/img/CNN\batchNormalization.png)

它的作用是自动的约束输出不会发散，导致导致整个网络的训练死掉，具体的官方的好处有如下四点：

1）梯度传递（计算 ）更为顺畅 

2）学习率设高一点也没关系

3）对于初始值的依赖减少了！！  

4）其实这里也可以看做一种正则化 ， 减少了对dropout的需求。

**3.Dropout**

 这是一种防止过拟合的一种正则化方式，以前的正则化方式是在loss中加上所有的W，但在神经网络中不可行，因为w的数量太大了，不仅会使loss值很大，也会浪费很多时间在计算w的和上。简单的理解dropout就是别一次开启所有学习单元

 <img src="{{sit.url}}/img/CNN\dropout.png" alt="img" style="zoom:80%;" />

dropout一般在训练阶段使用，在测试或者预测时并不会去dropout，工业上的做法是在输入的X上乘以P得到X的期望，或者输入不做变化而是对所有的有dropout层都做X/p

dropout能防止过拟合的的理解方式：

理解一：别让你的神经网络记住那么多东西

理解二：每次都关掉一部分感知器 ， 得到一个新模型 ， 最后做融合