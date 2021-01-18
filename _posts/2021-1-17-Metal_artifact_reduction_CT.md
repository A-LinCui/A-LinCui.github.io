---
layout:     post
title:      Metal Artifact Reduction for Medical CT
subtitle:   Undergraduate Graduation Project
date:       2021-1-17
author:     A-LinCui
header-img: img/post/night.png
catalog: true
mathjax: true
tags:
    - Deep Learning
---

### Paper list

| Domain | Year | Paper |
|:----:|:----:|:----:|
| | 2016 | [Low-Dose CT via Deep Neural Network](https://arxiv.org/abs/1609.08508) | 
| | 2016 | [Low-Dose CT with a Residual Encoder-Decoder Convolutional Neural Network (RED-CNN)](https://arxiv.org/abs/1702.00288v3) |
| | 2016 | [Image Prediction for Limited-angle Tomography via Deep Learning with Convolutional Neural Network](https://arxiv.org/abs/1607.08707) |
| Image | 2019 | [Generative Mask Pyramid Network for CT/CBCT Metal Artifact Reduction with Joint Projection-Sinogram Correction](https://arxiv.org/abs/1907.00294) |
| Image | 2019 | [Metal artifact reduction for practical dental computed tomography by improving interpolation‐based reconstruction with deep learning](https://aapm.onlinelibrary.wiley.com/doi/10.1002/mp.13644) |
 Image | 2020 | [Unsupervised domain adaptation for practical metal artefact reduction in X-ray CT]() |

### Challenges
#### Different distribution of simulated dada and practical data
Lack of practical data, simulated data is always required, especially for supervised learning. However, due to the complicated physical process, data directly simulated with metal artefact always lies in different distribution, leading to pool generalization ability of trained models.

### Papers
#### [Metal artifact reduction for practical dental computed tomography by improving interpolation‐based reconstruction with deep learning](https://aapm.onlinelibrary.wiley.com/doi/10.1002/mp.13644)
- **Abstract**: To solve the problem caused by different distribution between directly simulated data and practical data, replace data  related to metal fillings with interpolation both in training process and inference process. Therefore, the complicated physical process is bypassed and the distribution becomes near.
- **Dataset Simulation**
  - High-quality metal-free images were chosen.
  - Metal fillings were manually randomly added on the teeth.
  - The traces of the metal projections were determined by forward projecting the metal regions.
  - Linear interpolation was used to replace the metal-related projections.
  - **Final inputs**: The simulated inputs were FBP reconstructions of the projection data with the interpolated projections. 
  - **Final labels**: The high-quality images that did not contain metal fillings were used as the corresponding labels.
  - Notation: 
    - The process doesn't use the actual physical projections of the metal fillings. 
    - The manually added metal fillings were only used to locate the metal traces, as a result only the metal filling shapes and locations influenced the results.
- **Inference**
  - Recognize metal-related projections in a sinogram by image-domain thresholding and do bilinear interpolation for these projections.
  - Analytically develop a preliminary reconstruction from the interpolated sinogram.
  - Use a trained deep learning network to remove the artifacts in the preliminary reconstruction and recover the nonmetal information. 
- **Shortcomings**: May introduce new artefacts or wrong structure in the situation of multiple metals or inaccurate segmentation. 

#### [Unsupervised domain adaptation for practical metal artefact reduction in X-ray CT]()
- **Abstract**: Model the different distribution problem as a domain-invariant problem and solve it by domain-invariant adaption. A domain classifier is applied to classify source domain and target domain, with the features from the shallow layers of the MAR network as inputs. And the corresponding regularization term is added to the original supervised loss term to close the distance between two domains.
- **Formulation**: 
  - $\Phi(\theta_R) = \overset{N_S}{\mathop{\sum}\limits_{n=1}}{((R(\mu_n^{a,S};\theta_R)-\mu_n^*))}_1 + \lambda\varphi^{DD}(\mathcal{S},\mathcal{T};\theta_R)$
  - $\hat{\theta_R} = \mathop{argmin}\limits_{\theta_R} \Phi(\theta_R)$
