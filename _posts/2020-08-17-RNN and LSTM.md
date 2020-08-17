---
layout: post
title: LSTM
date: 2020-08-17
categories: blog
tags: [DL, NLP]
description: the introduction for LSTM.
---

<!-- We are given a training dataset of n points:<span>![](http://latex.codecogs.com/gif.latex?(x_1,y_1),\\dots,(x_n,y_n))</span>
 -->

## RNN
### Motivation
If we had separate parameters for each value of the time index, we could not share statistical strength across different sequence lengths and across different positions in time. For example "I go to Beijing in 2010" and "In 2010, I go to Beijing". When we choose a feedforward network that processes sentences of fixed length, it would have separate parameters for each input feature, so it would need to learn all of the rules of the language separately at each position in the sentence. By comparison, RNN shares the weights across several time steps.

RNN use <span>![](http://latex.codecogs.com/gif.latex?h^{(t)} = f(h^{(t-1)},x^{(t)};\\theta))</span>

When RNN is trained to perform a task that requires predicting the future from the past, the network typically learns to use <span>![](http://latex.codecogs.com/gif.latex?h^{(t)})</span> as a kind of lossy summary of the task-relevant aspects of the past sequence of inputs up to t.This summary is in general necessarily lossy, since it maps an arbitrary length sequence to a fixed length vector.

Its advantages:
1. Regardless of the sequence length, the learned model always has the same input size, because it is specified in terms of transition from one state to another state, rather than specified in terms of a variable-length history of states.
2. It is possible to use the same transition function f with the same parameters at every time step.    

### RNN
#### Patterns
Base on mativation before, we can construct several kinds of RNN. Some examples of important design patterns for recurrent neural networks include the following:
- RNN that produce an output at each time step and have recurrent connections between hidden units.
![](https://github.com/Little0o0/Little0o0.github.io/blob/master/img/RNN_Pattern1.png)
- RNN that produce an output at each time step and have recurrent connections only from the output at one time step to the hidden units at the next time step. It is less powerful than those in family represented by pattern1 because o has less information than h.
![](https://github.com/Little0o0/Little0o0.github.io/blob/master/img/RNN_Pattern2.png)
-RNN with recurrent connections between hidden units, that read an entire sequence and then produce a single output, illustrated in figure
![](https://github.com/Little0o0/Little0o0.github.io/blob/master/img/RNN_Pattern3.png)

#### Update Equations
t = 1 to t = t0 , we apply the following update equations:
![](http://latex.codecogs.com/gif.latex?a^{(t)} = b + Wh^{(t-1)} + Ux^{(t)})
![](http://latex.codecogs.com/gif.latex?h^{(t)} = tanh(a^{(t)}))
![](http://latex.codecogs.com/gif.latex?o^{(t)} = c + Vh^{(t)})
![](http://latex.codecogs.com/gif.latex?\\bar{y}^{(t)} = softmax(o^{(t)}))

where the parameters are the bias vectors b and c along with the weight matrices U, V and W, respectively for input-to-hidden, hidden-to-output and hidden-tohidden connections.

#### Loss
The total loss for a given sequence of x values paired with a sequence of y values would then be just the sum of the losses over all the time steps.

take <span>![](http://latex.codecogs.com/gif.latex?L^{t}) </span> for example, which is the is the negative log-likelihood of <span>![](http://latex.codecogs.com/gif.latex?y^{t}) </span>. Thus the total Loss L is:
![](http://latex.codecogs.com/gif.latex?L(\\{x^{(1)},\\dots,x^{(t0)}\\},\\{y^{(1)},\\dots,y^{(t0)}\\} ) = \\sum_t L^{t} = -\\sum_t log P_{model}(y^{(t)} | \\{x^{(1)},\\dots,x^{(t0)}\\} )) 

Computing the gradient of this loss function with respect to the parameters is an expensive operation. So we need an alternative.

### Teacher Forcing and Networks with Output Recurrence
Teacher forcing is a procedure that emerges from the maximum likelihood criterion, in which during training the model receives the ground truth output <span>![](http://latex.codecogs.com/gif.latex?y^{(t)})</span> as input at time t + 1.
![](https://github.com/Little0o0/Little0o0.github.io/blob/master/img/RNN_teacher_forcing.png)

### Gradient
Computing the gradient through a recurrent neural network is straightforward.
![](http://latex.codecogs.com/gif.latex?\\frac{\\partial L}}{\\partial L^{(t)}} = 1)
In this derivation we assume that the outputs <span>![](http://latex.codecogs.com/gif.latex?o^{(t)})</span> are used as the argument to the softmax function.

![](https://github.com/Little0o0/Little0o0.github.io/blob/master/img/RNN_Gradient1.png)
![](https://github.com/Little0o0/Little0o0.github.io/blob/master/img/RNN_Gradient2.png)

We can then iterate backwards in time to back-propagate gradients through time, from t = t0 − 1 down to t = 1, noting that <span>![](http://latex.codecogs.com/gif.latex?h^{(t)})</span> (for t < t0) has as descendents both<span>![](http://latex.codecogs.com/gif.latex?h^{(t+1)})</span> and <span>![](http://latex.codecogs.com/gif.latex?o^{(t)})</span>. Its gradient is thus given by
![](https://github.com/Little0o0/Little0o0.github.io/blob/master/img/RNN_Gradient3.png)

Using this notation, the gradient on the remaining parameters is given by:
![](https://github.com/Little0o0/Little0o0.github.io/blob/master/img/RNN_Gradient4.png)

### Encoder-Decoder Sequence-to-Sequence Architectures
We often call the input to the RNN the “context.” We want to produce a representation of this context, C. The context C might be a vector or sequence of vectors that summarize the input sequence X.

1. an encoder or reader or input RNN processes the input sequence. The encoder emits the context C, usually as a simple function of its final hidden state.

2. a decoder or writer or output RNN is conditioned on that fixed-length vector (just like in figure 10.9) to generate the output sequence Y.

In a sequence-to-sequence architecture, the two RNNs are trained jointly to maximize
the average of P(Y|X).The last state ![](http://latex.codecogs.com/gif.latex?h^{(n_x)}) of the encoder RNN is typically used as a representation C of the input sequence that is provided as input to the decoder RNN.

![](https://github.com/Little0o0/Little0o0.github.io/blob/master/img/RNN_seq2seq1.png)

Here we discuss how an RNN can be trained to map an input sequence to an output sequence which is not necessarily of the same length.

## The Long Short-Term Memory(LSTM)