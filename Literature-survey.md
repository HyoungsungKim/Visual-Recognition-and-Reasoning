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

During meta-learning, it learns to learn a deep distance metric to compare a small number of images  within episodes, each of which is designed to simulate the few-shot setting. Once trained, a RN is able to classify images of new classes by computing relation scores between query images and the few examples of each new class without further updating the network.

Data augmentation and regularization techniques can alleviate overfitting in such a limited-data regime, but they do not solve it. 

Therefore contemporary approaches to few-shot learning often decompose training into an auxiliary meta learning phase where transferable knowledge is learned in the form of good initial conditions, embeddings or optimization strategies.

> Meta-learning : Learning to learn

Specifically, we propose a two-branch Relation Network(RN) that performs few-shot recognition by learning to compare query images against few-shot labeled sample images.

First an embedding module generates representations of the query and training images. ***Then these embeddings are compared by a relation module that determines*** if they are from matching categories or not.

### Related work

Many of these approaches use a meta-learning or learning-to-learn strategy in the sense that they ***extract some transferable knowledge from a set of auxiliary tasks (meta-learning, learning-to-learn)***, which then helps them to learn the target few-shot problem well ***without suffering from the overfitting*** that might be expected when applying deep models to sparse data problems

Metric-learning based approaches aim to learn a set of projection functions such that when represented in this embedding, images are easy to recognize using simple nearest neighbor or linear classifiers.

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

### Why does Relation Network Work?

In contrast to prior work’s fixed metric or fixed features and shallow learned metric, Relation Network can be seen as both learning a deep embedding and learning a deep non-linear metric (similarity function)

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

### Methods

Idea

1.  How to use directional derivatives to quantify the sensitivity of ML model predictions for different user-defined concepts
2. How to compute a final quantitative explanation ($TCAV_Q$ measure) of the relative importance of each concept to each model prediction class, without any model retraining or modification.

#### User-defined Concepts as Sets of Examples

***The first step in our method is to define a concept of interest***. 

- We do this simply by choosing a set of examples that represent this concept or find an independent data set with the concept labeled.
- The key benefit of this strategy is that it does not restrict model interpretations to explanations using only pre-existing features, labels, or training data of the model under inspection.
- Instead, there is great flexibility for even non-expert ML model analysts to define concepts using examples and explore and refine concepts as they test hypotheses during analysis.
- Section 4 describes results from experiments ***with small number of images (30) collected using a search engine***. For the case of fairness analysis (e.g.,  gender,  protected groups), curated examples are readily available.

### Concept Activation Vectors(CAVs)

We then define a “concept activation vector” (or CAV) as the normal(법선) to a hyperplane separating examples without a concept and examples with a concept in the model’s activations 

### Statistical significance testing

***One pitfall with the TCAV technique is the potential for learning a meaningless CAV***. After all, using a randomly chosen set of images will still produce a CAV. A test based on such a random concept is unlikely to be meaningful.

To guard against spurious(가짜의) results from testing a class against a particular CAV, we propose the following simple statistical significance test.

- Instead of training a CAV once, against a single batch of random examples N, we perform multiple training runs, typically 500.
- A meaningful concept should lead to TCAV scores that behave consistently across training runs.

Concretely we perform a two-sided $t$-test of the TCAV scores based on these multiple samples.

- If we can reject the null hypothesis of a TCAV score of 0.5, we can consider the resulting concept as related to the class prediction in a significant way.
- Note that we also perform a Bonferroni correction for our hypotheses (at $p < α/m$ with m= 2) to control the false discovery rate further. All results shown in this paper are CAVs that passed this testing

### TCAV extensions: Relative TCAV

Relative CAVs allow making such fine-grained comparisons. Here the analyst selects two sets of inputs that represent two different concepts, C and D. 

## Prototypical network for Few-shot Learning

Matching networks, which uses an attention mechanism over a learned embedding of the labeled set of examples (the support set) to predict classes for the unlabeled points (the query set).

- Matching networks can be interpreted as a weighted nearest-neighbor classifier applied within an embedding space.
- Notably, this model utilizes sampled mini-batches called episodes during training, where each episode is designed to mimic the few-shot task by sub-sampling classes as well as data points.
- The use of episodes makes the training problem more faithful to the test environment and thereby improves generalization

Ravi and Larochelle take the episodic training idea further and propose a meta-learning approach to few-shot learning.

- Their approach involves training an LSTM to produce the updates to a classifier, given an episode, such that it will generalize well to a test-set. Here, rather than training a single model over multiple episodes, the LSTM meta-learner learns to train a custom model for each episode.

We attack the problem of few-shot learning by addressing the key issue of overfitting. Our approach, prototypical networks, is based on the idea that there exists an embedding in which points cluster around a single prototype representation for each class

In order to do this, we learn a non-linear mapping of the input into an embedding space using a neural network and take a class’s prototype to be the mean of its support set in the embedding space. 

- Classification is then performed for an embedded query point by simply finding the nearest class prototype.
- We follow the same approach to tackle zero-shot learning; here each class comes with meta-data giving a high-level description of the class rather than a small number of labeled examples.
- We therefore learn an embedding of the meta-data into a shared space to serve as the prototype for each class.
- Classification is performed, as in the few-shot scenario, by finding the nearest class prototype for an embedded query point

### Prototype Networks

## Matching Networks for One Shot Learning

Our framework learns a network that maps a small labeled support set and unlabeled example to its label, obviating the need for fine-tunning to adapt to new class types.

### Introduction

Data augmentation and regularization techniques alleviateoverfitting in low data regimes, but do not solve it.

Many non-parametric models allow novel examples to be rapidly assimilated, whilst not suffering from catastrophic forgetting. Some models in this family (e.g., nearest neighbors) do not require any training but performance depends on the chosen metric

The novelty of our work is twofold:

- At the modeling level, and at the training procedure. We propose Matching Nets (MN), a neural network which uses recent advances in attention and memory that enable rapid learning.
- Secondly, our training procedure is based on a simple machine learning principle: test and train conditions must match.
- Thus to train our network to do rapid learning, we train it by showing only a few examples per class, switching the task from minibatch to minibatch, much like how it will be tested when presented with a few examples of a new task.