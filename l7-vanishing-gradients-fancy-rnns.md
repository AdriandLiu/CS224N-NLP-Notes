# L7 - Vanishing Gradients, Fancy RNNs

## Vanishing Gradients

It’s too difficult for the RNN to learn to preserve information over many timesteps with the problem of vanishing gradient

### Cause

In short, it's due to the chain rule during the backpropagation, multiple multiplies will turn the gradient smaller if the inputs are small.

![](.gitbook/assets/image%20%2832%29.png)

**Details refer to** 

{% embed url="https://app.gitbook.com/@ldhans107/s/common-ml-models-summary/rnn" %}

### Why Vanishing Gradient a problem

![](.gitbook/assets/image%20%28113%29.png)

#### Example

![](.gitbook/assets/image%20%2865%29.png)

### Solution: LSTM

Details refer to 

{% embed url="https://app.gitbook.com/@ldhans107/s/common-ml-models-summary/lstm-gru" %}

![](.gitbook/assets/image%20%2874%29.png)

![](.gitbook/assets/image%20%2852%29.png)

### How does LSTM Solve Vanishing Gradients

{% embed url="https://app.gitbook.com/@ldhans107/s/common-ml-models-summary/lstm-gru\#avoid-gradient-vanishing-how" %}

* **The additive update function for the cell state gives a derivative that's much more ‘well behaved’**
* **The gating functions allow the network to decide how much the gradient vanishes, and can take on different values at each time step. The values that they take on are learned functions of the current input and hidden state.**

### Solution: GRU

![](.gitbook/assets/image%20%28126%29.png)

### Rule of thumb

LSTM is a good default choice \(especially if your data has particularly long dependencies, or you have lots of training data\); Switch to GRUs for speed and fewer parameters.





## Exploding Gradients

Like the cause of vanishing gradient, input/weight is too large, as a result, gradients will get larger

### Why is a problem

![](.gitbook/assets/image%20%2817%29.png)

### Solution

**Gradient clipping**: if the norm of the gradient is greater than some threshold, scale it down before applying SGD update



## Vanishing Gradient only for RNN? NO

![](.gitbook/assets/image%20%2838%29.png)

### Solution

![](.gitbook/assets/image%20%284%29.png)

![](.gitbook/assets/image%20%2826%29.png)

## Unidirectional vs Bidirectional

### Why Bidirectional

上面我们讨论的是单向\(unidirectional\)的RNN,对于某些我们具有整个句子的上下文问题（不适用于语言模型，因为语言模型只有上文），我们可以用双向\(bidirectional\)的RNN，例如sentiment analysis问题,对于**the movie was terribly exciting这句话，如果我们只看到terribly的上文，我们会认为这是一个负面的词，但如果结合下文exciting，就会发现terribly是形容exciting的激烈程度，在这里是一个很正面的词，所以我们既要结合上文又要结合下文来处理这一问题**。我们可以用两个分别从前向后以及从后向前读的RNN，并将它们的隐藏向量联结起来，再进行sentiment的classification，其结构如下：

![](.gitbook/assets/image%20%2856%29.png)

### Note

![](.gitbook/assets/image%20%2892%29.png)

## Multi-layer RNN = Stacked RNN

![](.gitbook/assets/image%20%2829%29.png)

### In practice

![](.gitbook/assets/image%20%2835%29.png)





