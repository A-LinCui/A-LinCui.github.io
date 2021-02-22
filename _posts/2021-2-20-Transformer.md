---
layout:     post
title:      Transformer in Computer Vision
subtitle:   All in transformer!
date:       2021-2-20
author:     A-LinCui
header-img: img/post/night.png
catalog: true
mathjax: true
tags:
    - Deep Learning
    - Transformer
---

### Preface
**Transformer**, has become a mainstream model in the NLP field, since first put forward in [Attention Is All You Need (2017)](https://arxiv.org/abs/1706.03762). With the self-attention mechanism as the core, the transformer naturally achieve better global awareness than CNNs, which may contribute to computer vision tasks and robustness.

### Articles
#### [Tokens-to-Token ViT: Training Vision Transformers from Scratch on ImageNet](https://arxiv.org/abs/2101.11986)
##### Abstract
Modify tokenization scheme to model the local structure information and design a better backbone architecture.

##### Reasons for poor performance of ViT trained from scratch on Middle / Small datasets  
- The simple tokenization of input images fails to model the important local structure (e.g., edges, lines) among neighboring pixels, leading to its low training sample efficiency.
- The redundant attention backbone design of ViT leads to limited feature richness in fixed computation budgets and limited training samples.

##### Insights
- The vanilla ViT ignores the local structures when directly split images to tokens with a fixed length. 
- The backbone of vanilla ViT is not efficient as ResNets and presents limited feature richness when training samples are not enough.
- The architectures matters. NAS can be applied.

##### Architecture Perspective
- Deep-narrow structure benefit ViT.
- Dense connection hurts the performance of both ViT and T2T-ViT.
- SE block improves both ViT and T2T-ViT.
- ResNeXt structure has few effects on ViT and T2T-ViT.
- Ghost can further compress model and reduce MACs of T2T-ViT.

##### Methods
A T2T-ViT consists of two main components:
- A layer-wise “Tokens-to-Token module” (T2T module) to model the local structure information of images and reduce the length of tokens progressively; 
- An efficient “T2T-ViT backbone” to draw the global attention relations on the tokens from T2T module. We adopt a deepnarrow structure for the backbone to reduce redundancy and improve the feature richness after exploring several CNNbased architecture designs.

#### [Training data-efficient image transformers & distillation through attention](https://arxiv.org/pdf/2012.12877.pdf)

### Experiments
#### It seems that VIT isn't inherently robust. But it may result from the poor performance on clean data.

