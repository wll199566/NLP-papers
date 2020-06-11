# A Neural Probabilistic Language Model 

### TL;DR

* First paper to use neural networks to implement language model
* Embed each word with a feature vector and construct a lookup embedding matrix to store them. 
* Use a MLP to compute next word given n-1 preceding words (n-gram)

### Hypothesis

* Distribution representation: a word's meaning is represented by the context words around it. 
* Word similarity: similar words should have similar representation, when computing the joint probability for language model, substitute a word with a similar word will not change the probability too much. 

### Dataset and Preprocessing

* Dataset: Brown Corpus (English texts and books), Associated Press (AP) News
* Stemming
* Keep only frequent words, and map rare words (<=3) to a special symbol
* mapping numeric forms to special symbols, mapping rare words to a special symbol and mapping proper nouns to another special symbol

### Model

<img src="../imgs/nnlm.png" alt="NNLM" style="zoom:33%;" />



* Use look-up table/matrix to embed precedent words $w_{t-n+1}$ to $w_{t-1}$
* Concatenate all embedded vector together and feed it to a linear hidden layer with activation function of tanh
* Put the hidden states to the output layer. The direct connection from the result of feeding the concatenation $x = (C(w_{t-n+1})\ldots C(w_{t-1}))$ via another hidden layer (no activation function) to the output layer is optional.
* Use softmax to compute the probability of the next word $w_{t}$.
* The overall equation: $P(w_{i}=i|context)=Utanh(Hx+b)+Wx+d$.
* Loss function: CrossEntropy + L2 regularization, learnt with SGD
* Evaluation metrics: Perplexity

### Conclusion

* Neural language model outperforms traditional N-gram models by a large margin: 24% on Brown, 8% on AP news.
* NNLM takes advantage of more context words, more precedent words are taken into account, lower perplexity it will get.
* Hidden layer with tanh are useful which increases the depth of the model, enabling it to extract more abstract information
* Whether the direct connection from input to output helps is not clear. Using it decreases the training convergence time, but with higher perplexity.





