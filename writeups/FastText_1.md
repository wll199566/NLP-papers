# Enriching Word Vectors with Subword Information

This is the first paper of [fastText series](https://fasttext.cc/).

### TL;DR

* Word2Vec assigns a vector for each word, which ignores the subword information in the some morphologically rich languanges
* The paper is mainly based on Skip-gram model
* For context word, as Skip-gram, the paper assigns each one a single vector
* For center word, the paper extracts character-level N-grams and use the summation of gram-vector to represent the word. 
* Word vectors with subword information performs better on mophologically rich languages, English affixes and suffixes, and rare words. 

### Motivations

* For words, especially those languages which have more than one forms for some verbs and nouns, such as Finnish and French, subword information is very important since all those mophology has the same meaning and root form.
* At the same time, Word2Vec cannot deal with the unknown words which appear in test set using word-level representation
* For subtle structures in English like affixes and suffixes, they can be shared across multiple words, but Word2Vec does not consider that or design a parameter sharing scheme to deal with this.

### Model

* The model uses Skip-gram as the basis model
* For context words, the paper assigns word vectors for each of them
* For center words, the paper first adds the boundery < and > to segment the word out, and then extracts N-grams of characters in each word. Then the paper assigns each n-gram a vector to represent them. Finally, all vectors will be summed as the center word representation, and dot product with the context word vector to generate the score function which will be fed into softmax layer
* At test time, when there is the corresponding word-level vector for a word, then use it as the word vector, otherwise, sum character level n-grams together as the word representation 

### Conclusion

* When evaluated on less frequent words, using similarity at character level between words can learn a very good word vector
* Word vector considering subword information can greatly improve the performance on syntactic tasks, but for semantic tasks, it will not help
* Subword information is very important for mophologically rich languages
* It is important to include long N-gram, especially N=5 or 6, and larger N will perform better on semantic tasks

### Question:

* Is it possible that character-level N-grams does not appear in the training set? If it is the case, then what to do?
* Like Word2Vec, this paper does not consider the global statistics like stated in the GloVe paper. How to combine character-level global statistics along with word-level global statistics together?