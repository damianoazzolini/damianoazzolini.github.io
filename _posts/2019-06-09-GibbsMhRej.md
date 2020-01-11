---
layout: post
title: Gibbs sampling vs Metropolis Hastings vs Rejection sampling
date: 2019-06-09
categories: statistics
---
Often in probabilistic models, direct sampling is not feasible due to the complexity of the probability distribution. When the model is too complex, exact methods, which try to compute the values in an exact vay, are not usable. In this cases we can use *approximate inference*. Some of the most known methods are Gibbs sampling, Metropolis Hasting sampling and Rejection sampling. Here we briefly analyse these algorithms for probabilistic logic programs.


# Markov Chain Monte Carlo
Gibbs sampling and Metropolis Hastings sampling are two of the most known *Markov Chain Monte Carlo* [(MCMC)](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) methods.

The main idea of MCMC methods is to try to approximate the real probability distribution by creating a *Markov chain*. In a nutshell, a [Markov chain](https://en.wikipedia.org/wiki/Markov_chain) is a probabilistic model where the probability of each event depends only from the previous state. This is the so called *Markov property*. A graphical representation of a markov chain is (credits Wikipedia):

![Markov chain image (credits: Wikipedia)](/images/mc.svg)

The previous image can be interpreted as: there are two states, `E` and `A`. With probability 0.3 (i.e. 30%) the models stays in state `E` and with probability 0.7 the model goes to state `A`. Similarly, with probability 0.4 the model goes from `A` to `E` and with probability 0.6 the model stays in `A`. Notice that the sum of the probability on the outgoing arcs is 1.

Markov chain can be use both in *discrete*, as this example and *continuous* time. Here we consider mainly discrete time.

To use some formulas, the Markovian property can be expressed as 

$$P(X_{n+1} = x | X_1 = x_1,\dots,X_n = n_x) = P(X_{n+1} = x | X_n = x_n)$$ 

where $$X_1,\dots,X_n$$ are [random variables](https://en.wikipedia.org/wiki/Random_variable) (a random variable can be considered as a variable whose values depends on random phenomena) with the Markov property. If you are not familiar with this formula, take a look at, for instance, [conditional probability](https://en.wikipedia.org/wiki/Conditional_probability).

Going back to MCMC methods, MCMC algorithms generate samples from the [*posterior distribution*](https://en.wikipedia.org/wiki/Posterior_probability) (probability after the evidence) when directly sampling from the posterior is not feasible.
As the number of samples increases, the approximation gets closer and closer to the true posterior. Often, the first samples (for instance 100) are discarded because they does not represent the real distribution (a phase called *burnin*). The Markov chain from which the values are sampled should have some good property such as [ergodicity](https://en.wikipedia.org/wiki/Markov_chain#Ergodicity) but the explanation of this property will not be analyzed here (see the link for more information). 

## Gibbs Sampling
The main idea behind Gibbs sampling is to sample each variable independently considering the other variables observed. The process goes as follows: suppose we have `n` random variables $$X_1,\dots,X_n$$. We first set these variables to an initial value $$x_1^{(0)},\dots,x_n^{(0)}$$. This can be done for instance by sampling from a [*prior distribution*](https://en.wikipedia.org/wiki/Prior_probability) (the probability distribution without taking into account the evidence). Then we set a certain number o iterations and, for each iteration (or until convergency, i.e. when the computed values are stable), we take a sample

$$x_m^{(t)} \sim P(x_m \mid x_1^{t-1},x_2^{t-1},\dots,x_{m-1}^{t-1},x_{m+1}^{t-1},\dots,x_n^{t-1})$$

That is the main idea of the algorithm. There is also a variant called *blocked* Gibbs sampling where two or more variables are grouped together and the samples are taken from their joint distribution. 

## Metropolis Hastings
The basic idea behind Metropolis Hastings sampling is the following: we sample a random choice where the evidence is true to build an initial sample. Then we query the evidence. If the evidence succeeds then the query is asked by sampling and accepted according to an *acceptance ratio*. We continue this process for a certain number of samples or until convergency. The final probability is given by the number if acceptances over the number of samples.

## Rejection Sampling
Rejection sampling is maybe the first and simplest sampling algorithm. Despite the previous two, Rejection sampling does not build a Markov chain but it simply query the evidence, and, if the sample is successful, query the goal in the same sample. This process is repeated for a certain number of samples and the probability is computed as $$Successes/Samples$$. One of the main weakness is the fact that, if the evidence is very unlikely to happen, a lot of samples are discarded and the algorithm will be very slow.