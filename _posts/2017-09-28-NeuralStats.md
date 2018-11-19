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
- Draw a dataset prior \\( \boldsymbol{c} \sim N(\boldsymbol{0}, \boldsymbol{I}) \\)
- For each data point in the dataset
  - Draw a latent vector \\( \boldsymbol{z} \sim P(\cdot \mid \boldsymbol{c}) \\)
  - Draw a sample \\( \boldsymbol{x} \sim P(\cdot \mid \boldsymbol{z}) \\)
  
The likelihood of the dataset is:

$$ p(D) = \int p(c) \big\[ \prod_{x \in D} \int p(x \mid z;\theta)p(z \mid c;\theta)dz \big\]dc $$

The paper define the approximate inference network, \\( q(z \mid x,c;\phi) \\) and \\( q(c \mid D; \phi) \\) to optimize a variational lowerbound. The single dataset log likelihood lowerboud is:

$$ \mathcal{L}_D = E_{q(c \mid D;\phi)}\big\[\sum_{x \in d} E_{q(z \mid c, x; \phi)}\[ \log p(x \mid z;\theta)\] - D_{KL}(q(z \mid c,x;\phi)||p(z \mid c;\theta)) \big\] - D_{KL}(q(c \mid D;\phi)||p(c)) $$

The statistic network \\( q(c \mid D; \phi) \\) that approximates the posterior distribution over the context c given the dataset D. Basically, this inference network has an encoder to take each datapoint into a vector \\( e_i = E(x_i) \\). Then, add a pool layer to aggregate \\( e_i \\) into a single vector, an element-wise mean is used. The final vector is used to generate parameters of a diagonal Gaussian.

This model surprisingly works well for many tasks such as topic models, transfer learning, one-shot learning, etc.

## Reference
- [Towards a Neural Statistician](https://arxiv.org/abs/1606.02185)
