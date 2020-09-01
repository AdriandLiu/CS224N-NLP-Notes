# L13 - BERT

之前的Word Representation方法[CS224N\(一）：Word Vector](https://zhuanlan.zhihu.com/p/59016893)如**Word2Vec, GloVe, fastText等对每个单词仅有一种表示，而通常单词的含义依赖于其上下文会有所不同，而且每个单词不仅有一方面特征，而应有各方面特征如语义特征，语法特征等**，这一讲集中讨论contextual word representation，主要比较了ELMO，GPT与BERT模型。

### **ELMO**

ELMO的基本思想是利用双向的LSTM结构，对于某个语言模型的目标，在大量文本上进行预训练，从LSTM layer中得到contextual embedding，其中较低层的LSTM代表了比较简单的语法信息，而上层的LSTM捕捉的是依赖于上下文的语义信息。ELMO的全称就是Embeddings from Language Models。对于下游的任务，再将这些不同层的向量线性组合，再做监督学习。

详细来说，对于N个标记 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=%28t_1%2Ct_2%2C...%2Ct_N%29) ,forward language model学习的是根据 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=%28t_1%2Ct_2%2C...%2Ct_%7Bk-1%7D%29) 的信息推测 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=t_k) 的概率：![](https://pic1.zhimg.com/80/v2-93d95e4d8319c460b0c06b9ec3c0d903_1440w.jpg)

而 backward language model学习的是依据 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=%28t_%7Bk%2B1%7D%2C...%2Ct_N%29) 的信息推测![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=t_k) 的概率：![](https://picb.zhimg.com/80/v2-81727a6dee8d7a875383dba08e4c155b_1440w.png)

而bidirectional LSTM就是将两者结合起来，其目标是最大化 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=%5Csum_%7Bk%3D1%7D%5EN%28log+p%28t_k%7Ct_1%2C...%2Ct_%7Bk-1%7D%29%2Blog+p%28t_k%7Ct_%7Bk%2B1%7D%2C...%2Ct_%7BN%7D%29%29)

对于k位置的标记，ELMO模型用2L+1个向量来表示，其中1个是不依赖于上下文的表示，通常是用之前提及的word embedding或者是基于字符的CNN来得到 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=x_k%5E%7BLM%7D) 。L层forward LSTM每层会产生一个依赖于上文的表示 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=%5Cvec%7Bh%7D_%7Bk%2Cj%7D%5E%7BLM%7D+%28j%3D1%2C...%2CL%29) ，同样的，L层backward LSTM每层会产生一个依赖于下文的表示 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=%5Coverleftarrow%7Bh%7D_%7Bk%2Cj%7D%5E%7BLM%7D+%28j%3D1%2C...%2CL%29) ，我们可以将他们一起简计为![](https://pic2.zhimg.com/80/v2-5c14d64f2658ce364818387f3552bc9c_1440w.jpg)

其中 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=h_%7Bk%2C0%7D%5E%7BLM%7D%3Dx_k%5E%7BLM%7D%EF%BC%8Ch_%7Bk%2Cj%7D%5E%7BLM%7D+%3D%5B%5Cvec%7Bh%7D_%7Bk%2Cj%7D%5E%7BLM%7D+%3B%5Coverleftarrow%7Bh%7D_%7Bk%2Cj%7D%5E%7BLM%7D+%5D)

得到每层的embedding后，对于每个下游的任务，我们可以计算其加权的表示![](https://pic1.zhimg.com/80/v2-e84bd8205876446b7d995b5286790957_1440w.png)

其中 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=s%5E%7Btask%7D) 是利用softmax归一化的权重， ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=%5Cgamma%5E%7Btask%7D) 是引入的可调控的scale parameter。

采用了ELMO预训练产生的contextual embedding之后，在各项下游的NLP任务中，准确率都有显著提高。

### **GPT**

GPT全称是Generative Pre-Training, 和之后的BERT模型一样，它的基本结构也是Transformer，关于Transformer结构可以详见之前的总结[Attention机制详解（二）——Self-Attention与Transformer](https://zhuanlan.zhihu.com/p/47282410)。

GPT的核心思想是利用Transformer模型对大量文本进行无监督学习，其目标函数就是语言模型最大化语句序列出现的概率，不过这里的语言模型仅仅是forward单向的，而不是双向的。得到这些embedding后，再对下游的task进行supervised fine-tuning。![](https://pic1.zhimg.com/80/v2-842f9c9d7215005386c9e6b31ae8080e_1440w.jpg)

### **BERT**

BERT原理与GPT有相似之处，不过它利用了双向的信息，因而其全称是Bidirectional Encoder Representations from Transformers。![](https://pic2.zhimg.com/80/v2-6be403efd1d99eb637e2d297cae60ca8_1440w.jpg)

BERT做无监督的pre-training时有两个目标：

* 一个是将输入的文本中 k%的单词遮住，然后预测被遮住的是什么单词。
* 另一个是预测一个句子是否会紧挨着另一个句子出现。

预训练时在大量文本上对这两个目标进行优化，然后再对特定任务进行fine-tuning。

BERT由于采用了Transformer结构能够更好的捕捉全局信息，并且利用了上下文的双向信息，所以其效果要优于之前的方法，它大大提高了各项NLP任务的性能。

如何进行更有效的pre-training，在NLP领域是一个很有意思的问题，其发展也日新月异，BERT在2018年末被提出，而近期XLNet\([https://arxiv.org/abs/1906.08237](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1906.08237)\)则在多项任务上超越了BERT，这一领域的发展值得关注。

