# L11 - Convolutional Networks for NLP

CNN \(Convolutional Neural Network\) convolutional neural network has many applications in image processing. In this regard, let's look at the application of CNN in NLP. 

In the previous RNN system \(without using Attention\), we usually use the last hidden vector to represent all the information of the entire sentence, which creates an **information bottleneck. The idea of CNN processing is to calculate a feature vector for all sub-phrases, and finally combine them according to specific tasks. \(Same idea with: consider the entire words vectors for a sub-phrase as an "image"\)**

![](.gitbook/assets/image%20%28143%29.png)

## CNN Architecture

{% embed url="https://app.gitbook.com/@ldhans107/s/deep-learning/convolutional-neural-networks" %}

All architecture follows the ordinary CNN:

![](.gitbook/assets/image%20%28146%29.png)

### Convolutional layer

![](.gitbook/assets/image%20%28144%29.png) 

with multiple feature detectors and will result in multiple feature maps

### Pooling layer

Pooling layer: This is to **decrease the computational power** required to process the data through dimensionality reduction. Furthermore, it is useful for **extracting dominant features** which are rotational and positional invariant, thus maintaining the process of effectively training of the model.

refer here [https://github.com/Adrian107/Interview-Preparation/blob/master/pics/pooling.gif](https://github.com/Adrian107/Interview-Preparation/blob/master/pics/pooling.gif)

#### Max Pooling

![](.gitbook/assets/image%20%28148%29.png)

#### Sub-sampling/ Average pooling

**Feature Maps** -&gt; Extract the average value in the box \(2X2 stride\) -&gt; **Pooled Feature Map**

### **Pooling vs Convolutional layer:**

Pooling: use statistical methods, such as max pooling and average pooling

Convolutional layer: use different feature detector 

### Batch Normalization

![](.gitbook/assets/image%20%28145%29.png)

## CNN in NLP

Consider word vectors of a phrase as an image with vector size of pixels and apply CNN.

![](.gitbook/assets/image%20%28149%29.png)

## CNN application in NLP

### Translation

![](.gitbook/assets/image%20%28147%29.png)

