# Visualizing and UnderstandingConvolutional Networks

https://cs.nyu.edu/~fergus/papers/zeilerECCV2014.pdf

In this paper we explore both issues.

- We ***introduce a novel visualization technique*** that gives insight into the function of intermediate feature layers and the operation of the classifier.
- We also perform an ablation study ***to discover the performance contribution from different model layers***.

Convnet : Convolution network

Deconvnet : Deconvolution network

## 1. Introduction

Several factors are responsible for this dramatic improvement in performance:

1. The availability of much larger training sets, with millions of labeled examples;
2. Powerful GPU implementations, making the training of very large models practical
3. Better model regularization strategies, such as Dropout 

But these are not enough to understand how CNN works

In this paper we introduce a visualization technique that reveals the input stimuli that excite individual feature maps at any layer in the model.

- It also allows us to observe the evolution of features during training and to ***diagnose potential problems with the model***.
- The visualization technique we propose uses a multi-layered Deconvolutional Network(deconvnet) to project the feature activations back to the input pixel space.
- We also perform a sensitivity analysis of the classifier output by occluding portions of the input image, revealing which parts of the scene are important for classification.

## 2. Approach

### 2.1 Visualization with a Deconvnet

Understanding the operation of a convnet requires interpreting the feature activity in intermediate layers.

- We present a novel way to ***map these activities back to the input pixel space***, showing what input pattern originally caused a given activation in the feature maps.
  - We perform this mapping with a Deconvolutional Network(deconvnet).
  - A deconvnet can be thought of as a convnet model that uses the same components (filtering, pooling) but in reverse, so instead of mapping pixels to features does the opposite.

Convnet : Convolution filtering with filter F

Deconvet(Transposed convolution) : Convolution filtering with filter transpose of F

[Convolution and Deconvolution animation](https://github.com/vdumoulin/conv_arithmetic)

#### Unpooling

In the convnet, the max pooling operation is non-invertible, how-ever we can obtain an ***approximate inverse by recording the locations of the maxima within each pooling region in a set of `switch` variables***.

## 4. Convnet Visualization

### 4.2 Occlusion Sensitivity

With image classification approaches, a natural question is if the model is truly identifying the location of the object in the image, or just using the surrounding context.

- The examples clearly show the model is localizing the objects within the scene, as the probability of the correct class drops significantly when the object is occluded.

## 5. Experiments

### Varying ImageNet Model Sizes:

In each case, the model is trained from scratch with the revised architecture. 

- Removing the fully connected layers (6,7) only gives a slight increase in error.
- Removing two of the middle convolutional layers also makes a relatively small difference to the error rate.
- However, removing both the middle convolution layers and the fully connected layers yields a model with only 4 layers whose performance is dramatically worse.
  - This would suggest that the overall depth of the model is important for obtaining good performance
- Increasing the size of the middle convolution layers goes give a useful gain in performance. But increasing these,while also enlarging the fully connected layers ***results in over-fitting***.

### Feature Analysis

The feature hierarchies become deeper, they learn increasingly powerful features.

## 6. Discussion

First, we presented a novel way to visualize the activity within the model.

- This reveals the features to be far from random, uninterpretable patterns.

Rather, they show many intuitively desirable properties such as compositionality, increasing invariance and class discrimination as we ascend the layers.

- We also show how these visualization can be used to identify problems with the model and so obtain better results.

We then demonstrated through a series of occlusion experiments that the model, while trained for classification, is ***highly sensitive to local structure in the image and is not just using broad scene context***.