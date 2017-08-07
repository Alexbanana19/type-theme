---
layout: post
title:  TD Series (1)
feature-img: "img/sample_feature_img.png"
---
# Understading TD Learning I: Intuition

## Introduction
**TD (temporal difference) Learning** is by far the most fascinating and successful Reinforcement Learning algorithm. It bridges the gap between the traditional trial-error methods and some advanced ideas like **Emphasis** [2] and **Eligibility Traces** [1]. However, the mechanism behind the scene have often been neglected since people tend to focus on the specific implementations of this idea, like **Sarsa** and **Q-Learning** [1]. This blog post tends to explain the TD idea more intuitively.

## What's TD Learning?
To understand TD learning more intuitively, here's a classic example to illustrate how TD Learning works in our daily life.

<img src="{{ site.baseurl }}/img/2017-07-27-TD-learning/header.png" width="4000" height="500" />

> <center> <small>Figure 1: driving home example by Monte Carlo methods
(left) and TD methods (right).[1]</small></center>

- Setting: We are driving home from work.
- Goal: Estimate how long it will take to get home.
- Action: Drive.
- Transition: All things can happen to us during driving.
- Reward: Elapsed time.
- Return: Actual time to go.
- Value of states: Expected time to go.
- The x-axis represents the time step and events, the y-axis is the predicted total time to go home.
- On the left is MC method and on the right is TD method.

As we can see, when we update the value of each state, MC method will have to wait till the end to do the update; it's like our original prediction to go home is 30 minutes, then we caught in a traffic jam on road at 6:00 p.m., but we don't change our estimate untill we get home at 8:00 p.m.. Not until then can we realize that getting caught in a traffic jam is not good.

<img src="{{ site.baseurl }}/img/2017-07-27-TD-learning/traffic.jpg" width="2000" height="700" />

It may sound foolish but that's what MC method does. On the contrary, TD method is more "human-like": When we get caught in a traffic jam at 6:00 p.m., we will immediately realize that our original estimate (30 mins to go home) is too optimistic, because 6:00 p.m. is the evening rush hour and we will probably run into traffic jam. So we can update our state value right away rather than wait until we get home.

## Why TD Learning?
Now we know why it's named Temporal-Difference Learning: It uses the difference between time steps to update its estimate rather than use the actual return, which can only be acquire in the end.

The example above explains the main idea of TD Learning: Sample and Bootstrap. Like Monte Carlo
methods, TD methods can learn directly from raw experience; Also like Dynamic Programming, TD methods update estimates based in part on other learned estimates, without waiting for a final outcome.

In short, TD Learning is a combination of Monte Carlo Learning and Dynamic Programming.

<img src="{{ site.baseurl }}/img/2017-07-27-TD-learning/TD_structure.png" width="4000" height="500" />

> <center> <small> Figure 2: TD Learning illustration: Sample and Bootstrap [3] </small> </center>

## How to implement TD Learning?
Mathematical analysis and specific control algorithms, like Sarsa, Q-learing and expected Sarsa will be discussed in next post.

## References
[1] Sutton R S, Barto A G. Reinforcement learning: An introduction[M]. Cambridge: MIT press, 1998.

[2] Mahmood A R, Yu H, White M, et al. Emphatic temporal-difference learning[J]. arXiv preprint arXiv:1507.01569, 2015.

[3] Silver D, Online Course: Introduction to Reinforcement Learning, Lecture 4. ppt: page 30.
