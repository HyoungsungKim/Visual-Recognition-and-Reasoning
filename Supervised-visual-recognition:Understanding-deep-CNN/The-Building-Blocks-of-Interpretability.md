# The Building Blocks of Interpretability

https://distill.pub/2018/building-blocks/

***In this article, we treat existing interpretability methods as fundamental and composable building blocks for rich user interfaces***. We find that these disparate techniques now come together in a unified grammar, fulfilling complementary roles in the resulting interfaces. Moreover, this grammar allows us to systematically explore the space of interpretability interfaces, enabling us to evaluate whether they meet particular goals.

***We will present interfaces that show what the network detects and explain how it develops its understanding, while keeping the amount of information human-scale***. For example, we will see how a network looking at a labrador retriever detects floppy ears and how that influences its classification.    

## Making Sense of Hidden Layer

We cannot work with spatial activations. Because it is too abstract. However we can transform this abstract vector into a more meaningful ***"semantic dictionary"***".

To make a semantic dictionary, ***we pair every neuron activation with a visualization of that neuron*** and sort them by the magnitude of the activation. 

- This marriage of activations and feature visualization changes our relationship with the underlying mathematical object.
- Activations now map to iconic representations, instead of abstract indices, with many appearing to be similar to salient human ideas, such  as “floppy ear,” “dog snout,” or “fur.”  

Semantic dictionaries are powerful not just because they move away from meaningless indices, but because ***they express a neural network’s learned abstractions with canonical examples***.

- We can know what neural learn
- They used Olah, et al., "Feature Visualization", Distill, 2017. https://distill.pub/2017/feature-visualization/

By bringing meaning to hidden layers, `semantic dictionaries` set the stage for our existing interpretability techniques to be composable building blocks. As we shall see, just like their underlying vectors, ***we can apply dimensionality reduction to them***.

- In other cases, semantic dictionaries allow us to push these techniques further.
  - For example, besides the one-way attribution that we currently perform with the input and output layers, semantic dictionaries allow us to attribute to-and-from specific hidden layers.
    - 레이어의 결과를 알 수 있음
  - In principle, this work could have been done without semantic dictionaries but it would have been unclear what the results meant.
    - 레이어의 결과를 아는건 semantic dictionaries 없이 알 수도 있지만, 결과의 의미를 알 수 없음

## What dose the Network See?

***what does each single neuron detect?*** Building off this representation, we can also consider an activation vector as a whole.

- Instead of visualizing individual neurons, ***we can instead visualize the combination of neurons that fire at a given spatial location***. (Concretely, we optimize the image to maximize the dot product of its activations with the original activation vector.)
- Applying this technique to all the activation vectors allows us to not only see what the network detects at each position, but also what the  network understands of the input image as a whole.

***By scaling the area of each cell by the magnitude of the activation vector, we can indicate how strongly the network detected features at that position:***

## How Are Concepts Assembled?

Feature visualization helps us answer *what* the network detects

- But it does not answer *how* the network assembles these individual pieces to arrive at later decisions
- Or *why* these decisions were made.

We use a fairly simple method, linearly approximating the relationship, but could easily substitute in essentially any other technique. Future improvements to attribution will, of course, correspondingly improve the interfaces built on top of them.  

### Spatial Attribution with Saliency Maps

The most common interface for attribution is called a ***saliency map*** — a simple heatmap that highlights pixels of the input image that most caused the output classification. We see two weaknesses with this current approach.  

- First, it is not clear that individual pixels should be the primary unit of attribution. The meaning of each pixel is extremely entangled with other pixels, is ***not robust to simple visual transforms*** (e.g., brightness, contrast, etc.), and is far-removed from high-level concepts like the output class.
- Second, traditional saliency maps are a very limited type of interface — ***they only display the attribution for a single class at a time***, and do not allow you to probe into individual points more deeply. 

As they do not explicitly deal with hidden layers, it has been difficult to fully explore their design space.  

We instead treat attribution as another user interface building  block, and apply it to the hidden layers of a neural network.

- In doing so, we change the questions we can pose.
  - Rather than asking whether the color of a particular pixel was important for the “labrador retriever” classification, we instead ask ***whether the high-level idea detected at that position (such as “floppy ear”) was important***.
    - Saliency map : Focus on pixel
    - Writer's way : focus on position
  - This approach is similar to what Class Activation Mapping (CAM) methods do but, because they interpret their results back onto the input image, they miss the opportunity to communicate in terms of the rich behavior of a network’s hidden layers.  

