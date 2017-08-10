---
layout: post
title: TD Series (4)
feature-img: "img/sample_feature_img.png"
---

# Eligibility Traces II: TD(lamda) and Backward View

## Introduction
In this post we are going to introduce a mechanism to efficiently implement(or approximate) the forward view on-line. The original forward view we mentioned in [last post]() suggests doing updates at the end of each episode, or **_off-line_** updates, which is generally very slow and impratical. Therefore, we use a **Backward View** instead to achieve **_on-line_** learning and it's very computationally efficient with **Eligiblity Trace**. The equivalence and difference between the foward view and backward view will also be discussed in this post.

## Eligiblity Trace
It may seem quite odd at first that eligitbility trace would have something to do with $$\lambda$$-return, but it is actually very intuitive once we finish the derivation. First, let's see what an eligiblity trace is. It has the following form:


## Backward View

## TD(lambda)
