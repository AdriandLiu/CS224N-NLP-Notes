---
description: 'https://zhuanlan.zhihu.com/p/63397627'
---

# L6 - Language Models and RNNs

## Language Models

### Definition

_Language Modeling is the **task of predicting** what word comes next, also can think of Language model as a system that **assigns probability to a piece of text**_

![](.gitbook/assets/image%20%2884%29.png)

![](.gitbook/assets/image%20%2814%29.png)

![](.gitbook/assets/image%20%2885%29.png)

### Application

Language models are widely used. For example, **typing on a mobile phone can intelligently predict what the next word you want to type, or auto-filling problems when searching on Google,** which are all language model applications. Language models are part of many NLP problems that involve generating text or predicting text probability, such as **speech recognition, handwritten text recognition, automatic error correction, machine translation, and so on**.

## n-gram Language Models

### Definition

![](.gitbook/assets/image%20%2895%29.png)

### Idea

**Collect statistics about how frequent different n-grams are, and use these to predict next word. In other words, the word with highest frequency in n-grams combination will be selected = higher freq, high chances to be selected, based on** _**n**_

![](.gitbook/assets/image%20%28129%29.png)

### Example

![](.gitbook/assets/image%20%2830%29.png)

### Problems

#### Sparsity Problem

In a large corpus, it is possible that the combination of numerator or denominator has not appeared, and its count is zero. And as n increases, the sparsity becomes more serious. 

![](.gitbook/assets/image%20%2891%29.png)

 

#### Storage Problem

We must store the counts corresponding to all n-grams. As n increases, the amount of model storage also increases

![](.gitbook/assets/image%20%2813%29.png)

### Application: Generate text

![](.gitbook/assets/image%20%2869%29.png) ![](.gitbook/assets/image%20%2849%29.png) 

![](.gitbook/assets/image%20%2880%29.png) 

We need to consider more than three words at a time if we want to model language well. But increasing n worsens sparsity problem, and increases model size…



## Windows-based DNN

Combine the word embeddings in the **fixed-length window** together, and use the neural network to do the classification prediction of the next word. **The number of classes is the vocabulary in the corpus:**

![](.gitbook/assets/image%20%2893%29.png)

### Advantage

* No sparsity problem 
* Don’t need to store all observed n-grams

### Disadvantage

![](.gitbook/assets/image%20%2898%29.png) 

## Recurrent Neural Nets

### Core Idea

Apply same weights repeatedly

### Overview of RNN architecture

![](.gitbook/assets/image%20%2850%29.png)

### Advantage

 ![](.gitbook/assets/image%20%28136%29.png) 

### Disadvantage

![](.gitbook/assets/image%20%2887%29.png) 

### Training Overview

Loss/cost function: cross-entropy

Overall loss: average entire training set

![](.gitbook/assets/image%20%2888%29.png)

### Training Procedure

![](.gitbook/assets/image%20%28122%29.png) ![](.gitbook/assets/image%20%28130%29.png) 

![](.gitbook/assets/image%20%28108%29.png) ![](.gitbook/assets/image%20%2846%29.png) 

![](.gitbook/assets/image%20%28115%29.png)

### Problem

![](.gitbook/assets/image%20%2873%29.png)

### Backpropagation

![](.gitbook/assets/image%20%28117%29.png)

Chain rule!

![](.gitbook/assets/image%20%2840%29.png)

### Generating Text

#### Procedure

1. train RNN with any kind of text
2. use this trained RNN to generate text in the style of training text

![](.gitbook/assets/image%20%285%29.png)

### Evaluation

![](.gitbook/assets/image%20%28134%29.png)

That is, the inverse of the probability of our corpus, can also be expressed as the exponential form of the loss function, so the smaller the perplexity, the better our language model. In other words, we want to maximize the probabilities of predict the correct next word by given previous texts.

### Application

1. Tagging - entity recognition: cat:NN; knocked:VBN, etc
2. Sentiment classification
3. Question answering
4. Machine translation

### Unidirectional vs Bidirectional

上面我们讨论的是单向\(unidirectional\)的RNN,对于某些我们具有整个句子的上下文问题（不适用于语言模型，因为语言模型只有上文），我们可以用双向\(bidirectional\)的RNN，例如sentiment analysis问题,对于**the movie was terribly exciting这句话，如果我们只看到terribly的上文，我们会认为这是一个负面的词，但如果结合下文exciting，就会发现terribly是形容exciting的激烈程度，在这里是一个很正面的词，所以我们既要结合上文又要结合下文来处理这一问题**。我们可以用两个分别从前向后以及从后向前读的RNN，并将它们的隐藏向量联结起来，再进行sentiment的classification，其结构如下：

## Why Study Language Model

![](.gitbook/assets/image%20%2855%29.png)



