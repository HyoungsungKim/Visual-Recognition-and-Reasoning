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