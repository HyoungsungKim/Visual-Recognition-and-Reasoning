# Relational Reasoning

## Compositional Learning for Human Object Interaction

In this paper, we explore the problem of zero-shot learning of human-object interactions.

- It is not clear if the learned representations of actions are generalizable to new categories

***we want to learn a model than can work even on unseen combinations***.

- To deal with this problem, In this paper, we propose a novel method using external knowledge graph and graph convolutional networks which learns how to compose classifiers for verb-noun pairs. 

### Introduction

We propose to explore using external knowledge to bridge the gap of contextuality, and to help the modeling of compositionality for human object interactions.

- Specifically, we extract Subject, Verb and Object (SVO) triplets from knowledge bases to build an external knowledge graph.
- These triplets capture a large range of human object interactions, and encode our knowledge about actions
- Each verb (motion) or noun (object) is a node in the graph with its word embedding as the node’s feature

***We focus on the compositional learning by leveraging external knowledge***.

### Method

$$
p(y|x) = \phi(x,y^v,y^n;\mathcal{K})
$$

- $\mathcal{K}$ is the prior knowledge about actions.
- Our key idea is to represent $\mathcal{K}$ via a graph structure and use this graph for learning to compose representations of novel actions.

The core component of our model is a graph convolutional network $g(y^v,y^n;\mathcal{K})$.

- $g$ learns to compose action representation $z_a$ based on embeddings of verbs and nouns, as well as the knowledge of SVO triplets and lexical information.
- The output $z_a$ is further compared to the visual feature $x$ for zero shot recognition.

### A Graphical Representation of Knowledge

$$
\mathcal{G} = \mathcal{(V, E, Z)}
$$

- $\mathcal{G}$ : Graph
- $\mathcal{V}$  : Vertex
- $\mathcal{E}$  : Edge between vertex and feature vector
- $\mathcal{Z}$ : Feature vector

We propose to use this graph structure to encode two important types of knowledge:

1. The “affordance” of objects, such as “book can be hold” or “pen can be taken”, defined by SVO triplets from external knowledge base
2. The semantic similarity between verb or noun tokens, defined by the lexical information from WordNet 

#### Graph Construction

> ***How powerful are Graph Convolutional Network?***