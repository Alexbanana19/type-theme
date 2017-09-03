---
layout: post
title:  "Using Fast Weights to Store Temporary Memories" by Geoffrey Hinton
feature-img: "img/sample_feature_img.png"
comments: true
---

##The basic Idea

- On each connection, the total weight is the sum of two components.

- The standard slow weight: This learns slowly and may also decay slowly. It holds the long-term knowledge of the system.

- The fast weight: This learns quickly and decays quickly. It holds temporary knowledge.

## Priming: Evidence for fast weights?

- If you hear the word cucumber, you will be better at hearing "cucumber" in a very noisy signal many minutes later.

- Do you really have to maintain a pattern of activity that represents cucumber for the intervening minutes?

  - If we had localist repersentation we could just temporarily lower the thresold of the cucumber unit.

  - If we use point attractors instead of localist unists we can temporarily increase the attractiveness of the cucumber attractor.

## why weight matrices are a better form of memory than activity vectors

- weight matrices have hugely more capacity than activity vectors

  - this is true even if we use dumb on-shot, outer-product learning, as in a Hpfield net

- A fast weight matrix of 1000x1000 can easily make about 100 attractors more attractive

## three ways to store temporary knowledge

- an LSTM stores temporary knowledge in its activity vectors
  - this means that temproray memmories that are currently irrelevant interfere with the ongoing processing

- an additional  external memeory can store things without interference but:
   - it needs to decide whne and where to read & write

- fast weights allow the temporary knoledge to be stored without having any extra neurons
  - they just make some attractors easier to fall into and they also "flavor" the attractor by slightly changing the activity vector that you end up with
