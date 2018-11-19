---
layout: post
title: "Towards a Neural Statistician (ICLR'17)"
date: 2017-09-28
excerpt: "A summery of Hierarchical VAE model."
tags: [VAE]
mathjax: true
comments: false
---

One extension of VAE is to add a hierarchy structure. In contrast to the classical VAE which its prior is drawn from a standard Gaussian distribution, Hierarchical VAE (HVAE) learns the prior distribution from the dataset. 

The generative process is:
- Draw a dataset prior \\( \mathbf{c} \sim N(\mathbf{0}, \mathbf{I}) \\)
- For each data point in the dataset
  - Draw a latent vector \\( \mathbf{z} \sim P(\cdot | \mathbf{c}) \\)
  - Draw a sample \\( \mathbf{x} \sim P(\cdot | \mathbf{z}) \\)
  
The likelihood of the dataset is:

$$ p(D) = \int p(c) \big[ \prod_{x \in D} \int p(x|z;\theta)p(z|c;\theta)dz \big]dc $$

The paper define the approximate inference network, \\( q(z|x,c;\phi) \\) and \\( q(c|D; \phi) \\) to optimize a variational lowerbound. The single dataset log likelihood lowerboud is:

$$ \mathcal{L}_D = E_{q(c|D;\phi)}\big[  \sum_{x \in d} E_{q(z|c, x; \phi)}[ \log p(x|z;\theta)] - D_{KL}(q(z|c,x;\phi)||p(z|c;\theta)) \big] - D_{KL}(q(c|D;\phi)||p(c)) $$

The statistic network \\( q(c|D; \phi) \\) that approximates the posterior distribution over the context c given the dataset D. Basically, this inference network has an encoder to take each datapoint into a vector \\( e_i = E(x_i) \\). Then, add a pool layer to aggregate \\( e_i \\) into a single vector, an element-wise mean is used. The final vector is used to generate parameters of a diagonal Gaussian.

This model surprisingly works well for many tasks such as topic models, transfer learning, one-shot learning, etc.

## Reference
- [Towards a Neural Statistician](https://arxiv.org/abs/1606.02185)
