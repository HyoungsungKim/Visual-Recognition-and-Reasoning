# Deep Variation-structured Reinforcement Learning for Visual Relationship and Attribute Detection

https://arxiv.org/pdf/1703.03054.pdf

## Abstract

To capture such global interdependency, we propose a deep Variation-structured Reinforcement Learning (VRL) framework to sequentially discover object relationships and attributes in the whole image

## Introduction

Detecting relationships and attributes ismore challenging than traditional object detection due to the following reasons:

1. There are a massive number of possible relationship and attribute types (e.g., 13,894 relationship types in Visual Genome), resulting in a greater skew of rare and infrequent types.
2. Each object can be associated with many relationships and attributes, making it inefficient to exhaustively search all  possible relationships for each pair of objects.
3. A global, holistic perspective of the image is essential to resolve semantic ambiguities (e.g.,“woman wearing wetsuit” vs. “woman wearing shirt”). Existing approaches only predict a limited set of relationship types and ignore semantic interdependencies between relationships and attributes by evaluating each region within a scene separately.

### Variation-structured  Reinforcement  Learning(VRL)  framework 

- First, we use language priors to build a directed semantic action graph $\mathcal{G}$,  where the nodes are nouns, attributes, and predicates, connected by directed edges that represent semantic correlations. 
  - This graph provides a highly-informative, compact representation that enables the model to learn rare relationships and attributes from frequent ones using shared graph nodes.
  - For example, the semantic meaning of “riding” learned from “person-riding-bicycle” can help predict the rare phrase “child-riding-elephant”. This generalizing ability allows VRL to handle a considerable number of possible relationship types. 
- Second, existing deep reinforcement learning (RL) models often require several costly episodes of trial and error to converge, even with a small action space, and our large action space would exacerbate this problem.
  - To efficiently discover all relationships and attributes in a small number of steps, we introduce a ***novel variation-structured traversal scheme*** over the action graph which constructs small, adaptive action sets $\Delta_a,\Delta_p,\Delta_c$ for each step based on the current state and historical actions:
    - $\Delta_a$ contains candidate attributes to describe an object;
    - $\Delta_p$ contains candidate predicates for relating a pair of objects;
    - $\Delta_c$ contains new object instances to mine in the next step.
  - Since an object instance may belong to multiple object categories which the object detector cannot distinguish, we introduce an ambiguity-aware object mining scheme to assign each object with the most appropriate category given the global scene context.
  - Our variation-structured traversal scheme offers a very promising technique for extending the applications of deep RL to complex real-world tasks.
- Third, to ***incorporate global context cues for better reasoning***, we explicitly encode the semantic embeddings of previously extracted phrases in the state vector.
  - It makes a better trade-off between increasing the input dimension and utilizing more historical context, compared to appending history frames or binary action vectors as in previous RL methods.

