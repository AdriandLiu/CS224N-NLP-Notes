# L5 - Dependency Parsing

## Syntactic Structure

Phrase structure organizes words into nested constituents

### Consistency

**Constituency = phrase structure grammar = context-free grammars \(CFGs\)**

For example: we can use phrase to determine if the word is noun, adv, v, etc

![](.gitbook/assets/image%20%28141%29.png)

### Dependency

Dependency structure shows which words depend on \(modify or are arguments of\) which other words.

![](.gitbook/assets/image%20%2870%29.png)

**We want what word modifies the other words**

#### Problem

_Ambiguity \(same sentence represents different meaning\)_

![](.gitbook/assets/image%20%28132%29.png)

### Why do we need sentence structure? 

We need to understand sentence structure in order to be able to interpret language correctly Humans communicate complex ideas by composing words together into bigger units to convey complex meanings We need to know what is connected to what

## Dependency Grammar and Dependency Structure

Dependency syntax postulates\(假定\) that syntactic\(句法\) structure consists of relations between lexical\(词汇的\) items, normally binary asymmetric relations \(“arrows”\) called **dependencies**

![](.gitbook/assets/image%20%2886%29.png)

The arrows are commonly typed with the name of grammatical relations \(subject, prepositional object, apposition, etc.\)

![](.gitbook/assets/image%20%2872%29.png)

### Universal treebank

Universal Dependencies: [http://universaldependencies.org/](http://universaldependencies.org/) 

![](.gitbook/assets/image%20%2839%29.png)

#### Advantage:

* Reusability of the labor
  * Many parsers, part-of-speech taggers, etc. can be built on it
  * Valuable resource for linguistics 
* Broad coverage, not just a few intuitions 
* Frequencies and distributional information 
* A way to evaluate systems

### Sources of information for dependency parsing?

![](.gitbook/assets/image%20%286%29.png)

### Methods of dependency parsing

![](.gitbook/assets/image%20%2857%29.png)

#### Basic transition-based dependency parser

![](.gitbook/assets/image%20%28105%29.png)

![](.gitbook/assets/image%20%282%29.png)

### Evaluation

我们有两个metric，一个是LAS（labeled attachment score）即只有arc的箭头方向以及语法关系均正确时才算正确，以及UAS（unlabeled attachment score）即只要arc的箭头方向正确即可。

![](.gitbook/assets/image%20%2899%29.png)

### **Transition-based Dependency Parsing**

![](.gitbook/assets/image%20%28120%29.png)

### **Neural Dependency Parsing**

![](.gitbook/assets/image%20%2868%29.png)

![](.gitbook/assets/image%20%2853%29.png)

\*\*\*\*













