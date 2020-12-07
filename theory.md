---
layout: default
title: Theory
mathjax: true
---




### The general idea

A pixel is typically specified by three numbers (R, G, B).  Colour as perceived by humans is inherently three dimensional as there are three types of cone cells in the human retina.
Similarly a colour printer will usually have four pigments (it's the ratio of the pigments that matter, so four are needed to express three dimensions of colour).
However an artist does not start with these four pigments, rather they would start with a palette of colours suited to the image they have in mind to paint.
Typically they might then mix two or three of those palette colours at a time to achieve their desired colour on canvas.

Our algorithm performs a similar task.
Given an image it 

1. finds a suitable palette of colours, and 
2. splits any colour into a mixture of these palette colours.

You might think that this is impossible: if I have 8 palette colours say, then from a three dimensional RGB value, the algorithm gives an 8-dimensional vector specifying the mixture, how can (useful) information be gained this way?
The answer is that we make an assumption about the distributions of colours in the image, this comes in two parts

1. (sparsity) each colour in the image is formed from a mixture of two or three colours, and
2. (non-uniform sparsity) some combinations of two or three colours are more likely than others.

With this assumption of non-uniform sparsity it seems more reasonable to expect that we could split three coordinates into say, 8, most of the resulting coordinates would be zero.


#### A statistical model

The above idea is made precise using a statistical model, whose parameters are 

1. a small set of palette colours and 
2. a discrete probability distribution on the set of triples of these colours.

These define a probability distribution on the set of colours.
To sample from this distribution first choose one of the triples at random, $$C = (c_i, c_j, c_k)$$ according to the discrete distribution.
Then choose at random a mixture of these three colours $$u_i c_i + u_j c_j + u_k c_k$$, where the triple $$(u_i, u_j, u_k)$$ is picked uniformly from the set of all possible mixture parameters, a triangle.

Such a model is called a _simplicial mixture model_.


#### The hypothesis

The efficacy of our approach relies on the following hypothesis:

> For a natural image, the distribution of pixel colour values is well-approximated by some simplicial mixture model.

If this hypothesis is true, and if we can find such a model then we have a great tool to manipulate the colours.

For images created by an artist from a palette of colours this is a reasonable assumption.
Although photographs of natural scenes weren't painted they are still created from a mixture of colours, for example the colours in an image of a flower are created by mixing colour from the light source, from the pigment of the petals, the green from the chlorophyll and black for shadows.

An alternative way to consider image formation is to look at the rendering equations for physically based rendering; these govern how to mix various colours derived from the properties of a material, or more generally a whole scene.  Again this is reasonable approximated by a simplicial mixture model.


#### Fitting a model

This is the tricky part, but it builds on standard methods from statistics.
First we suppose that our set of pixels $$\{x_1,\ldots,x_N\}$$ was drawn a distribution $$\text{SMM} + K$$, where $$\text{SMM}$$ is a simplicial mixture model and $$K$$ is a multivariate normal distribution with zero mean and some covariance matrix.
There is a measure of how good an assumption this is, it's called the _likelihood_, i.e. the likelihood that a dataset, our pixels, was drawn from such a distribution.  The hypothesis above could be formulated as saying that there is some set of parameters

* palette colours
* distribution of simplices
* covariance of $$K$$

which has a high likelihood.

To fit a model we use the [Expectation-Maximisation (EM) algorithm](https://en.wikipedia.org/wiki/Expectation–maximization_algorithm).
The general gist is that we start with any choice of initial parameters, then each step of the EM algorithm outputs a new set of parameters which is guaranteed to have a higher likelihood than the previous step.
If we repeat this enough times then we hope to get a good set of parameters and a well-fitted model.

There are two steps to this algorithm, the expectation step, and the maximisation step.  The role of the expectation step is to compute some coefficients based on an old set of parameters which are then used in the maximisation step to find a new set of parameters.
The good news is that the maximisation step is quick and simple to calculate for a simplicial mixture model.

The bad news is that the coefficients needed by the maximisation step are not so easy to calculate and there is no exact formula for the expectation step.  So instead we must estimate the coefficients using numerical integration, for this we use a [Markov Chain Monte Carlo](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) technique.  In practice this entails running a Markov Chain for every pixel of our training image, the state of this Markov chain is a choice of a simplex $$(u \leq v \leq w)$$ and a choice of coefficients $$(x,y,z) \mid x,y,z > 0, x+y+z=0$$ in that triangle.
For each step one of the vertices $$u, v, w$$ is removed at random and replaced with a new vertex, while the coefficients are also changed.

The likelihood of this new state is compared to the likelihood of the old state, if it is more likely then the new state is accepted, if it is less likely then the new state is accepted according to a random variable.
This is the [Metropolis-Hastings algorithm](https://en.wikipedia.org/wiki/Metropolis%E2%80%93Hastings_algorithm).


