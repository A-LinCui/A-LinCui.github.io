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


### Challenges
#### Different distribution of simulated dada and practical data
- Lack of practical data. Simulated data is always required, especially for supervised learning. However, due to the complicated physical process, data directly simulated with metal artefact always lies in different distribution, leading to pool generalization ability of trained models.
- For unsupervised learning. The GANs cannot perfectly module the actual distribution, causing loss on real structures. And a large number of hyper-parameters need to be to adjusted manually, reducing their feasibility and reasonabily.

### Whimsicality
- NAS for better architectures. Some article did report the performance is correlated with the architecture.
- Combine with adversarial attack to module small perturbation. It has been implemented in an article. However, the perturbation is randomly searched. We may apply other adversaries.
- Transformer for better global information extraction.
- Distillation / Mutual learning.
- Auto loss.

### Schedule
#### **2021.2.23-2021.3.1**: Construct the basic flow.
#### **2021.2.23-2021.3.8**: First presentation. And determine which direction do work on.

--------------------------------
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
  - $\Phi(\theta_R) = \overset{N_S}{\mathop{\sum}\limits_{n=1}}{\|\|R(\mu_n^{a,S};\theta_R)-\mu_n^*\|\|}_1 + \lambda\varphi^{DD}(\mathcal{S},\mathcal{T};\theta_R)$
  - $\hat{\theta_R} = \mathop{argmin}\limits_{\theta_R} \Phi(\theta_R)$

#### [Generative Mask Pyramid Network for CT/CBCT Metal Artifact Reduction with Joint Projection-Sinogram Correction](https://arxiv.org/abs/1907.00294)
- **Abstract**: Modulate the problem as an inpainting problem. Learn two GANs. The first GAN, enhenced with a mask pyramid network, called projection completion module, is used to inpaint the projection image at each angle. The second GAN, called sigogram correction module, is used to inpaint the total sigogram. The mask pyramid network is used to generate a mask that softly restrains the GAN to pay attention to metal area.

#### [Convolutional Neural Network Based Metal Artifact Reduction in X-ray Computed Tomography](https://arxiv.org/abs/1709.01581v2)
- **Abstract**: Develop a convolutional neural network based open MAR framework, which fuses the information from the original and corrected images to suppress artifacts.
- **Work Flow**:
  - Metal trace segmentation
  - Artifact reduction with the LI and BHC
  - Artifact reduction with the trained CNN
  - Generation of a CNN prior image using tissue processing
  - Replacement of metal-affected projections with the forward projection of CNN prior, followed by the FBP reconstruction
- **Insights**
  - Performance is correlated to model architectures. And with the increase of capacity, the performance tends to increase.
  - NAS can be applied.
  - The generalization of the method is bad for new types of metal artifacts. 
- **Problem**: The working flow is rediculous to understand. Many unreasonable flows exist. 

#### [ADN: Artifact Disentanglement Network for Unsupervised Metal Artifact Reduction](https://arxiv.org/abs/1908.01104)
- **Abstract**: The first article to introduce unsupervised learning.
- **Problem**: The network is too complicate.
- **Code Available**: [https://github.com/liaohaofu/adn](https://github.com/liaohaofu/adn)

#### [Regularized Three-dimensional Generative Adversarial Nets for Unsupervised Metal Artifact Reduction](https://arxiv.org/abs/1911.08105)
- **Abstract**: 3D, unsupervised.
- **Volume-to-volume translation**: Translate volumn-to-volumn due to the poor performance on large global artifacts. It may result from the inherent weakness of CNNs. May apply transformer or other self-attention block?

#### [Adversarial Robust Training of Deep Learning MRI Reconstruction Models](https://arxiv.org/pdf/2011.00070.pdf)
- **Abstract**: Adversarial attack. The adversarial perturbation is randomly searched. 
- **Code Available**: [https://github.com/fcaliva/fastMRI_BB_abnormalities_annotation](https://github.com/fcaliva/fastMRI_BB_abnormalities_annotation)

#### [Deep Sinogram Completion with Image Prior for Metal Artifact Reduction in CT Images](https://arxiv.org/abs/2009.07469)
- **Method**: ![MAR1](https://github.com/A-LinCui/A-LinCui.github.io/raw/master//img/post/MAR_1.png)
- **Comment**: Rediculous!
  
---------------------
### Paper list

| Year | Type | Domain | Paper |
|:----:|:----:|:----:|:----:|
| 2016 | | | [Low-Dose CT via Deep Neural Network](https://arxiv.org/abs/1609.08508) | 
| 2016 | | | [Low-Dose CT with a Residual Encoder-Decoder Convolutional Neural Network (RED-CNN)](https://arxiv.org/abs/1702.00288v3) |
| 2016 | | | [Image Prediction for Limited-angle Tomography via Deep Learning with Convolutional Neural Network](https://arxiv.org/abs/1607.08707) |
| 2018 | Supervised | Image | [Convolutional Neural Network Based Metal Artifact Reduction in X-ray Computed Tomography](https://arxiv.org/abs/1709.01581v2) |
| 2019 | Unsupervised | Image | [Generative Mask Pyramid Network for CT/CBCT Metal Artifact Reduction with Joint Projection-Sinogram Correction](https://arxiv.org/abs/1907.00294) |
| 2019 | Supervised | Image | [Metal artifact reduction for practical dental computed tomography by improving interpolation‐based reconstruction with deep learning](https://aapm.onlinelibrary.wiley.com/doi/10.1002/mp.13644) |
| 2019 | Unsupervised | Image | [ADN: Artifact Disentanglement Network for Unsupervised Metal Artifact Reduction](https://arxiv.org/abs/1908.01104) |
| 2019 | Unsupervised | Image | [Regularized Three-dimensional Generative Adversarial Nets for Unsupervised Metal Artifact Reduction](https://arxiv.org/abs/1911.08105) |
| 2020 | Unsupervised | Image  | [Unsupervised domain adaptation for practical metal artefact reduction in X-ray CT]() |
| 2020 | Supervised | Image | [Adversarial Robust Training of Deep earning MRI Reconstruction Models](https://arxiv.org/pdf/2011.00070.pdf) |
| 2020 | Supervised | Image & Sinogram | [Deep Sinogram Completion with Image Prior for Metal Artifact Reduction in CT Images](https://arxiv.org/abs/2009.07469) |

---------------------
### Server
`ssh root@166.111.32.16 -p 30058`