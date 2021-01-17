---
layout:     post
title:      Metal artifact reduction for Medical CT
subtitle:   Undergraduate Graduation Project
date:       2021-1-17
author:     A-LinCui
header-img: img/post/night.png
catalog: true
mathjax: true
tags:
    - Deep Learning
---

#### Paper list

| Domain | Year | Paper |
|:----:|:----:|:----:|
| | 2016 | [Low-Dose CT via Deep Neural Network](https://arxiv.org/abs/1609.08508) | 
| | 2016 | [Low-Dose CT with a Residual Encoder-Decoder Convolutional Neural Network (RED-CNN)](https://arxiv.org/abs/1702.00288v3) |
| | 2016 | [Image Prediction for Limited-angle Tomography via Deep Learning with Convolutional Neural Network](https://arxiv.org/abs/1607.08707) |
| Image | 2019 | [Generative Mask Pyramid Network for CT/CBCT Metal Artifact Reduction with Joint Projection-Sinogram Correction](https://arxiv.org/abs/1907.00294) |
| Image | 2019 | [Metal artifact reduction for practical dental computed tomography by improving interpolation‐based reconstruction with deep learning](https://aapm.onlinelibrary.wiley.com/doi/10.1002/mp.13644) |

#### [Metal artifact reduction for practical dental computed tomography by improving interpolation‐based reconstruction with deep learning](https://aapm.onlinelibrary.wiley.com/doi/10.1002/mp.13644)
- **Motivation**: The problem is that a deep learning network trained with simulation data often cannot be transferred to practical applications. The key idea is, instead of directly simulating images containing metal artifacts, this method simulates images containing interpolation artifacts by replacing data related to metal fillings with interpolation.
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