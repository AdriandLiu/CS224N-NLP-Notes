# L2 - Word Vectors and Sense

## Review

![](.gitbook/assets/image%20%2828%29.png)

dot product here means the similarity between words



## Optimization

### Gradient descent

Goal: minimize the cost function $$J(\theta)$$ 

Idea: for current value of $$\theta$$ , calculate gradient of $$J(\theta)$$ , then take small step in the direction of negative gradient. Repeat.

![](.gitbook/assets/image%20%2894%29.png)

![](.gitbook/assets/image%20%2821%29.png)

#### Problem

1. $$J(\theta)$$ is a function of all windows in the corpus \(potentially billions!\) - expensive to compute

#### Solution

Stochastic gradient descent \(SGD\): Repeatedly sample windows, and update after each one

![](.gitbook/assets/image%20%2845%29.png)

#### Sparse data

![](.gitbook/assets/image%20%28118%29.png)

because probably in a corpus, there are millions of words

We might only update the word vectors that actually appear!

## Word2Vec details

### **Why each word has two vectors:**

1. one for contextual, one for center**"**
   1. **dog" as a context is not to be considered the same as "dog" as a center word**

      we assume that the words and the contexts come from distinct vocabularies, so that, for example, the vector associated with the word dog will be different from the vector associated with the context dog.
2. we will average both in the end
3. for mathematical efficiency - easier to compute partial derivate \(will take square of the same words, in \)

### **Variants**

* Skip-grams \(SG\) 
  * Predict context \(”outside”\) words \(position independent\) given center word 
* Continuous Bag of Words \(CBOW\) 
  * Predict center word from \(bag of\) context words

### Problem

![](.gitbook/assets/image%20%28114%29.png)

Given the formula of word2vec \(exp, sum, etc\), it is computationally expensive, so **negative sampling** would be helpful



## Skip-grams with negative sampling

N-gram is a basic concept of a \(sub\)sequnece of **consecutive** words taken out of a given sequence \(e.g. sentence\).

"Skip" because the subsequence of the words are not **consecutive**

\*\*\*\*

### Overall objective function:

![](.gitbook/assets/image%20%28124%29.png)

The **first term tries to maximize the probability of occurrence for actual words that lie in the context window,** i.e. they co-occur. While the second term, **tries to iterate over some random words** _**j**_ **that don’t lie in the window and minimize their probability of co-occurrence**.

### **Why this loss function**

![](.gitbook/assets/image%20%2864%29.png)

The highest similarity of u, v is 1, when the first term approaches to 1 \(we want them to close to 1\), the loss will go down, but when the second term approaches to 1 \(we want them to close to 0\), the loss will go up.

## Negative sampling

Negative sampling is a technique to help the training process of Word2Vec be faster and computationally inexpensive. It maximizes the possibilities of the training/**positive**/context pair sample and minimizes the possibilities of the **negative** pair/non-context pair that sampled from the corpus, so that the model is able to learn to distinguish the true pairs from negative ones. There is small trick inside the sampling process, using 3/4 power will make the less frequent words be sampled more often.

### **Overall Procedure**

For each training sample, the model is fed with a _positive_ pair \(a center word and another word that appears in its context\) and a small number of _negative_ pairs \(the center word and a randomly chosen word from the vocabulary\). The model learns to distinguish the true pairs from negative ones.

Let’s say we have a sentence like **“I want a glass of orange juice to go along with my cereal”**. I need to train the model to figure out the context, for e.g. **given the context orange , the word should be juice**. Using negative sampling, we define a supervised learning problem where we generate samples such that given context\(c\), word\(t\) we predict whether it is in context or not with binary target\(y\). 

To generate samples for the sentence above, we first get the sample with **positive case which is context=orange, word=juice and target = 1**. Then we deliberately **generate negative examples \(reason why it is called negative sampling\) like** : 

Context \| word \| target 

orange \| juice \| 1 

orange \| king \| 0 

orange \| book \| 0 

and so on k times. 

We can choose these words for negative samples using various criteria like : -&gt; Frequency in corpus -&gt; P\(wi\) = \(f\(wi\)^3/4\) / \(summation over all words in vocabulary f\(wj\)^3/4\) After generating samples, we can feed it into neural networks and treat each unit as a binary classification problem.

### Why negative sampling

