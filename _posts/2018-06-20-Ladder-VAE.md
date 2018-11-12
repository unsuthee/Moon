---
layout: post
title: "Ladder VAE (NIPS'2016)"
date: 2018-06-20
excerpt: "A summery of Ladder VAE."
tags: [VAE]
comments: false
---

Ladder VAE is a VAE architecture that can effectively several stochastic layers. The trick is to couple the distribution parameters from the inference and generative networks to better estimate the model's parameters.

## The generative network is:

$P(\textbf{z}) = P(\textbf{z}_L)\prod_{i=1}^{L-1}P(\textbf{z}_i|\textbf{z}_{i+1})$

Where:

$P(\textbf{z}_i|\textbf{z}_{i+1}) = Normal(\textbf{z}_i|\mu_{p,i}(\textbf{z}_{i+1}), \sigma^2_{p,i}(z_{i+1}))$

and

$P(\textbf{z}_L) = Normal(\textbf{z}_L|0, I)$

The prior $P(\textbf{z}_L)$ is a standard gaussian.

## The inference network:

The key difference is that each stochastic layer calculates the distribution parameters (mean and variance) but does not sample the latent vector from this distribution. The standard VAE will simply draw a sample from the distribution parameters computed by the inference network. However, the Ladder VAE draw the latent vector from:

$\sigma_{q,i} = \frac{1}{\hat \sigma^2_{q,i} + \sigma^2_{p,i}}$

$\mu_{q,i} = \frac{\hat \mu_{q,i} \hat \sigma^{-2}_{q,i} + \mu_{p,i}\sigma^{-2}_{p,i}}{\hat \sigma^{-2}_{q,i} + \sigma^{-2}_{p,i}}$

The approximate distribution at layer i is:

$q(\textbf{z}_i|\cdot) = Normal(\textbf{z}_i|\mu_{q,i},\sigma^2_{q,i})$

By coupling the distribution parameters from both inference and generative networks, the Ladder VAE utilizes information from both network for parameter learning.

## Reference
* Sønderby, Casper Kaae, et al. "Ladder variational autoencoders." Advances in neural information processing systems. 2016.
