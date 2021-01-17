---
layout:     post
title:      Ensemble Neural Architecture Search
subtitle:   For improvement and robustness.
date:       2021-1-17
author:     A-LinCui
header-img: img/post/night.png
catalog: true
mathjax: true
tags:
    - NAS
---

#### Paper list

| Class | Year | Paper |
|:----:|:----:|:----:|
| Ensemble Pattern | 2016 | [Simple and Scalable Predictive Uncertainty Estimation using Deep Ensembles](https://arxiv.org/abs/1612.01474) |
| Ensemble Pattern | 2015 | [Why M Heads are Better than One: Training a Diverse Ensemble of Deep Networks](https://arxiv.org/abs/1511.06314)|
| Ensemble NAS | 2020 | [Neural Ensemble Search for Performant and Calibrated Predictions](https://arxiv.org/abs/2006.08573) |


#### Ensemble Patterns
- **Random Initialization**: Randomly initializing network weights.
- **Bagging**: Randomly re-sampling dataset subsets.
  - [Random initialization may not only be sufficient but preferred over bagging for deep networks given their large parameter space and the neccessity of large training data.](https://arxiv.org/abs/1511.06314)
- **Architecture Diversity**: Ensemble networks with diverse architectures.
- **Parameter Sharing**: A TreeNet is an ensemble consisting of zero or more shared initial layers, followed by a branching point and zero or more independent layers. During training, the shared layers above a branch receive gradient information from each child network, which are accumulated according to back-propagation. At test time, each path from root to leaf can be considered an independent network, except that redundant computations at the shared layers need not be performed.
- **Ensemble-Aware Losses**
  - **Directly Optimizing for Model Averaging**: Optimize the performance of the corresponding Ensemble-Mean loss during training.  
    - [Explicitly optimizing for the performance of Ensemble-Mean does worse than averaging independently trained models. This may be due to two problems: **lack of diversity** and **numerical instability**.](https://arxiv.org/abs/1511.06314)
  - **Adding Diversity via Multiple Choice Learning**:  Consider a set of predictors $\{\theta _1,...,\theta_M\}$ such that $\theta_m: x \rightarrow P$ where P is a probability distribution over some set of labels. $L_{set}(x,y) = \mathop{min}\limits_{m\in[1,M]}l(\theta_m(x),y)$
  

#### Ensemble Loss Defination
- **Ensemble Loss**：$l(F(x),y), F(x)=\frac{1}{M}{\sum_{i=1}^M}f_{\theta_i}(x)$ 
- **Average Base Learner Loss**：$\frac{1}{M}{\sum_{i=1}^M}l(f_{\theta_i}(x),y)$ 
- **Oracle Ensemble Loss**
   - $l(F_{OE}(x),y), F_{OE}(x)=f_{\theta_k}(x)$, where $k\in \mathop{argmin}\limits_{i}l(f_{\theta_i}(x),y)$
   - Small oracle ensemble loss indicates more diverse base learner predictions.
   - $l(F_{OE}(x),y) \leq l(F(x),y) \leq \frac{1}{M}{\sum_{i=1}^M}l(f_{\theta_i}(x),y)$