Word2Vec uses Softmax as the last layer and Cross-Entropy loss. Now, the problem with Softmax is that the gradient is dependent on the summation across all classes. In case of word2vec, this means summing across **ALL word in the vocabulary. This is really expensive and will slow down the training**

just select a couple of contexts c1 at random. The end result is that if cat appears in the context of food, then the vector of food is more similar to the vector of cat \(as measures by their dot product\) than the vectors of **several other randomly chosen words \(e.g. democracy, greed,freddy\)**, instead of all other words in language.

### Unigram distribution

#### why unigram:

we only want one work at each time.

#### how:

* We take k negative samples \(using word probabilities\)
* Maximize probability that **real** outside word appears, minimize prob. that **random** words appear around center word

We use unigram distribution to do the negative sampling of k number of words from the corpus \(using words probabilities\). Unigram distribution means all size of 1. 

![](.gitbook/assets/image%20%28125%29.png)

One thing to note is that the probability of a word being selected as a negative sample is related to its frequency of appearance. The higher the frequency of occurrence, the easier it is to be selected as negative words.

#### Why 3/4

The point of the power of 3/4 makes less frequent words be sampled more often is that the words with less frequencies will take 3/4 power, which will increase its value, say $$0.8^{3/4}$$  = 0.845, so the less frequencies words will have higher probabilities to be sampled.

![](.gitbook/assets/image%20%2896%29.png)

The way this selection is implemented in the C code is interesting. They have a large array with **100M** elements \(which they refer to as the **unigram table**\). They **fill this table with the index of each word in the vocabulary** multiple times, and **the number of times a word’s index appears in the table is given by P\(wi\) \* table\_size**. Then, to actually select a negative sample, you just **generate a random integer between 0 and 100M, and use the word at that index in the table,** say, 1, the corresponding word of index of 1 will be selected. Since the higher probability words occur more times in the table, you’re more likely to pick those.







## GloVe \(Global Vector Representation\)

A word embedding algorithm that use ratio co-occurrence probabilities to encode meaning component, it combines the idea of LSA \(singular vector decomposition\) of producing lower dimension vector and Word2vec \(Skip-gram/CBOW\) of context similarity to take their advantages to get the words representation

{% embed url="https://blog.csdn.net/coderTC/article/details/73864097" %}

![](.gitbook/assets/image%20%28133%29.png) ![](.gitbook/assets/image%20%28116%29.png)

**LSA \(Latent semantic analysis\)** produces the low dimensional word vectors by singular value decomposition \(SVD\) on the **co-occurrence** matrix, while 

![](.gitbook/assets/image%20%2890%29.png)

**Word2Vec** employs a three-layer neural network to do the center-context word pair classification task where word vectors are just the by-product



### Co-occurrence matrix:

![](.gitbook/assets/image%20%28109%29.png)

### Cost function

![](.gitbook/assets/image%20%288%29.png)

this is basically the **square loss**, means that the dot product should be similar to the log of co-occurrence probability, and we also add two bias term for each word



