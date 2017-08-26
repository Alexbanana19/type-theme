---
layout: post
title: TD Series (4)
feature-img: "img/sample_feature_img.png"
comments: true
---

# Eligibility Traces II: TD(lamda) and Backward View

## Introduction
In this post we are going to introduce a mechanism to efficiently implement(or approximate) the forward view on-line. The original forward view we mentioned in [last post](https://alexbanana19.github.io/2017/07/29/eligibility-trace-1.html) suggests doing updates at the end of each episode, or **_off-line_** updates, which is generally very slow and impratical. Therefore, we use a **Backward View** instead to achieve **_on-line_** learning and it's very computationally efficient with **Eligiblity Trace**. The equivalence and difference between the foward view and backward view will also be discussed in this post.

## Eligiblity Trace
It may seem quite odd at first that eligitbility trace would have something to do with $$\lambda$$-return, but it is actually very intuitive once we finish the derivation. First, let's see what an eligiblity trace is(in tabular case). It has the following form:

$$e_{t} = \gamma \lambda e_{t-1} + I(S = S_{t})$$

$$e_{t}$$ is a vector with the same size as the state value vector $$V(S)$$(or state-action value vector $$Q(s,a)$$). It is also called _accumulating trace_. In the function approximation case it has the same numbers of components as the weight vector $$w_{t}$$. Here we plot a curve according to the formula to see how it varies in time:

<center>
<img src="{{ site.baseurl }}/img/2017-07-30-eligibility-trace-2/trace.png" width="1000" height="400" />
</center>

<center><small> Figure 1: Eligibility Trace. Pictures from David Silver's course: Introduction to Reinforcement Learning.[2]</small></center>
<br />



## Backward View

## TD(lambda)
