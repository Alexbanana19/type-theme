---
layout: post
title:  A Computational Principle that explains sex, brain and sparse coding by Geoffrey Hinton
feature-img: "img/sample_feature_img.png"
comments: true
---

## A problem with sexual repoduction

- Fitness depends on genes working well together. But sexual reproduction breaks up sets of co-adapted genes

- Actually this is a good thing even though it maybe bad in the short term.
  - It may help optimization in the long reproduction

  - It may make organisms more robust to changes in the environment

## A problem with neural activity

- cortical neurons do signal processing by sending discrete spikes of activity with apparently random timing

  - why don't they communicate precise analog values by using the precise times of spikes

  - surely analog values are more useful for signal processing?

## How spike timing could be used to compute scalar products

- we want to take the scalar product of a vector of activations (coded as spikes times)
with a vector of parameters.

- the answer must then be converted back to a spike time.