**According to the cost function, from personal understanding, we are seeking to let the** $$P(i|j)$$**\(probability of i given j\) be similar to the real co-occurrence probability matrix that counts from dataset, in other words,** _**minimize the difference between the calculated predicted probability and real co-occurrence probability. \(For every pair of word i and j that might co-occur, we try to minimize the difference between the inner product of their word embedding and the log count of i and j.**_ The term _f\(_P_ij\)_ allows us to weight lower some very frequent co-occurrences and cap the importance of very frequent words.

In GloVe, ****we measure the **similarity** of the hidden factors between words to predict their co-occurrence count

\*\*\*\*

![](.gitbook/assets/image%20%2819%29.png)

f\(x\) similar to negative sampling mentioned above, lower freq words will have higher chance to be selected.

### Training

The GloVe model is trained on the non-zero entries of a global word-word co-occurrence matrix, which tabulates how frequently words co-occur with one another in a given corpus. **Populating this matrix requires a single pass through the entire corpus to collect the statistics.** For large corpora, this pass can be computationally expensive, but it is a one-time up-front cost. **Subsequent training iterations are much faster because the number of non-zero matrix entries is typically much smaller than the total number of words in the corpus.**

### **Procedure**

Inspired by [**https://github.com/GradySimon/tensorflow-glove/blob/master/tf\_glove.py**](https://github.com/GradySimon/tensorflow-glove/blob/master/tf_glove.py)\*\*\*\*

* Get the global statistics of the co-occurrence matrix \(as shown above\)

```text
self.__cooccurrence_matrix = {
            (self.__word_to_id[words[0]], self.__word_to_id[words[1]]): count
            for words, count in cooccurrence_counts.items()
            if words[0] in self.__word_to_id and words[1] in self.__word_to_id}
```

* Then this matrix is factorized to a **lower-dimensional \(word x features\) matrix**, where each row now stores a vector representation for each word \(In general, this is done by minimizing a “reconstruction loss”. This loss tries to find the lower-dimensional representations which can explain most of the variance in the high-dimensional data. - Same idea of PCA\)
* Randomly initialize the **two** separate vectors for each word for all words \(dim = vocab size \* embedding size\) in corpus, as well as bias \(dim = vocab size\), this step is same with Word2Vec, two vectors for each word

```text
focal_embeddings = tf.Variable(
    tf.random_uniform([self.vocab_size, self.embedding_size], 1.0, -1.0),
    name="focal_embeddings")
context_embeddings = tf.Variable(
    tf.random_uniform([self.vocab_size, self.embedding_size], 1.0, -1.0),
    name="context_embeddings")

focal_biases = tf.Variable(tf.random_uniform([self.vocab_size], 1.0, -1.0),
                           name='focal_biases')
context_biases = tf.Variable(tf.random_uniform([self.vocab_size], 1.0, -1.0),
                             name="context_biases")
```

* Perpare the training batches, which include the two words \(i and j\) vectors and their corresponding count for the entire co-occurrence matrix \(for all combination of words\).

```text
cooccurrences = [(word_ids[0], word_ids[1], count)
                         for word_ids, count in self.__cooccurrence_matrix.items()]
        i_indices, j_indices, counts = zip(*cooccurrences)
```

* Loop through all the batches, use the loss function and AdaGrad to update **each word vector** in order to minminze the difference between the real count and similarity of two words vectors
* After converage, we will get **all** words embeddings that cloest to the real count between itself and its context embedding.

### Advantage

* Fast training: Unlike window-based methods, where we used to optimize one window at a time \(might train over the same co-occurrence multiple times\), in GloVe we just optimize one count at a time.
* Scalable: Rare words in a huge corpora don’t occur very often, but GloVe allows us to capture the semantics irrespective of the number of occurrences of a word.
* Efficient use of statistics: It helps in the model performing well even on small corpus and small vector sizes.

## Word2Vec vs GloVe

Both models learn geometrical encodings \(vectors\) of words from their co-occurrence information \(how frequently they appear together in large text corpora\). They differ in that **word2vec is a "predictive" model, whereas GloVe is a "count-based" model.** See this paper for more on the distinctions between these two approaches: [http://clic.cimec.unitn.it/marco/publications/acl2014/baroni-etal-countpredict-acl2014.pdf](http://clic.cimec.unitn.it/marco/publications/acl2014/baroni-etal-countpredict-acl2014.pdf).

**Predictive models learn their vectors in order to improve their predictive ability of Loss\(target word \| context words; Vectors\),** i.e. the loss of predicting the target words from the context words given the vector representations. In word2vec, this is cast as a feed-forward neural network and optimized as such using SGD, etc.

**Count-based models learn their vectors by essentially doing dimensionality reduction on the co-occurrence counts matrix.** They first construct a large matrix of \(words x context\) co-occurrence information, i.e. for each "word" \(the rows\), you count how frequently we see this word in some "context" \(the columns\) in a large corpus. The number of "contexts" is of course large, since it is essentially combinatorial in size. So then they factorize this matrix to yield a lower-dimensional \(word x features\) matrix, where each row now yields a vector representation for each word. **In general, this is done by minimizing a "reconstruction loss" which tries to find the lower-dimensional representations which can explain most of the variance in the high-dimensional data**. In the specific case of GloVe, the counts matrix is preprocessed by normalizing the counts and log-smoothing them. This turns out to be A Good Thing in terms of the quality of the learned representations.

However, as pointed out, when we control for all the training hyper-parameters, the embeddings generated using the two methods tend to **perform very similarly** in downstream NLP tasks. The additional **benefits of GloVe over word2vec is that it is easier to parallelize the implementation which means it's easier to train over more data**, which, with these models, is always A Good Thing.



## Evaluation

![](.gitbook/assets/image%20%2860%29.png)

### Intrinsic

![](.gitbook/assets/image%20%281%29.png)









 





