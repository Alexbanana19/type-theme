---
layout: post
title:  TD Series (2)
feature-img: "img/sample_feature_img.png"
---
# Understading TD Learning II: Math

## Introduction
In the last post we established some intuitions about TD Learning. Now in this post we will have to go through some mathematic formulas and specific algorithms to gain more insights about this method.

## Equations for TD Learning
We consider the standard reinforcement learning setting where an agent interacts with an environment $$E$$ over a number of discrete time steps. At each time step $$t$$, the agent receives a state $$S_{t}$$ and selects an action $$a_{t}$$ from some set of possible actions $$A$$  according to its policy $$\pi$$, where $$\pi$$  is a mapping from states $$S_{t}$$ to actions $$a_{t}$$ .

In return, the agent receives the next state $$S_{t+1}$$ and receives a scalar reward $$R_{t}$$. The process continues until the agent reaches a terminal after which the process restarts. The return $$G_{t} = \sum\limits_{k = 0}^\infty \gamma^k R_{t+k}$$ is the total accumulated return from time step $$t$$ with discount factor $$\gamma \in (0, 1]$$. The goal of the agent is to estimate the expected return from each state $$S_{t}$$:

$$ V_{\pi}(S_{t})$$

$$ = E_{\pi}[G_t|S_{t} = s] $$

$$ = E_{\pi}[R_t + \gamma V_\pi (S_{t+1})|S_{t} = s] $$

Now we can do sampling from the expectation to update our state value. But because samples are just estimates of the expectation, so we want some kind of balance between the current value and the new value:

$$ V(S_{t}) :=  (1-\alpha)V(S_{t}) + \alpha [R_{t+1} + \gamma V(S_{t+1})]$$

which can be also written in an incremental update form:

$$ V(S_{t}) :=  V(S_{t}) + \alpha [R_{t+1} + \gamma V(S_{t+1}) - V(S_{t})]$$

where $$\alpha$$ is the learning rate parameter and $$\gamma$$ is the discounting factor. (There are also some [interesting discussion]() about $$\gamma$$.) Because we are bootstrapping, we use $$R_{t+1} + \gamma V(S_{t+1})$$ as target rather than the actual return $$G_{t}$$ to update $$V(S_{t})$$; Because we are sampling from an expectation, so we need to use a relatively small or decreasing $$\alpha$$ (Empirically, $$\frac{1}{n}$$ works fine).

Note that $$R_{t+1} + \gamma V(S_{t+1})$$ is also known as _TD-error_, w.r.t $$\delta_t$$ [1], which is very essential and appears in many works in Reinforcement Learning. It has the following equivalence [1] with return $$G_t$$.

$$G_t - V_t$$

$$= R_{t+1} + \gamma G_{t+1} - V(S_t) + \gamma V(S_{t+1}) - \gamma V(S_{t+1})$$

$$ = \delta_t + \gamma (G_{t+1} - V(S_{t+1}))$$

$$ = \delta_t + \gamma \delta_{t+1} + \gamma^2 (G_{t+2} - V(S_{t+2})) $$

$$ = ... $$

$$ = \sum\limits_{k = t}^{T-1} \gamma^{k-t} \delta_{k} $$ (This sum is called _Monte Carlo error_)

This identity is not exact if $$V$$ is updated during the episode (as it is in TD(0)), but
if the step size is small then it may still hold approximately. Generalizations of this
identity play an important role in the theory and algorithms of temporal-difference
learning [1].

## Algorithms for TD Learning
The following two algorithms are methods that using TD Learing to do control, which involves another two concepts: General Policy Iteration and On/Off Policy Learning. Details will not be discussed here, but basically they are just iteratively refining their policies and state value to reach fixed point.

<center>
<img src="{{ site.baseurl }}/img/2017-07-27-TD-learning-2/GPI.png" width="300" height="300" />
</center  >

<center><small>Figure 1: General Policy Iteration [1]</small></center>

### Sarsa
<center>
<img src="{{ site.baseurl }}/img/2017-07-27-TD-learning-2/sarsa.png" width="1170" height="500" />
</center>

<center> <small>Figure 2: Sarsa [1]</small></center>

### Q-learning
<img src="{{ site.baseurl }}/img/2017-07-27-TD-learning-2/q-learning.png" width="1500" height="500" />

<center><small>Figure 3: Q-Learning [1]</small></center>

##Experiments
Here's a cliff walking example from the book _Reinforcement Learning: An Introcduction_ to compare the two learning algorithm.

<center>
<img src="{{ site.baseurl }}/img/2017-07-27-TD-learning-2/cliff-walking.png" width="1500" height="500" />
</center>

<center><small>Figure 4: cliff walking [1]</small></center>

You can also check out my [repo](https://github.com/Alexbanana19/Reinforcement-Learning-An-Introduction/blob/master/cliff_walking.ipynb) to see more implementation details.

<center class = "half">
<img src="{{ site.baseurl }}/img/2017-07-27-TD-learning-2/exp1.png" width="500" height="400" />
<img src="{{ site.baseurl }}/img/2017-07-27-TD-learning-2/exp2.png" width="400" height="400" />
</center>

<center><small>Figure 5: implementation and empirical results of cliff walking</small></center>


## References
[1] Sutton R S, Barto A G. Reinforcement learning: An introduction[M]. Cambridge: MIT press, 1998.
