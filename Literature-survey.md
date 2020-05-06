# Literature survey

## Attentive Relational Networks for Mapping Images to Scene Graphs

Despite the recent success in object detection using deep learning techniques, inferring complex contextual relationships and structured graph representations from visual data remains a challenging topic.

We propose a novel Attentive Relational Network that consists of two key modules with an object detection backbone to approach this problem.

- The first module is a semantic transformation module utilized to capture semantic embedded relation features, by translating visual features and linguistic features into a common semantic space.
- The other module is a graph self-attention module introduced to embed a joint graph representation through assigning various importance weights to neighboring nodes.
- Finally, accurate scene graphs are produced by the relation inference module to recognize all entities and the corresponding relations.

We evaluate our proposed method on the widely-adopted Visual Genome Dataset, and the results demonstrate the effectiveness and superiority of our model.

Effectively extracting a whole joint graph representation to model the entire scene graph for reasoning is promising but remains an arduous problem.

## Learning to Compare: Relation Network for Few-Shot Learning

Data augmentation and regularization techniques can alleviate overfitting in such a limited-data regime, but they do not solve it. 

Therefore contemporary approaches to few-shot learning often decompose training into an auxiliary meta learning phase where transferable knowledge is learned in the form of good initial conditions [10], embeddings or optimization strategies.

Specifically, we propose a two-branch Relation Network(RN) that performs few-shot recognition by learning to compare query images against few-shot labeled sample images.

First an embedding module generates representations of the query and training images. ***Then these embeddings are compared by a relation module that determines*** if they are from matching categories or not.

### Problem Definition

We have three datasets: a training set, a support set,and a testing set.

- The support set and testing set share the same label space, but the training set has its own label space that is disjoint with support/testing set.
- If the support set contains K labeled examples for each of C unique classes, the target few-shot problem is called C-way K-shot

With the support set only, we can in principle train a classifier to assign a class label $\hat{y}$ to each sample $\hat{x}$ in the testset.

- However, due to the lack of labeled samples in the support set, the performance of such a classifier is usually not satisfactory.
- Therefore we aim to ***perform meta-learning on the training set***, in order to extract transferable knowledge that will allow us to perform better few-shot learning on the support set and thus classify the test set more successfully.

An effective way to exploit the training set is to mimic the few-shot learning setting via episode based training, as proposed in [39].

### Model

Our Relation Network (RN) consists of two modules: an embedding module $f_\varphi$ and a relation module $g_\phi$, 

## Interpretability Beyond Feature Attribution: Quantitative Testing with Concept Activation Vectors (TCAV)

The key idea is to view the high-dimensional internal state of a neural net as an aid, not an obstacle. 

### Introduction

***A CAV for a concept is simply a vector in the direction of the values (e.g., activations) of that concept’s set of examples***.

- CAV란 컨셉의 예시 집합의 값의 방향에 있는 벡터
- In this paper, ***we derive CAVs by training a linear classifier*** between a concept’s examples and random counter examples and then taking the vector orthogonal to the decision boundary. This simple approach is supported by recent work using local linearity.
- TCAV uses directional derivatives to quantify the model prediction’s sensitivity to an underlying high-level concept, learned by a CAV.
- We conduct statistical tests where CAVs are randomly re-learned and rejected unless they show a significant and stable correlation with a model output class or state value

### Related work

Meaningful directions can be efficiently learned via simple  linear  classifiers. 

Our work extends this idea and computes directional derivatives along these learned directions in order to ***gather the importance of each direction for a model’s prediction***.

- Using TCAV’s framework, we can conduct hypothesis testing on any concept on the fly (customization) that make sense to the user (accessibility) for a trained network (plug-in readiness) and produce a global explanation for each class
- 