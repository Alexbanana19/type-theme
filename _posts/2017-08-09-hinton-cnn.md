---
layout: post
title:  What is wrong with convolutional neural networks by Geoffrey Hinton
feature-img: "img/sample_feature_img.png"
comments: true
---

## I Standard neural nets

- Problem: two few levels of **structure**, no explict **entities** (?)

- Solution: "**Capsules**" that group the neurons in each layer may entail the emergence of "entities" which is inspired by a "mini-column"(?)

## II Capsules

### What is a capsule

- each capsule represents the presence (whether it exists in the sample) and the instantiation parameters of a multi-dimensional entity of the type that the capsule detects (properties or features, e.g. size, color).


- A capsule detects a particular type of object or object-part.

- A capsule outputs two things:
  1. the probability that an object of that type is present

  2. The **generalized pose** of the object which includes position, orientation, scale, velocity, etc.

### What do capsules do: conincidence filtering

- difference from normal neural nets: take predictions from the low level capsules, find the tightest cluster regardless of how other outliers behave(unsupervised), which is exactly Rancic and Hough transform in computer vision.(?)

- A capsule has high dimensional pose space, so coincidences (in this case are the clusters) do not happen by chance, which presents some kind of entity that exists.(say for two six-dimentional vectors which agree on every dimension within 10% odds, then the total chance is 1/10^6)

## III Object Recognition

### Convnets

- What's good:

  - multiple layers of feature detectors

  - feature detectors are local, replicated across space

  - the spatial domains of the feature detectors get bigger in higher layers. (why it is good?)

- What's bad:

  -  interleaving with pooling layers.

### Motivations for Pooling

1. Pooling gives a small amount of translational invariance at each level.
  - The precise location of the most active feature dector is thrown away

  - it's ok if:
    - pools overlap a lot

    - feature detectors code relative positions of other features

- reduces number of inputs to the next layer of feature extraction.

  - this allows us have more types of feature at the next layer(with bigger domains)

### What kind of a percept does a convnet have?

- The activations in the last hidden layer of a deep convnet are a percept

  - the percept contains information about many of the objects in the image

  - how about the relationship among objects?

- By mapping the percept to the initial hidden state of a deep recurrent neural net and train the RNN to ouput caption, we can see the convnet can do impressive stuffs.

(_*This above suggests that Hinton believes in convolution but not pooling_)

### Four arguments against pooling

- **Argument 1**: It is a bad fit to the psycology of shape perception:

  - It does not explain why we assign intrinsic **coordinate frames** to objects and why they have such huge effects.(give examples)

  - Maybe neural net is so powerful that it can actually fake intrinsic coordinate frames (This is not quite true because we haven't proved it yet)

  - **mental rotation**

- conclusion:

  - people use retangular coordinate frames embedded in objects

  - The relationship between an object and the viewer is represented by a whole bunch of active neurons that capture different aspects of the relationship, not by a single neuron or a coarse coded set of neurons

- **Argument 2**: It solves the wrong Problem, we want:
  -  Equivariance in view-point, invariance in knoledge.(activities don't have to be invariant with view-point)

    - (what is equivariance, give examples)
    - two types of equivariance

  - Disentangling rather than discarding.

  - Convnets try to make he neural activities invariant to samll changes to small changes in viewpoint by combining the activities within a pool.

    - This is the wrong goal.

    - It is motivated by the fact that the final label needs to be viewpoint-invariant

  - It's better to aim for equivariance: changes in viewpoint lead to corresponding changes in neural activities.

      - In the perceptual system, it's the weights that code viewpoint-invariant knoledege, not the neural activities(crucial)



- **Argument 3**: It fails to use the underlying linear structure:
  - It does not make use of the natural linear manifold that perfectly handles the largest source of variance in images.(fails to use the knowledge of computer graphics)

  - current nn: train on different viewpoint to get invariance

  - better approach:

    - the manifold of images of the same rigid shape is highly non-linear in the space of pixel intensities.

    - transform to a space in which the manifold is globally linear (i.e. the graphic representation that uses explicit pose coordinates)

    - this allows massive extrapolation(and uses whole lot less data)

  - Generalize over viewpoints by using the globally linear manifold used by computer graphics

    - graphics program use hierarchical models in which spatial structure is modeled by matrices that represent the transformation from a coordinate frame embedded in the whole to a coordinate frame embedded in each part.

    - these matrices are totally viewpoint invaraint

    - this representation makes it easy to compute the relationship between a whole and the camera from the relationship between a part and the camera. (give examples: face, just Hough transform, but with higher dimensions)

- **Argument 4**: Pooling is a poor way to do dynamic routing(dimension hopping):

  - we need to route each part of the input to the neurons that know how to deal with it. Finding the best routing is equivalent to parsing the image.

  - we don't want to replicate the knowledge across all locatins with a small stride.

    - it's better to route the information to a single capsule that can deal with it.

    - but the information could appear anywhere in the image

  - routing:

    - convnet: in each pool, the ouput neuron only attends to the most active neuron in the pool.

    - better approach: send the information to the capsule in the layer above that is best at dealing  with it.(parse tree)
