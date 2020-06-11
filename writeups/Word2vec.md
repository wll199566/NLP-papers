# Efficient Estimation of Word Representations in Vector Space

### TL;DR

* Solve the problem of computationally expensive of NNLM by removing the hidden layer
* CBOW: predict the center word according to context words
* Skip-gram: predict the context words according to the center word
* Hierarchical softmax to reduce the computation of the final probability
* Measure the model performance by algebraic operations

### Models

* This paper is to solve the following problem in all previous NNLM works: due to the dense hidden states, it is impossible to train the model on a very large corpus. But it is the fact that larger training samples wll lead to better performance. Therefore, this paper remove the hidden state to obtain a much simpler model and enable it to be trained on a much larger dataset.
* CBOW: (1) embed each context word $C(w_{j-m}) \ldots C(w_{j+m})$ with window size m (2) compute the average of them (3) instead of using a hidden layer, the model feed this averaged vector directly to the output layer and use softmax to compute the probability for the center word. (4) We can also understand it as this way (borrowed from CS224N): there are two matrices, the input embedding and the output embedding, what we are doing is to compute the inner product between the averaged context word and the output embedding of each words, and push them together as similar as possible.
* Skip-gram: (1) embed the center word $C(w_{j})$ (2) feed it into the output layer using a linear layer and use softmax to compute the probability for each context word (3) note that (from CS224N) the for output probability $p(w_{j-m}, \ldots, w_{j+m}|w_{j})$, we assume they are conditionally Independent, which means $p(w_{j-m}, \ldots, w_{j+m}|w_{j}) = p(w_{j-m}|w_{j})p(w_{j-m+1}|w_{j}) \ldots p(w_{j+m}|w_{j})$. Therefore, we can construct the dataset using the pairs (center_word, context_word) and use the one-hot vector for output labels instead the multi-hot vector.
* Since the vocabulary size $|V|$ is huge, computing softmax is very expensize, the papaer use the hierarchical softmax to reduce the compute time to $log|V|$. 
* Training tricks: (1) select 1m most frequent words (2) use Adagrad optimizer (3) linearly decrease the learning rate.

### Evaluation Metrics

The paper uses the algebraic operation evaluation metrics: X = vector(word A) - vector(word B) + vector(word C), where A and B have strong relationship and the relationship between C and X is similar to that between A and B. The paper designs both semantic and syntactic relationships to test models. In evaluation, it first compute the righthand size of the equation, and search for the closest word using cosine similarity. It select the exactly the same word, even if the synonym does not work.

### Conclusion

*  Adding the number of training words,  the dimensionality of word vector and the training time can improve the performance.
* CBOW performs good on syntactic test but poor on semantic test. Skip-gram performs good on both test. 
* Both CBOW and Skip-gram outperform previous NNLM and RNNLM models.

### Question:

when training the model, how to deal with the out-of-bag words which are not in the most frequent words?