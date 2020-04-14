# Understanding Deep Image Representations by Inverting Them

https://arxiv.org/pdf/1412.0035.pdf

CNN에서 얻은 feature를 가지고 이미지를 복원?

In this paper we conduct a direct analysis of the visual information contained in representations by asking the following question: ***given an encoding of an image, to which extent is it possible to reconstruct the image itself?***

- To answer this question we contribute a general framework to invert representations. We show that this method can invert representations such as HOG and SIFT more accurately than recent alternatives while being applicable to CNNs too.
- Among our findings, we show that several layers in CNNs retain photographically accurate information about the image, with different degrees of geometric and photometric invariance.

## Introduction

Despite the progress in the development of visual representations, their design is still driven empirically and a good understanding of their properties is lacking.

- In this paper we conduct a ***direct analysis of representations by characterising(특색있는) the image information that they retain***.

We do so by modeling a representation as a function $$\Phi(x)$$ of the ***image `x`*** and then computing an approximated  inverse $$\phi^{−1}$$, reconstructing `x` from  the  code $$ \Phi(x)$$.  

- A  common  hypothesis  is  that  representations  collapse irrelevant differences in images (e.g.  illumination or viewpoint), so that $$\Phi$$ should not be uniquely invertible.
- We pose this as a reconstruction problem and find a number of possible reconstructions rather than a single one.
  - By doing so, we obtain insights into the invariances captured by the representation.

### Contribution

- First, we propose a general method to invert representations, including SIFT, HOG, and CNNs.
  - Crucially, this method uses only information from the image representation and a generic natural  image prior, starting from random noise as initial solution, and hence captures only the information contained in the representation itself.
- Second, we show that, despite its simplicity and generality, this method recovers significantly better  reconstructions from DSIFT and HOG compared to recent alternatives.
- Third, ***we apply the inversion technique to the analysis of recent deep CNNs***, exploring their invariance by sampling possible approximate reconstructions. 
- Fourth, we study the locality of the information stored in the representations by reconstructing images from selected groups of neurons, either spatially or by channel.

### Related work

The works most related to ours are Weinzaepfel et al. and Vondricket al. which invert sparse DSIFT and HOG features respectively. While our goal is similar to theirs, ***our method is substantially different from a technical view-point, being based on the direct solution of a regularised regression problem***.

- The benefit is that our technique applies equally to shallow (SIFT, HOG) and deep (CNN) representations.
- Our work is also related to the DeConvNet method of Zeiler and Fergus, who backtrack the network  computations to identify which image patches are responsible for certain neural activations.
  - Demonstrated that ***DeConvNets can be interpreted as a sensitivity analysis of the network input/output relation***.
  - A consequence is that DeConvNets do not study the problem of representation inversion in the sense adopted here, which has significant methodological consequences;
  - For example, DeConvNets require auxiliary information about the activations in several intermediate layers, while our inversion uses only the final image code
  - In other words, ***DeConvNets look at how certain network outputs are obtained***
  - Whereas ***we look for what information is preserved by the network output***.

## Inverting representations

This is formulated as the problem of finding an image whose representation best matches the one.

- Formally, given a representation function $$\Phi :\mathbb{R}^{H×W×C} \rightarrow \mathbb{R}^d$$ 
- A representation $$Φ_0= Φ(x_0)$$ to be inverted
- Reconstruction finds the image $$x\in R^{H×W×C}$$ that minimizes the objective

### Objective

$$
x^* = \underset{x \in \mathbb{R}^{H x W x C}}{argmin} \, \mathcal{l}(\Phi(x), \Phi_0) + \lambda \mathcal{R}(x)
$$

- `x` is image
  - In short, find image which make loss function less
- loss function $$\mathcal{l}$$ compares the image representation $$\Phi(x)$$ to the target one $$\Phi_0$$
- $$\mathcal{R}$$ is a regularizer capturing a natural image prior

### Loss function

$$
\mathcal{l}(\Phi(x), \Phi_0) = ||\Phi(x) - \Phi_0||^2
$$

### Regularisers

Discriminatively-trained representations may discard a significant amount of low-level image statistics as these are usually not interesting for high-level tasks

As this information is nonetheless useful for visualization, it can be partially recovered by restricting the inversion to the subset of natural images $$\mathcal{X} \subset R^{H×W×C}$$

[Total variation denoising](https://en.wikipedia.org/wiki/Total_variation_denoising)

[Why Do Most of The Papers Use the Frobenius Norm for Denoising?](https://dsp.stackexchange.com/questions/45886/why-do-most-of-the-papers-use-the-frobenius-norm-for-denoising)

### Balancing the different terms

While representations are largely insensitive to the scaling of the image range, this is not exactly true for the first few layers of CNNs, where biases are tuned to a“natural” working range.

The final form of the objective function is
$$
|| \Phi(\sigma x) - \Phi_0||^2_2 / ||\Phi_0||^2_2 + \lambda_\alpha \mathcal{R}_\alpha(x) + \lambda_{V^\beta}\mathcal{R}_{V^{\beta}}(x)
$$
***It is in general non convex because of the nature of $$\Phi $$.  We next discuss how to optimize it.***

### Optimization

Deep representations are a chain of several non-linear layers.

- We extend ***gradient descent(GD) to incorporate momentum*** that proved useful in learning deep networks

## Representations

This section describes the image representations studied in the paper: DSIFT(Dense-SIFT), HOG, and reference deep CNNs.

- Furthermore, it shows how to implement DSIFT and HOG in a standard CNN framework in order to compute their derivatives.

## Experiments with shallow representations

Shallow : DSIFT, HOG

Regularizer effects to output a lot. 

- More impressive is DSIFT: it is quantitatively similar to HOG with bilinear orientations, but produces significantly more detailed images.
- DSIFT와 HOG는 숫자상으로는 비슷하게 나왔지만 DSIFT가 눈으로 봤을 때 더 많은 디테일을 포함하고 있었음
  - HOG에서 finer quantization을 썼는데 이게 결과적으로 DSIFT보다 많은 정보를 무시하게 됨

## Experiments with deep representations

Deep : CNN based

Somewhat surprisingly, the quantitative results show that CNNs are, in fact, not much harder to invert than HOG. The error rarely exceeds 20%, which is comparable to the accuracy for HOG.

All the convolutional layers maintain a photographically faithful representation of the image, although with increasing fuzziness.

- The 4,096-dimensional fully connected layers are perhaps more interesting, as they invert back to a ***composition of parts similar but not identical to the ones found in the original image***.
- Going from relu7 to fc8 reduces the dimensionality further to just 1,000; nevertheless some of these visual elements can still be identified.

Similar effects can be observed in the reconstructions in Fig. 7. This figure includes also the reconstruction of an abstract pattern, which is not included in any of the ImageNet classes;

- ***Still, all CNN codes capture distinctive visual features of the original pattern, clearly indicating that even very deep layers capture visual information***.
- CNN이 깊어지더라도 이미지의 특징은 잘 잡아냄

Note that all these and the original images are nearly indistinguishable from the viewpoint of the CNN model; it is therefore interesting to note the lack of detail in the deepest reconstructions, ***showing that the network captures just a sketch of the objects, which evidently suffices for classification***.

- 이미지가 구분할 수 없으면 classification에 필요한 스케치만 잡아냄

It is interesting to note that a lot of the inverted images have large green regions. We claim that this is a property of the network and not the natural image prior.

