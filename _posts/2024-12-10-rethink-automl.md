---
title: 'Rethink the Future of AutoML'
summary: 'A brief discussion about AutoML at present and in the future.'
date: 2024-12-10
share: false
permalink: /posts/rethink-the-future-of-automl
tags:
  - Automated Machine Learning
pager: false
---

<div style="text-align:justify; font-size:14px;">

<h2>The Era's Challenges in Neural Architecture Search</h2>

<p>Looking back at history, modern neural architecture search (NAS) technology emerged in 2016, when convolutional neural networks (CNNs) were highly favored for computer vision tasks. Although CNNs are primarily composed of convolutions, the different combinations of various modules create diverse network architectures. Different CNN architectures are suitable for different tasks and hardwares, thus leading to a thriving research field related to architectures, such as SqueezeNet, MobileNet, and others. Subsequently, with the introduction of methods like differentiable search, predictor-based search, one-shot evaluation strategies, and more, the entire research field entered its golden age around 2018. During this time, architectures discovered through NAS were indeed applied in practical scenarios, such as MobileNet-V3.</p>

<p>However, with the advent of Vision Transformers (ViTs), this new kind of architecture quickly became the preferred alternative to CNNs. And due to their relatively simple structure and unified architecture paradigm, they can be easily adapted to different modalities, tasks, and hardware. Their popularity has led to an increasing homogenization in neural network design. Specifically, mature Transformer architectures typically have a fixed topology, with only a few non-topological hyperparameters requiring tuning. This poses severe challenges to NAS technology from different perspectives.</p>

<p>Firstly, <b>the feasibility of NAS.</b> ViTs typically require pre-training on large-scale datasets before being transferred to downstream tasks. Compared to the traditional practice of pre-training CNNs on the ImageNet-1k dataset with millions of samples, the scale and cost of datasets used for pre-training ViTs are incomparable. For instance, ViT utilizes the JFT-3B dataset with approximately 3 billion training samples for pre-training, requiring a total computation volume of hundreds to thousands of TPUv3-core days. For NAS, the cost of evaluating the performance of candidate architectures increases drastically, posing severe challenges to the feasibility of the entire search system.</p>

<p>Secondly, <b>the significance of NAS.</b> For a long time, researchers have generally believed that the performance of neural networks is closely related to their architecture. However, the success of models like Transformers in recent years has proven that, with sufficient training data, even simple architectures can achieve superior performance. To a large extent, this is because simple architectures are not influenced by biases introduced by human priors. In particular, some studies have even revitalized the most primitive multilayer perceptrons (MLPs) by using large-scale datasets. Therefore, researchers have to ponder whether simply increasing the amount of training data is more effective than spending vast computational resources to improve neural network architectures.</p>

<p>In recent years, with the emergence of large language models (LLMs), the aforementioned challenges have become even more severe. On the one hand, LLMs require higher training costs compared to earlier Transformer models, making it almost impossible for researchers to apply existing NAS techniques to their architecture design. On the other hand, the performance of LLMs is more closely related to training data and methods, and the performance differences between architectures have become even more subtle. In the future era of LLMs, NAS may be completely marginalized.</p>

<h2>The Future Significance of Neural Architecture Search</h2>

<p>NAS, and even the entire field of Automated Machine Learning (AutoML), is not without value in the future. In my personal opinion, there are at least three areas where its application or research holds value.</p>

<p>Firstly, <b>architecture tuning for edge devices under extreme conditions.</b> Despite the continuous increase in computational resources available to people today and the continuous improvement of neural network deployment environments, there are still deployment scenarios for edge devices under certain extreme conditions. For example, within nuclear power facilities, there may be a need for small neural network models that are highly fault-tolerant, robust, and low-cost. These models may have extremely stringent requirements for certain performance indicators, and therefore, neural architecture search methods must be utilized for targeted tuning.</p>

<p>Secondly, <b>the research on the interpretability of neural networks.</b> Neural networks have achieved SOTA performance across various tasks and have been extremely widely applied. However, for researchers, neural networks remain almost entirely black-box models. Although some studies have attempted to theoretically explain neural networks, due to limitations in mathematical tools and others, relevant research has not yet made breakthrough progress. AutoML methods can find the best configurations based on given conditions, for example, NAS looks for the optimal architecture within a certain search space, thereby allowing the analysis of the connection between model performance and different configurations. This aids researchers in interpreting neural networks. For instance, Ru et al. designed an architecture search based on Bayesian models and utilized Bayesian models to analyze the relationship between architecture patterns and performance without the need for actual training of the architectures.</p>

<p>Finally, <b>the transfer and application of methodologies.</b> NAS can essentially be viewed as an optimization problem: given a search space and relevant constraints, the goal is to find a neural network architecture that best meets the optimization objective. Being considered an optimization problem, the optimization methods can also be transferred to other optimization problems. For example, NAS and structured pruning can be seen as two sides of the same coin. The former creates an architecture from scratch within the search space, while the latter retains an architecture from an existing model. However, essentially, both are engaged in architecture tuning for the same optimization objective. Specifically, in structured pruning, algorithms need to model the relationship between retained channels and performance, so NAS methods such as predictor-based methods and differentiable methods can be used for modeling this task. Relevant research has already made attempts, for example, DSA draws on differentiable architecture search methods to design a differentiable structured pruning strategy.</p>

</div>
