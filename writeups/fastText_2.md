# Bag of Tricks for Efficient Text Classification

This is the second paper for [fastText](https://fasttext.cc/). The first is [here](./FastText_1.md). 

### TL;DR

* The paper uses a simple linear model which is similar to CBOW for text classification task
* The method greatly improve the training and test time while achieving the similar performance of state-of-the-art deep learning methods.
* It turns out the order of words is very important for text classification

### Motivation

* Deep learning models achieve a good performance on text classification tasks, but the speed to train and test them are so slow due to the overparameters.
* Linear models like SVM is fast to train, but it needs well-designed input features which cannot be shared with all words
* This paper tends to use log-bilinear models to speed-up the training and testing time for text classification, while maintaining similar performance as SOTA methods.

### Model

* The model models a text using a bag-of-word model and give each word a vector to represent

* The model has a similar architecture as CBOW model, except that

  (1) input features all words in a text

  (2) output is the a vector for classification label, instead of the center word

  (3) there is a linear hidden layer between input features and output vector

* Along with the word, the paper also uses word-level N-grams for the features to capture the order information in the text

* To further improve the speed, Hierachical softmax are used to compute softmax and hashing function are used to retrieve N-gram vectors 

### Conclusion

* The model can greatly shorten the training and test time (hours vs. second) compared to SOTA deep learning based models while getting a similar performance
* It turns out adding N-grams for additional features along with bag-of-word can improve the performance for text classification, due to the local word order information it captures 