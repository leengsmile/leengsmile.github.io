---
title: Layer Normalization
date: 2019-03-09 00:46:41
tags: deep learning
mathjax: true

---

在深度学习中, `Batch Normalization` (BN)并不适合RNN结构, BN有以下缺点:

1. 不同的时间步权重不一致, 需要保留每个时间步对应的数据.
2. 序列长度不同. 序列长度决定时间步长度, 使得归一化参数可靠程度不同.
3. 不能用于online 学习. 
4. batch size影响着结果.

针对BN的缺点, Jimmy等人提出的 [Layer normalization](https://arxiv.org/pdf/1607.06450.pdf)(LN),  能够有效地避免BN的缺点. LN只使用当前的样本(序列)

1. 对每一个时间步, 分别计算各层所有神经元**输入** 端的均值和标准差, 并以此归一化.
2. 每一层进行尺度缩放.

LN对神经元的输入做Normalization的步骤如下.

假设当前hidden layer的输入为$\mathbf{a}_{t}$, 则该层所有神经元上对应的均值与标准差分别为

$$
\mu_t = \frac{\sum \limits _{i=1}^{H} a_{t,i}}{H} 
\label{mean_t}
\tag{1}
$$

$$
\sigma_t = \sqrt{\frac{1}{H} \sum \limits _{i=1} ^{H} (a_{t,i} - \mu_t )^2}
\label{eq:sigma1}
\tag{2}
$$

归一化后, rescale,  再进入激活函数

$$
\mathbf{h}_t = f\left(\frac{\mathbf{g}}{\sigma_t} \odot (\mathbf{a}_t - \mu_t) + \mathbf{b}\right)
\label{eq:rescaled-activation}
\tag{3}
$$

在 Jimmy的原文中, 各时间步共享参数$\mathbf{g}​$与$\mathbf{b}​$, 但是在NNDL中, 每一层有自己的缩放因子.

>  It also has only one set of gain and bias parameters shared over all time-steps.

个人觉得应该使用共享的参数, 否则遇到超过训练数据长度的序列时, 将没有对应的参数. 

## 参考文献

[LN] Jimmy L. Ba, Jamie R. Kiros, Geoffrey E. Hinton (2016). Layer Normalization.  <https://arxiv.org/pdf/1607.06450.pdf>

[NNDL] Charu C. Aggarwal. Neural Network and Deep Learning. 