### Channel Attribution

Saliency maps implicitly slice our cube of activations by applying attribution to the spatial positions of a hidden layer. This aggregates over all channels and, as a result, ***we cannot tell which specific detectors at each position most contributed to the final output classification***.

***An alternate way to slice the cube is by channels instead of spatial locations.*** Doing so allows us to perform *channel attribution*: how much did each detector contribute to the final output?

채널을 합칠때 좋지만 방법에 단점이 있음

Attribution to spatial locations and channels can reveal powerful things about a model, especially when we combine them together. Unfortunately, this family of approaches is burdened by two  significant problems.

- On the one hand, it is very easy to end up with an overwhelming  amount of information: it would take hours of human auditing to understand the long-tail of channels that slightly impact the output.
- On the other hand, both the aggregations we have explored are extremely lossy and can miss important parts of the story. And, while we could avoid lossy aggregation by working with individual neurons, and not aggregating at all, this explodes the first problem combinatorially.  

## Making Things Human-Scale

***If one only uses spatial activations or channels, they miss out on very important parts of the story***. For example it’s interesting that the floppy ear detector helped us classify an image as a Labrador retriever, but it’s much more interesting when that’s combined with the locations that fired to do so. One can try to drill down to the level of neurons to tell the whole story, but the tens of thousands of neurons are simply too much information. Even the hundreds of channels, before being split into individual neurons, can be overwhelming to show users!  

- The key to doing so is finding more meaningful ways of breaking up our activations.
- Often, many channels or spatial positions will work together in a highly correlated way and are most useful to think of as one unit.
  - Other channels or positions will have very little activity, and can be ignored for a high-level overview.
  - So, it seems like we ought to be able to find better decompositions  if we had the right tools.  
  - 관련성이 있는 채널은 함께 동작함. 따라서 관련되지 않은 채널은 무시 할 수 있음
- There is an entire field of research, called matrix factorization, that studies optimal strategies for breaking up matrices.
  - By flattening our cube into a matrix of spatial locations and channels,  we can apply these techniques to get more meaningful groups of neurons.

Unfortunately, any grouping is inherently a tradeoff between reducing things to human scale and preserving information. 

The goals of our user interface should influence what we optimize our  matrix factorization to prioritize. 

- For example, if we want to prioritize what the network detected, we would want the factorization to fully describe the activations.
- If we instead wanted to prioritize what would change the network’s behavior, we would want the factorization to fully describe the gradient.

- Matrix factorization은 우리가 우선권을 어디에 두느냐에 따라 결정 됨
  - 네트워크가 무엇을 찾는지 알고 싶으면 묘사에 사용된 activation을 factorization 함
  - 네트워크의 움직임 변화를 알고 싶으면, gradient를 묘사하기 위해 factorization 함
- If we want to prioritize what caused the present behavior, we would want the factorization to fully describe the attributions. 
  - 현재 행동을 유발한 것을 알고 싶으면 attribution을 factorization

***we’ve constructed groups that prioritize the activations***.

## The Space of Interpretability Interfaces

Composing these pieces is not an arbitrary process, but rather follows a structure based on the goals of the interface. For example, should the interface emphasize *what* the network recognizes, prioritize *how* its understanding develops, or focus on making things *human-scale*. To evaluate such goals, and understand the tradeoffs, we need to be able to *systematically* consider possible alternatives.  

- Each element displays a specific type of *content* (e.g., activations or attribution) using a particular style of *presentation* (e.g., feature visualization or traditional information visualization).     

Attribution : 뉴런들의 관계를 설명하는 방법

Activation : 특정 뉴련의 결과 벡터(?)

We can think of dataset examples as another substrate in our design space, thus becoming another building block that fully composes with the others. ***In doing so, we can now imagine interfaces that not only allow us to inspect the influence of dataset examples on the final output classification (as Koh & Liang proposed), but also how examples influence the features of hidden layers, and how they influence the relationship between these features and the output.*** For example, if we consider our “Labrador retriever” image, we can not only see which dataset examples most influenced the model to arrive at this classification, but also which dataset examples most caused the “floppy ear” detectors to fire, and which dataset examples most caused  these detectors to increase the “Labrador retriever” classification.

