# GloVe: Global Vectors for Word Representation

### TL;DR

* GloVe combines word2vec with global co-occurance statistics
* When constructing co-occurance matrix, GloVe takes the distance between words into account
* To combat the computational expense of softmax, GloVe discards the denominator of it entirely
* Compared to Word2Vec, GloVe is fast training and have good performance even with smaller corpus size and smaller vector dimension
* Although there is similarity between GloVe and Word2Vec, GloVe does not use methods in Word2Vec like negative sampling or Hierarchical Softmax

### Model

* Like Word2Vec, there are still two vectors for each word: one for center words, one for context words.
* To deal with the problem of summation for softmax, GloVe discards the denominator of it.
* Different from Word2Vec, GloVe merges the word-word co-occurance matrix into the loss function by minimize the squared difference between co-occurance number $X_{ij}$ and its approximation $exp(w_{i}^{T}w_{j})$.
* To combat the problem that $X_{ij}$ is usually very large, which leads to optimization difficulty, GloVe uses logarithm of it
* For each squared difference w.r.t $X_{ij}$, the paper also assigns it a weighted coefficient $f(X_{ij})$ with the form $(x/x_{max})^{\alpha}$ when $x<x_{max}$, and 1 otherwise. The shape of the function is similar to the subsampling function of Word2Vec and coincidently, optimal $\alpha=\frac{3}{4}$ is the same as in Negative Sampling. Also, $f(X_{ij})$ balances the rare words and frequent words by choosing the smart functional shape. 
* The final loss function is $J = \sum_{ij}f(X_{ij})(w_{i}^{T}w_{j}-logX_{ij})$.
* How to compute the co-occurance matrix $X$: before training the GloVe model, it uses the same context window to run through the corpus. For each pair $(w_{i}, w_{j})$, multiply the total counts with the weight $\frac{1}{d}$ which depends on distance $d$ between the center and context word. This is drawn from the intuition that farther words provide less information. 
* Finally, the word vector from the GloVe will be $w_{i} + w_{j}$ for each word.