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


| Class | Paper | Year |
|:----:|:----:|:----:|
| Ensemble Pattern | [Simple and Scalable Predictive Uncertainty Estimation using Deep Ensembles](https://arxiv.org/abs/1612.01474) | 2016 |
| Ensemble NAS | [Neural Ensemble Search for Performant and Calibrated Predictions](https://arxiv.org/abs/2006.08573) | 2020 |


#### Ensemble Patterns
- **Deep Ensemble**: 固定一个架构，不同的初始化训练多个模型，然后集成在一起。

#### Ensemble Loss
- **Ensemble Loss**：$l(F(x),y), F(x)=\frac{1}{M}{\sum_{i=1}^M}f_{\theta_i}(x)$ 
- **Average Base Learner Loss**：$\frac{1}{M}{\sum_{i=1}^M}l(f_{\theta_i}(x),y)$ 
- **Oracle Ensemble Loss**
   - $l(F_{OE}(x),y), F_{OE}(x)=f_{\theta_k}(x)$, where $k\in \mathop{argmin}\limits_{i}l(f_{\theta_i}(x),y)$
   - Small oracle ensemble loss indicates more diverse base learner predictions.
   - $l(F_{OE}(x),y) \leq l(F(x),y) \leq \frac{1}{M}{\sum_{i=1}^M}l(f_{\theta_i}(x),y)$