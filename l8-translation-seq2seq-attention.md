---
description: 'https://zhuanlan.zhihu.com/p/64887738'
---

# L8 - Translation, Seq2Seq, Attention

![](.gitbook/assets/image%20%2833%29.png)

Many of the early machine translations were **rule-based,** and then the Statistical Machine Translation \(**SMT**\), which relied on statistical information, was gradually developed. Later, the Neural Machine Translation \(**NMT**\), which used neural networks to greatly improve the accuracy, was developed. It relies on the **Sequence-to-Sequence** model architecture and the **Attention** mechanism further improves it.



## Machine Translation

![](.gitbook/assets/image%20%28100%29.png)

## Statistical Machine Translation

### Core Idea

Learn a **probabilistic** model from data

### Language and Translation Model

![](.gitbook/assets/image%20%2816%29.png)

### Train Translation Model

1. Large amount of **parallel** data: pairs of human-translated French/English sentences
2. Learn the alignment ![](.gitbook/assets/image%20%2862%29.png) 

### Problem with alignment

1. Alignment might be one-to-many, many-to-one, or many-to-many, very hard to do so![](.gitbook/assets/image%20%2824%29.png) 

### Problem with SMT

![](.gitbook/assets/image%20%2822%29.png)



## Neural Machine Translation

### Definition

![](.gitbook/assets/image%20%2897%29.png)

### Seq2seq

![](.gitbook/assets/image%20%2823%29.png)

### Application of Seq2seq

![](.gitbook/assets/image%20%2834%29.png)

### Conditional Language Model - Seq2seq

![](.gitbook/assets/image%20%28110%29.png)

### Training

![](.gitbook/assets/image%20%2876%29.png)

### Cost Function

![](.gitbook/assets/image%20%28123%29.png)

### Advantage

NMT的优势是我们可以整体的优化模型，而不是需要分开若干个模型各自优化，并且需要feature engineering较少，且模型更灵活，准确度也更高。其缺点是更难理解也更纠错，且很难设定一些人为的规则来进行控制。

![](.gitbook/assets/image%20%2854%29.png)

### Disadvantages

![](.gitbook/assets/image%20%2866%29.png)

## **Optimization of Seq2seq in Decoder**

### Greedy Decoding

_means_ ****take the most probable word on each step

![](.gitbook/assets/image%20%28138%29.png)

### Problem with Greedy Decoding

可能当前的最大概率的单词对于翻译整个句子来讲不一定是最优的选择，但由于每次我们都做greedy的选择我们没机会选择另一条整体最优的路径。

![](.gitbook/assets/image%20%28135%29.png)

### Solution: Beam search decoding

**Overview**: it walks through every possible path of $$K^2$$ number of paths/hypotheses and selects the path/hypotheses with the normalized highest score as the final decoding result



On each step of decoder, keep track of the **k most probable** partial translations \(which we call hypotheses\) • k is the beam size \(in practice around 5 to 10\)

![](.gitbook/assets/image%20%28111%29.png)

#### Beam Search Example

![](.gitbook/assets/image%20%2837%29.png)

### Stopping Criterion

![](.gitbook/assets/image.png)

Note: different hypotheses refer to different K, there are $$K^2$$ number of path, means that the walking path has number of $$K^2$$, they all may pass the different words.

### Problem with Beam Search

Longer hypotheses/paths = lower score \(b/c the score is negative number\)

\*\*\*\*![](.gitbook/assets/image%20%28139%29.png) ****

## **Evaluation of Machine Translation**

BLEU基本思想是看你machine translation中n-gram在reference translation（人工翻译作为reference）中相应出现的几率。但是可能会遇到翻译的很好但是分数很低的情况

![](.gitbook/assets/image%20%2883%29.png)

![](.gitbook/assets/image%20%2875%29.png)

## Difficulties Remain

![](.gitbook/assets/image%20%28112%29.png)

## Attention

### Why Attention

Attention is designed to solve the seq2seq problem: there is a bottleneck between encoder and decoder, it was used to transfer the encoded information, which contains all the contextual information from source sentence, to the decoder. In other words, only one vector that contains all source sentence information will be passed into decoder, this causes the **problem:** 

1. Decoder forces all of the information about the source sentence from one vector, what if some information isn't in the vector \(put too much pressure to one single vector to represent entire sentence\)
2. It will lose some information when the sequence gets longer

### Definition

Attention mechanism pays specific attention on one specific part of vector when translating one specific sentence by calculating the attention score \(a scalar, dot product of two vectors, also named _similarity_\) 

### Training Procedure

1. Take the hidden state that obtained from encoder
2. Take the dot product of the current decoder hidden state and all of the hidden states inside the encoder to get the attention score \(a scalar, check their similarity\)
3. Take the softmax across all the attention score to get the attention distribution \(sum to 1\)
4. Take the weighted concatenate of softmax-ed attention score and the hidden state to get the context vector ![](.gitbook/assets/image%20%2858%29.png) 
5. Take the concatenate of context vector and current decoder hidden state in order to 对于当前输出位置得到比较重要的输入位置的权重，在预测输出时相应的会占较大的比重
6. Repeat 2 - 5 until the last word

![](.gitbook/assets/image%20%2820%29.png) ![](.gitbook/assets/image%20%28104%29.png)

 ![](.gitbook/assets/image%20%2859%29.png) ![](.gitbook/assets/image%20%2842%29.png)

![](https://picb.zhimg.com/v2-ef925bd2adec5f51836262527e5fa03b_b.webp)



![](https://miro.medium.com/max/1400/1*wBHsGZ-BdmTKS7b-BtkqFQ.gif)

![](.gitbook/assets/image%20%2841%29.png)

### Procedure in Equation 

![](.gitbook/assets/image%20%2867%29.png)



### Advantage

![](.gitbook/assets/image%20%28131%29.png)



##  

\*\*\*\*

