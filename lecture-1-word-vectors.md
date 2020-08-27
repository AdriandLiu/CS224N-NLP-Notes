# L1 - Word Vectors

## How to represent a meaning of word:

1. Human: By definition 
2. Computer: Use e.g. **WordNet**, a thesaurus containing lists of synonym sets and hypernyms \(“is a” relationships\)

### Problem with WordNet

1. Great as a resource but missing nuance \(细微差别\) 
   1. e.g. “proficient” is listed as a synonym for “good”. This is only correct in some contexts. 
2. Missing new meanings of words 
   1. e.g., wicked, badass, nifty, wizard, genius, ninja, bombest 
3. Impossible to keep up-to-date! 
4. Subjective 
5. Requires human labor to create and adapt  
6. Can’t compute accurate word similarity

## One-hot encoding

motel = \[0 0 0 0 0 0 0 0 0 0 1 0 0 0 0\] 

hotel = \[0 0 0 0 0 0 0 1 0 0 0 0 0 0 0\]

**Vector dimension** = number of words in vocabulary \(e.g., 500,000\) \(each word has its corrsponding one-hot vector\), vector would be enormous, millions of words, etc

### Problem with one-hot encoding

#### Example:

Search for "Seattle motel", suppose to have similarity with "Seattle hotel", but their vectors are

* motel = \[0 0 0 0 0 0 0 0 0 0 1 0 0 0 0\] 
* hotel = \[0 0 0 0 0 0 0 1 0 0 0 0 0 0 0\] 

These two vectors are **orthogonal**. There is **no natural notion of similarity** for one-hot vectors!

### Solution

* WordNet? Fail badly: incompleteness, etc
* **Learn to encode similarity in the vectors themselves**

## **Representing words by their context**

**Context words:**

* …government debt problems turning into **banking** crises as happened in 2009… 
* …saying that Europe needs unified **banking** regulation to replace the hodgepodge… 
* …India has just given its **banking** system a shot in the arm…

### Word vector

![](.gitbook/assets/image%20%2881%29.png)



_**Word vectors**_ **are sometimes called** _**word embeddings**_ **or** _**word representations**_**. They are a** _**distributed representation**_

### **Visualisation**

![](.gitbook/assets/image%20%2861%29.png)

## **Word2Vec**

#### Idea: to see how close they are in the vector space 

* We have a large corpus of text  
* Every word in a fixed vocabulary is represented by a vector 
* Go through each position t in the text, which has a center word c and context \(“outside”\) words o 
* Use the **similarity of the word vectors** for c and o to **calculate the probability** of o given c \(or vice versa\) 
* **Keep adjusting the word vectors** to maximize this probability

#### Example

![](.gitbook/assets/image%20%2827%29.png)

![](.gitbook/assets/image%20%289%29.png)

### Objective function

![](.gitbook/assets/image%20%28106%29.png)

![](.gitbook/assets/image%20%287%29.png)

### Prediction function

![](.gitbook/assets/image%20%28127%29.png)

### Partial derivative

![](.gitbook/assets/image%20%2847%29.png)

![](.gitbook/assets/image%20%2877%29.png)

### Train the model

![](.gitbook/assets/image%20%2844%29.png)

#### **Why each word has two vectors:**

1. one for contextual, one for center**"**
   1. **dog" as a context is not to be considered the same as "dog" as a center word**

      we assume that the words and the contexts come from distinct vocabularies, so that, for example, the vector associated with the word dog will be different from the vector associated with the context dog.
2. we will average both in the end
3. for mathematically efficiency - easier to compute partial derivate \(will take square of the same words, in \)



![](.gitbook/assets/image%20%2810%29.png)







![](.gitbook/assets/image%20%2851%29.png)



### Variants

* Skip-grams \(SG\) 
  * Predict context \(”outside”\) words \(position independent\) given center word 
* Continuous Bag of Words \(CBOW\) 
  * Predict center word from \(bag of\) context words



### Optimisation

![](.gitbook/assets/image%20%28140%29.png)

![](.gitbook/assets/image%20%2825%29.png)























