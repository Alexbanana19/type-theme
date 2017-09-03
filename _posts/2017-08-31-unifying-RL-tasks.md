---
layout: post
title:  Unifying Task Specification in Reinforcement Learning
feature-img: "img/sample_feature_img.png"
comments: true
---

## Abstract
This work has two main  contributions:

- introduce a RL task formalism through transition-based discounting, including

  - problem formulation

  - unifying episodic and continuing specification

  - options

  - general value functions

- prove the contraction bounds on generalized Bellman operator

It also has theoratical proofs and specification about the properties like equivalence between transition-based discounting and state-based discounting as well as convergence of some algorithms, which will not be discussed in this blog.

## Introduction
It may seem quite hard for beginners to try to understand why the author wants to introduce a formalism for RL because the original one works very well in many tasks.

The fact is that algorithms and settings tend to vary from task to task in Reinforcement learning, but they are just special cases of generalized RL tasks, according to this paper. And to unify them the author propose a new formalism as well as prove the contraction bounds of generalized Bellman operator.

Another flaw of original RL task settings are that sometimes the learning objectives and dynamics of the enviorment seem to couple with each other. And to solve that the author proposes to use transition-based discounting, which can also break the gap between episodic and continous tasks.

To further understand this paper, readers must understand that as the tasks and algorthms become more generalized, the discouting factor $$ \gamma $$ and the bootstrap factor $$ \lambda $$ become more and more important in theoratical analysis. We can view $$\gamma$$ as soft-termination factor and $$\lambda$$ as a bias-variance trade-off factor, which will also be discussed later in the post.

## RL Formalism

### 1. Generalized Problem Settings

- __Dynamics__ of the environment, as the tuple (S, A, Pr):

  - S is the set of states

  - A is the set of actions

  - Pr : S x A x S -> [0, 1] is the transition probability function

- A __task__ is specified on top of these
transition dynamics, as the tuple (P, r, ?, i):

  - P is a set of policies $$\pi$$ : S x A->[0,1];
  - r : S x A x S -> R specifies reward received from ($$s$$,$$a$$,$$s_{0}$$);
  - $$\gamma$$ : S x A x S -> [0,1] is a transition-based discount function;
  - i : S -> [0, $$\infty$$) is an interest function that specifies the user defined interest in a state.

- __Goal__ is to maximize the cumulative discounted
reward obtained from following that policy:

  $$G_{t} = \sum\limits_{i = 0}^\infty (\prod\limits_{j = 0}^{i-1} \gamma (s_{t+j}, a_{t+j}, s_{t+1+j})) R_{t+1+i}$$

### 2. Unifying episodic and continuing specification

<center>
<img src="{{ site.baseurl }}/img/2017-08-31-unifying-RL-tasks/discount.png"/>
</center>
<center><small>Figure 1: Unifying episodic and continuing specification</small></center>

<br />

We can use trainsition-based discounting factor to transform episodic tasks to continuing tasks by  removing absorbing state and setting the discounting factor to 1 when teleporting the agent to the start-state.

### 3. Options
Options are just generalization of primitive actions.
However, Any fixed set of options defines a discrete-time SMDP embedded within the original MDP, as shown in Figure 2.

<center>
<img src="{{ site.baseurl }}/img/2017-08-31-unifying-RL-tasks/option.png"/>
</center>
<center><small>Figure 2: Options</small></center>

<br />

**Proposition 1.** An option, defined as the tuple ($$\pi$$, $$\beta$$, $$I$$) with policy $$\pi$$ : S x A -> [0,1], termination function $$\beta$$ : S -> [0,1] and an initiation set $$I \subset S$$ from which the option can be run, can be equivalently cast as an RL task.

To further explain it, we first choose an intial state from I, an choose and option to execute. We can terminate the option according to $$\beta(s)$$. The actions in the option are selected by $$\pi(a\|s)$$.

It seems very nice to have temporal abstraction using options, but it is also very redundant to include an another function $$\beta(s)$$ and the intiation set $$I$$. In this paper, the author points out that it can all be included in this formalism by setting:

  - $$\gamma(s, a, s') = 1 - \beta(s')$$

  - i(s) = 1 if $$s \in I$$ and i(s) = 0 otherwise, focuses learning resources on the states of interest.

### 4. General Value Functions(GVFs)
In a similar spirit of abstraction as options, general value functions were introduced for single predictive or goal-oriented questions about the world (Sutton et al., 2011). The idea is to encode predictive knowledge in the form of value function predictions: with a collection or horde of prediction demons, this constitutes knowledge (Sutton et al., 2011; Modayil et al., 2014; White, 2015).

The general value function has three _question_ functions:

- $$\pi$$ : S x A -> [0,1]; target policy to be learned.

- $$\gamma$$ : S -> [0, 1]; termination or discounting function.

- r : S x A x S -> $$\mathbb{R}$$; reward function

In many publications there is also specified a fourth question function, the terminal reward function z : S -> R used to specify a final reward at termination. More recently its has been recognized that this functionality can be included in the reward function.

and the four _answer_ functions:

- b : S x A -> [0, 1]; behavior policy

- I : S x A -> [0, $$\infty$$); interest function

- $$\phi$$ : S x A -> $$\mathbb{R}^n$$ ;feature-vector function

- $$\lambda$$ : S -> [0,1]; bootstrapping or eligibility-trace decay-rate function


It's hard to explain at first about what do question functions and answer functions mean and how to use them. Actually they are closely related to the [Horde architecture]() as well as [nexting](), which will not be discussed here. Off-policy learning is also involved in these settings, which generally includes Emphatic TD($$\lambda$$) and GQ($$\lambda$$).

### 5. Generalized Bellman Operator

In this section we are just going to introduce the defintion and derivation of the generalized Bellman operator. Readers can find other properties and proof of the approximation bounds in the paper.

First we have the tradition definition of a Bellman operator:

<center>
<img src="{{ site.baseurl }}/img/2017-08-31-unifying-RL-tasks/bellman.png"/>
</center>
<center><small>Figure 3: Bellman Operator</small></center>

<br />

The trace parameter $$\lambda$$ : S x A x S -> [0, 1] influences the fixed point and provides a modified (biased) return, called the $$\lambda$$-return; this parameter is typically motivated as a bias-variance trade-off parameter (Kearns and Singh, 2000).

and then we have the defintion of the generalized Bellman operator:

<center>
<img src="{{ site.baseurl }}/img/2017-08-31-unifying-RL-tasks/gbellman.png"/>
</center>
<center><small>Figure 4: Generalized Bellman Operator</small></center>

<br />

To see why this is the definition of the Bellman operator, we define the expected $$\lambda$$-return, $$v_{\pi, \lambda} \in \mathbb{R}^n$$ for a given approximate value function, given by a vector $$v \in \mathbb{R}^n$$.

<center>
<img src="{{ site.baseurl }}/img/2017-08-31-unifying-RL-tasks/derivation1.png"/>
</center>

<center>
<img src="{{ site.baseurl }}/img/2017-08-31-unifying-RL-tasks/derivation2.png"/>
</center>
<center><small>Figure 5: Derivation of Generalized Bellman Operator</small></center>

<br />

By using this generalized bellman operator together with other RL formalism in this paper, we can now establish a new mordern Reinforcement Learning Architecture. 
