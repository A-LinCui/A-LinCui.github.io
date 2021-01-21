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

#### Ensemble Patterns
- **Random Initialization**: Randomly initializing network weights.
- **Architecture Diversity**: Ensemble networks with diverse architectures.
- **Parameter Sharing**: A TreeNet is an ensemble consisting of zero or more shared initial layers, followed by a branching point and zero or more independent layers. During training, the shared layers above a branch receive gradient information from each child network, which are accumulated according to back-propagation. At test time, each path from root to leaf can be considered an independent network, except that redundant computations at the shared layers need not be performed.
- **Stacking**: Stack ouputs of each sub-architecture.
- **Voting**
- **Boosting**: Serialize.The basic idea is to increase the weight of the samples that the previous base learner predicted wrong, so that the subsequent base learner pays more attention to these mislabeled samples and tries its best to correct these errors.T base learners are trained until the weighted combination of these T base learners is finally achieved.
- **Bagging**
- **Averaging**
- **Weight Averaging**
  
#### Ensemble Loss
- **Ensemble Estimating Loss**
  - **Ensemble Loss**：$l(F(x),y), F(x)=\frac{1}{M}{\sum_{i=1}^M}f_{\theta_i}(x)$ 
  - **Average Base Learner Loss**：$\frac{1}{M}{\sum_{i=1}^M}l(f_{\theta_i}(x),y)$ 
  - **Oracle Ensemble Loss**
    - $l(F_{OE}(x),y), F_{OE}(x)=f_{\theta_k}(x)$, where $k\in \mathop{argmin}\limits_{i}l(f_{\theta_i}(x),y)$
    - Small oracle ensemble loss indicates more diverse base learner predictions.
    - $l(F_{OE}(x),y) \leq l(F(x),y) \leq \frac{1}{M}{\sum_{i=1}^M}l(f_{\theta_i}(x),y)$
- **Ensemble-Aware Losses**
  - **Directly Optimizing for Model Averaging**: Optimize the performance of the corresponding Ensemble-Mean loss during training.  
    - [Explicitly optimizing for the performance of Ensemble-Mean does worse than averaging independently trained models. This may be due to two problems: **lack of diversity** and **numerical instability**.](https://arxiv.org/abs/1511.06314)
  - **Adding Diversity via Multiple Choice Learning**:  Consider a set of predictors $\{\theta_1,...,\theta_M \}$ such that $\theta_m: x \rightarrow P$ where P is a probability distribution over some set of labels. $L_{set}(x,y) = \mathop{min}\limits_{m \in [1,M]} l(\theta_m(x),y)$

#### Ensemble NAS
- **AdaNAS**: Adjust network capacity with ensemble instead of common-used architecture augment. Apply greedy algorithm to build the final ensemble architecture from a sub-architecture. In each iteration, sample some architectures with a generator. And each newly sampled architecture is combined with the knowledge distillation training of all the previously selected sub-architectures. Select the best new architecture into the ensemble architecture. Until the preset capacity is reached.

#### Ensemble Insights
- The ideal ensemble is one comprised of accurate networks that make errors on different parts of the input space.
- The large ensembles of neural network models can be summarized with a relatively small number of representative models selected via clustering based on distances between model outputs.
- The increased diversity of network structure in the ensemble produces increased diversity in predictions of the networks leading to improved ensemble performance.

----------------
#### Whimsicality
- **Idea 1**
  - **Asumption**: Logit domain information is consistent for each sub-architecture in the one-shot model with itself after seperately trained.
  - **Method**: Classify candidate architectures into several classes clustering in advance. Sample sub-architectures separately in each class during the search.
- **Idea 2**
  - **Asumption**: The gap between the different architecture domains is mainly caused by the upper topology.
  - **Method**: Search for a parameter-sharing treeNet with only the upper-topologies different.
- **Idea 3**
  - **Asumption**: Distance between archiectures indicates distance between their domain.
  - **Method**: Search with the guidance of topology distance.

#### To be Discussed
- Evolutionary search is much more efficient than random sampling in the one-shot model.
- The correlationship between performance in the one-shot model and performance after training from scratch. Theory confirmation or empirical evidence.
- How does the ensemble accuracies improve in one-shot model?
- **Development**
  - EnsembleFinalTrainer: Parallel training and joint training.
  - Different inner search spaces 
  
---------------

| Class | Year | Paper |
|:----:|:----:|:----:|
| Ensemble Pattern | 2010 | [Ore Grade Prediction Using a Genetic Algorithm and Clustering Based Ensemble Neural Network Model](https://link.springer.com/article/10.1007/s11004-010-9264-y) |
| Ensemble Pattern | 2015 | [Why M Heads are Better than One: Training a Diverse Ensemble of Deep Networks](https://arxiv.org/abs/1511.06314) |
| Ensemble Pattern | 2016 | [Simple and Scalable Predictive Uncertainty Estimation using Deep Ensembles](https://arxiv.org/abs/1612.01474) |
| Ensemble Insights | 2003 | [Clustering ensembles of neural network models](https://www.sciencedirect.com/science/article/abs/pii/S0893608002001879?via%3Dihub) |
| Ensemble NAS | 1996 | [Actively Searching for an Effective Neural-Network Ensemble](http://ftp.cs.wisc.edu/machine-learning/shavlik-group/opitz.consci96.pdf) |
| Ensemble NAS | 2001 | [Genetic Algorithm based Selective Neural Network Ensemble](https://www.researchgate.net/publication/2517848_Genetic_Algorithm_based_Selective_Neural_Network_Ensemble) |
| Ensemble NAS | 2018 | [Ensembles of Networks Produced from Neural Architecture Search](https://www.springerprofessional.de/en/ensembles-of-networks-produced-from-neural-architecture-search/18504652) |
| Ensemble NAS | 2019 | [Improving Neural Architecture Search Image Classifiers via Ensemble Learning](https://arxiv.org/abs/1903.06236) |
| Ensemble NAS | 2020 | [Neural Ensemble Search for Performant and Calibrated Predictions](https://arxiv.org/abs/2006.08573) |