# L10 - Question Answering

**问答系统\(Question Answering\)**实际需求很多，比如我们常用的谷歌搜索就可看做是问答系统。通常我们可以将问答系统看做两部分：从海量的文件中，找到与问题相关的可能包含回答的文件，这一过程是传统的information retrieval；从文件或段落中找到相关的答案，这一过程也被称作**Reading Comprehension阅读理解**，也是这一讲关注的重点。

### **SQuAD**

Reading Comprehension需要数据是Passage即文字段落，Question问题以及相应的Answer回答。SQuAD\(Stanford Question Answering Dataset\)就是这样的数据集。对于每个问题都有人类提供的三个标准答案，为了评估问答模型，有两个metric：

1. Exact Match，即模型回答与任意一个标准答案匹配即计数为1，否则为零。统计整体的准确率。
2. F1，即将模型答案与标准答案当做bag of words,计算 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=precison%3D%5Cfrac%7BTP%7D%7BTP%2BFP%7D) , ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=recall%3D%5Cfrac%7BTP%7D%7BTP%2BFN%7D) ,并计算它们的harmonic mean ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=F1%3D%5Cfrac%7B2PR%7D%7BP%2BR%7D) ，然后对所有问题的F1求平均值。

通常F1 score被当做是更可靠的metric。

SQuAD 1.0版本中所有问题的答案都包含在段落中，而2.0版本中引入了一些问题在段落中是没有答案的，对于这类问题，当模型输出没有答案时得分1，否则为0。

当然SQuAD存在其局限性：1.答案需直接截取自段落中的文字，没有是非判断、计数等问题。2.问题的选择依赖于段落，可能与实际中的信息获取需求不同那个。3.几乎没有跨句子之间的理解与推断。

### **Stanford Attentive Reader**

接下来讲的是Manning组的关于QA的模型Stanford Attentive Reader。

其思路是对于Question，先利用Bidirectional LSTM提取其特征向量。![](https://pic3.zhimg.com/80/v2-ef511cc025297ea01625000f868b2c57_1440w.jpg)

![](.gitbook/assets/image%20%28137%29.png)

再对Passage中的每个单词进行Bidirectional LSTM提取其特征向量， 并对每个单词对应的特征向量与问题的特征向量进行Attention操作，分别得到推测答案起始位置与终止位置的attention score，损失函数为 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=L%3D-%5Csum+logP%5E%7Bstart%7D%28a_%7Bstart%7D%29-%5Csum+logP%5E%7Bend%7D%28a_%7Bend%7D%29) 。

![](.gitbook/assets/image%20%283%29.png)



之后还在该模型基础上进行了改进：

1. 对于question部分，不仅仅是LSTM最后的输出，而是用了类似于self-attention的weighted sum来表示，并且增多了BiLSTM的层数。
2. 对于passage的encoding，除了利用Glove得到的word embedding之外，还加入了一些语言学的特征，如POS（part of speech\)和NER\(named entity recognition\) tag以及term frequency。另外还加入了比较简单的特征如段落中单词是否出现在问题中的binary feature，对于这种match，又分为三种即exact match, uncased match\(不区分大小写\), lemma match\(如drive和driving\)。更进一步，还引入了aligned question embedding, 与exact match相比，这可以看做是对于相似却不完全相同的单词（如car与vehicle\)的soft alignment： ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=f_%7Balign%7D%28p_i%29%3D%5Csum_ja_%7Bi%2Cj%7DE%28q_j%29) ，其中 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=a_%7Bi%2Cj%7D) 代表了段落中单词 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=p_i) 与问题中单词 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=q_j) 的相似度。

![](.gitbook/assets/image%20%28102%29.png)

### **BiDAF**

另一个比较重要的QA模型是**BiDAF\(Bi-Directional Attention Flow\)**,其模型结构如下。![](https://pic3.zhimg.com/80/v2-14fbd57f2339731ebd4f2ff4a1126216_1440w.jpg)

![](.gitbook/assets/image%20%2815%29.png)

其核心思想是Attention应该是双向的，既有从Context（即passage）到Query（即Question）的attention,又有从Query到Context的attention。

首先计算similarity matrix![](https://pic3.zhimg.com/80/v2-accbde64e1c2cf13e063a55f886beddb_1440w.png)

![](.gitbook/assets/image%20%2879%29.png)

其中 ![\[&#x516C;&#x5F0F;\]](https://www.zhihu.com/equation?tex=c_i%2C+q_j) 分别代表context vector与query vector。

对于Context2Query Attention，我们想要知道对于每个context word，哪些query word比较重要，因此得到attention score及weighted vector：![](https://pic1.zhimg.com/80/v2-944476dc81e82c7dded91f4ba3a546b9_1440w.jpg)

![](.gitbook/assets/image%20%2812%29.png)

而对于Query2Context Attention，我们想要知道对于query，哪些context words与任意一个query words相似度较高，我们得到对于query来说最关键的context words的加权求和：![](https://pic4.zhimg.com/80/v2-a62a57ee08b95bbb93393e69481302bc_1440w.jpg)

由此，我们得到了Attention Flow Layer 的输出![](https://pic1.zhimg.com/80/v2-f4bf9892baf41294c2bc9e2903a75526_1440w.png)

![](.gitbook/assets/image%20%2848%29.png)

再对其进行多层LSTM与Softmax得到相应的输出。

更近期的发展基本上是更复杂的结构以及attention的各种结合。对于embedding的提取方面，也更多采用contextual embedding，收到了很好的效果，关于contextual embedding如Elmo，BERT等会在第13讲详细讲解。

