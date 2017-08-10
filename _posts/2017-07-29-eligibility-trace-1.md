---
layout: post
title: TD Series (3)
feature-img: "img/sample_feature_img.png"
---

# Eligibility Traces I: Lambda-Return and Forward View

## Introduction
In the last two posts, we have already grasped some basic understandings of TD Learning's mechanism and its advantages over Monte Carlo Learning and Dynamic Programming.
By sampling and bootstrapping, TD Learning can be implemented on-line and has nice convergence property (low variance). But still, updating just one state value at every time step can be very slow and sometimes the target can be very biased (like the overestimation problem in Q-Learning[1]) hence the correct state-value may not be learned.

<center>
<img src="{{ site.baseurl }}/img/2017-07-29-eligibility-trace-1/grid_world.png" width="1000" height="500" />
</center>

<center><small> Figure 1: Sarsa agent in windy grid world. Since the reward is 1 at the terminal and 0 everywhere else, only one state value is updated after the first episode. Pictures from David Silver's course: Introduction to Reinforcement Learning.[2]</small></center>
<br />

To address these two problems, we need to find a less biased target and a new learning algorithm that is more data-efficient.

## Lambda-Return
Let's consider forming a new target. Our original target in TD Learning is:

$$R_{t+1} + \gamma V(S_{t+1})$$

This target is biased because it's an estimate(bootstrap) of an estimate(sample). In fact, the more time steps we experience, the less biased our estimate is. This is called *n-step TD* and the limit form of it is Monte Carlo learning, which is unbiased because it uses actual return $$G_t$$ as a target.

<center>
<img src="{{ site.baseurl }}/img/2017-07-29-eligibility-trace-1/n-step-td.png" width="1000" height="600" />
</center>

<center><small> Figure 2: n-step TD and Monte Carlo Return. Pictures from David Silver's course: Introduction to Reinforcement Learning.[2]</small></center>
<br />

However, we don't want to give up the advantage of bootstrapping and using actual return can be of high variance. To bridge the gap between Monte Carlo Learning and TD Learning, we now introduce a new target, called $$\lambda$$-return. It has the following form:

<center>
<img src="{{ site.baseurl }}/img/2017-07-29-eligibility-trace-1/lambda-return.png" width="1000" height="700" />
</center>

<center><small> Figure 3: n-step TD and Monte Carlo Return. Pictures from Reinforcement Learning: An Introduction.[1]</small></center>
<br />


We use $$\lambda^{n-1}$$ to weight the n-step returns, and because $$\frac{1}{1-\lambda} = 1 + \lambda + \lambda^2 + ... + \lambda^n, 0 < \lambda < 1 $$, we can use $$(1-\lambda)$$ to normalize the sum:

$$ G_t^\lambda = (1-\lambda)\sum\limits_{n = 1}^\infty \lambda^{n-1} G_{t:t+n}$$

## Forward-View

By setting the $$\lambda$$ parameter appropriately, we can obtain a very nice bias-variance trade-off. This is called the forward-view and it provides us with a reasonable target and theory. We look forward in time to all the future rewards and decide how best to combine them.[1]

<center>
<img src="{{ site.baseurl }}/img/2017-07-29-eligibility-trace-1/forward-view.png" width="1000" height="370" />
</center>

<center><small> Figure 3: The Foward-View. Pictures from Reinforcement Learning: An Introdution.[7]</small></center>
<br />

However, implementing the forward-view directly is impractical because we will have to again wait until the end of the episode to do the update like MC Learning.

Therefore, an on-line method will be introduced in the next post involving two inportant components: The Backward-View and Eligibility Trace, which is also the one of the most essential concepts in Reinforcement Learning.

## References
[1] Sutton R S, Barto A G. Reinforcement learning: An introduction[M]. Cambridge: MIT press, 1998.

[2] Silver D, Online Course: Introduction to Reinforcement Learning, Lecture 4. ppt: page 22.
