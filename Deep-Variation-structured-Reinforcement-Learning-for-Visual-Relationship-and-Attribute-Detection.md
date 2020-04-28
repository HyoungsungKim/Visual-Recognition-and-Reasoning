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

## Deep Variation-structured ReinforcementLearning 

We propose a novel VRL framework which formulates the problem of detecting visual relationships and attributes as a sequential decision-making process.

### Directed Semantic Action Graph

We build a directed semantic graph G= (V,E) to organize all possible object nouns, attributes, and relationships into a compact and semantically meaningful representation.

- The nodes V consist of the set of all candidate object
  - Categories C
    -  Object categories  in C are nouns, and may be people, places, or parts of objects. 
  - Attributes A
    - Attributes in A can describe color, shape, or pose. 
  - Predicates P
    - Relationships are directional, i.e. they relate a subject noun and an object noun via a predicate.
    - Predicates in P can be spatial (e.g., “inside of”), compositional (e.g.“part of”) or action (e.g., “swinging”).
- The directed edges E consist of attribute phrases $E_A\subseteq C \times A$ and predicate phrases $E_P\subseteq C \times P \times C$.
  - An attribute phrase $(c,a)\in E_A$ represents an attribute $a\in A$ belonging to a noun $c \in C$.
    - For example, the attribute phrase “young girl” can be represented by $\text{(“girl”, “young”)}\in E_A$.
  - A predicate phrase $(c,p,c′)\in E_P$ represents a subject noun $c\in C$ and an object noun $c′\in C$ related by a predicate $p \in P$.
    - For example, the predicate phrase “a man is swinging a bat” can be represented by $\text{(“man”, “swinging”, “bat”)} \in E_P$.

### Variation-structured RL

We propose a novel variation-structured traversal scheme over the semantic action graph that dynamically constructs small action sets for each step.

- First, VRL uses an object detector to get a ***set S of candidate object instances***, and then sequentially assigns relationships and attributes to each instance $s \in S$.
  - For our experiments, we used state-of-the-art Faster R-CNN as the object detector, where the network parameters were initialized using the pre-trained VGG-16 ImageNet model.
  - Since subject instances in an image often have multiple relationships and attributes, ***we do a breadth-first search***:
    - We predict all relationships and attributes with respect to the current subject instance of interest, and then move onto the next instance.
    - We start from the subject instance with the most confident classification score. To prevent the agent from being trapped in a single search path (e.g., in a small local region), ***the agent selects a new starting subject instance if it has traversed through 5 neighboring objects in the breadth-first search***.
- The same object in multiple scenarios may be described by different, semantically ambiguous noun categories that cannot be distinguished by the object detector.  
  - To address this semantic ambiguity, ***we introduce an ambiguity-aware object mining scheme*** which leverages scene contexts captured by extracted relationships and attributes to help determine the most appropriate object category.

#### Variation-structured action space

The directed semantic graph G serves as the action space for VRL.

- For any object instance $s \in S $ in an image(S : Candidate object instance), denote its object category by $s_c\in C$ and its bounding box by $B(s) = (s_x,s_y,s_w,s_h)$ where $(s_x,s_y)$ is the center coordinate, $s_w$ is the width, and $s_h$ is the height.
- Given the current subject instances and object instances′, we select three actions $g_a \in A$, $g_p\in P$, $g_c\in C$ according to the VRL network as follows

1. Select an attribute $g_a$ describing s from the set $\Delta_a=\{a: (s_c,a)\in E_A \backslash H_A(s)\}$, 
   - Where $H_A(s)$ denotes the set of previously mined attribute phrases for s.
2. Select a predicate $g_p$ relating the subject noun $s_c$ and object noun $s′_c$ from $\Delta_p=\{p: (s_c,p,s′_c)\in E_P \}$ .
3. To select the next object instance $\tilde{s} \in S$ in the image, we select its corresponding object category $g_c$ from a set $\Delta_c \subseteq C$, which is constructed using an ambiguity-aware object mining scheme as follows

#### State space

Given the current subjects and objects′ instances in each time step, the state vector f is a concatenation of

1. The feature vectors of s and s′ ;
2. The feature vector of the whole image;
3. A history phrase embedding vector, which is created by concatenating the semantic embeddings of the last two relationship phrases (relating s and s′) and the last two attribute phrases(describing s) that were mined via the variation-structured traversal scheme.
   - More specifically, each phrase (e.g., “person  riding  bicycle”) is embedded into a 2400-dim vector using a pre-trained Skip-thought language model, thus resulting in a 9600-dim history phrase embedding.

#### Rewards

Suppose we have groundtruth labels, which consist of the set $\hat{S}$ of object instances in the image, and attribute phrase $\hat{E}^A$ and predicate phrase $\hat{E}^P$ describing the objects in $\hat{S}$.

- Given a predicted object instance $s \in S$, we say that a groundtruth object $\hat{s} \in \hat{S}$ overlaps with s if ***they have the same object category (i.e., $s_c = \hat{s}_c \in C$)*** And ***their bounding boxes have at least 0.5 Intersection-over-Union (IoU) overlap***.

We define the following reward functions ***to reflect the detection accuracy of taking action $(g_a,g_p,g_c)$ in state `f`***, where the current subject and object instances are s and s′, respectively:

1. $R_a(f,g_a)$ returns +1 if there exists a groundtruth object $\hat{s} \in \hat{S}$ that overlaps with s, and the predicted attribute relationship $(s_c,g_a)$ is in the groundtruth set $\hat{E}^A$. Otherwise, it returns -1.
   - $R_a(g_a | f)$ 가 더 맞는 표현 아닌가...?
2. $R_p(f,g_p)$ returns  +1 if there exists $\hat{s}, \hat{s}^′ \in \hat{S} $ that overlap with s and s′ respectively, and $(s_c,g_p,s′_c)\in \hat{E}^P$. Otherwise, it returns -1.
3. $R_c(f,g_c)$ returns +5 if the next object instance $\hat{s}\in S$ corresponding to category $g_c \in C$ overlaps with a new groundtruth object $\hat{s} \in S$. Otherwise, it returns -1. Thus, it encourages faster exploration over all objects in the image.

### Deep Variation-structured RL



