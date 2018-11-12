---
layout: post
title: "RBMs for Collaborative Filtering (ICML'2007)"
date: 2017-04-06
excerpt: "A summary of using RBMS for Collaborative Filtering."
tags: [CF, ICML]
mathjax: true
comments: false
---

If we are going to write a paper on deep learning for Collaborative Filtering problem, then RBM-CF [1] must be cited! Although it was not the best algorithm that yielded the lowest RMSE during the Netflix contest (SVD++ was the best algorithm at that time), RBM-CF was used as one of many algorithms for ensemble learning. One the main reason is that of its unique approach at that time. Thus, its predictions were slightly different from matrix factorization approaches.

It turns out that one of the state-of-the-art for collaborative filtering right now (as of  April 2017) that I am aware of is NADE-CF [2] which is based on a similar idea as RBM-CF. This post will summarize the RBM-CF model, its architecture, and extensions.

The RBM-CF models the joint probability between visible and hidden units. In this case of CF problem, visible units are observed ratings which are represented as a binary value. Each rating is one-hot encoded as a binary vector of length K where K is the maximum rating. The goal is to infer hidden units from these observed ratings. This means we need to learn a non-linear function that maps visible units to a probability of distribution of hidden units. In RBF, this non-linear function is a sigmoid function.

RBM-CF uses a softmax function to model the visible units and the hidden units are modeled by Bernoulli distribution. To infer all hidden units, this model is trained by using a contrastive divergence to approximate the gradient of the log-likelihood. In order to make a prediction, the author suggested to first compute all \\( p(v_q = k | V) \\) and normalize them using softmax. Then, compute the expectation of rating.

One possible variant is to model the hidden units as Gaussian latent variables. This variant increases the capacity of the model. Another variant to utilize the missing ratings as an extra information. The author observed that all rating in the test sets can be treated as all items that are viewed by a user without the rating. The viewing information is represented as a binary random variable and influences the hidden units. The conditional RBM model significantly improves performance. Imputing the missing values is a heuristic used to slightly improve the model performance.

The most interesting contribution is how the author proposed an architecture to reduce the number of free parameters. This can be done by factoring the weight matrix into a product of two lower-rank matrices. The less number of parameters means that we can avoid an overfitting and the convergence will be faster.

CF-RBM has a slightly lower RMSE than a standard SVD. The modern approaches such as Autoencoder or PMF are much more scalable than CF-RBM. This model can be extended to a deeper model and RBM used for parameter pretraining.

References:

[1] Salakhutdinov, Ruslan, Andriy Mnih, and Geoffrey Hinton. "Restricted Boltzmann machines for collaborative filtering." Proceedings of the 24th international conference on Machine learning. ACM, 2007.

[2] Zheng, Yin, et al. "A neural autoregressive approach to collaborative filtering." Proceedings of the 33nd International Conference on Machine Learning. 2016.
