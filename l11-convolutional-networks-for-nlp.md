# L11 - Convolutional Networks for NLP

CNN\(Convolutional Neural Network\)卷积神经网络在图像处理方面应用很多，这一讲来看看CNN在NLP中的应用。

之前的RNN系统中（不利用Attention的情况下），通常我们用最后的hidden vector来表示整个句子的所有信息，这就造成了信息的瓶颈。**而CNN处理的思路是对于所有的子短语，都计算一个特征向量，最后再根据具体的任务将它们结合在一起。**

## CNN Architecture



