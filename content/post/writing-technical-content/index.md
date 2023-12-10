---
title: 	Architecture Modeling and What It Empowers
date: 2023-12-10
math: true
# image:
#  placement: 2
#  caption: 'Image credit: [**John Moeses Bauan**](https://unsplash.com/photos/OGZtQF8iC0g)'
---


With the wide application of deep learning technology, the updated speed of neural architecture and demand for computing power is increasing daily. As a result, there is an urgent need for multi-level optimization for task performance and hardware overhead, from neural network architecture to hardware architecture.

However, the search space is always so huge that efficient evaluation methods are required to accelerate the exploration. For example, use a simulator or look-up table to evaluate hardware overhead and apply weight sharing approach to evaluate neural architecture performance. Unfortunately, efficiency and accuracy do not always go hand in hand. In other words, acceleration of the search process usually comes at the expense of discovering sub-optimal architectures. To solve this problem, learning architecture modeling to accelerate exploration and improve evaluation is an important research direction.

## Architecture Representation
Before formally discussing architecture modeling, we first briefly describe the architecture representation in neural architecture search. The so-called architecture representation is a structure that can represent a certain architecture in the search space. This is helpful for understanding the methodology of architectural modeling and its implications.

### Squential Architecture Representation
Considering the search space as shown in Figure 1, we search for operations between nodes from four candidate operations. The simplest architecture representation method is to serialize the architecture with the operation index on each edge sequentially. For example, the discovered architecture shown in Figure 1(d) can be represented as [3, 0, 1, 3, 2, 2].

This representation schema is called sequential architecture representation. On the one hand, sequential representation is so simple and general that it can represent architectures in any search space (topological or non-topological) as long as all the decisions are placed together in order. On the other hand, sequential representation also supports searching for other automatic machine learning elements such as learning rate, training epoch, optimizer type, etc. by simply adding the element to the list.

However, the drawbacks of sequential architecture representation are also prominent. On the one hand, this schema does not model the architecture topology. Unless knowing the specific encoding rules, we cannot infer the architecture from the architecture representation. On the other hand, decision representation also creates confusion. For example, considering the search space shown in Figure 1, although the difference between 3 and 0 is more significant than that between 2 and 0 numerically, it does not indicate that ''blue'' is more different from ''none'' than ''green''.

### Adjacent Matrix Architecture Representation
Neural architecture search is always performed in cell-based topological search spaces (e.g., DARTS [Liu et al., ICLR 2019] , ENAS [Pham et al., ICML 2018]). Since the cell can be modeled as a directed acyclic graph (DAG), we can represent the architecture with an adjacency matrix. For example, the architecture in Figure 1(d) can be represented as a 4 x 4 matrix A in Figure 1 (e), where A[i][j] denotes the selected operation index from node j to node i.

To a certain extent, the adjacent matrix architecture representation models architecture topology. However, many problems of the sequential architecture representation remain unsolved. Last but not least, this representation schema is not applicable in non-topological search spaces.

## Architecture Modeling
### Significance of Architecture Modeling
In this section, we start with the predictor-based neural architecture search to understand the significance of architecture modeling.

**Predictor-based NAS** methods utilize an approximate architecture performance predictor to sample architectures more worthy of evaluation, thereby reducing the number of architectures that need to be evaluated for more accurate but slower evaluation strategies. As show in Figure 2, a typical architecture performance predictor takes an architecture representation (such as an adjacency matrix) as input and predicts performance scores as output. It usually consists of two parts, an architecture encoder and a multilayer perceptron (MLP). Among them, the architecture encoder encodes the input architecture representation into a latent representation, and the multilayer perceptron maps the latent representation to the prediction score.

For predictor-based neural architecture search, existing architecture encoding schemes mainly include sequence and graph-based schemes. Sequence-based schemes use specific serialization methods to convert architectural decisions into sequences (sequential architecture representation), which are then fed into a multilayer perceptron or recurrent neural network to obtain architectural representations. Graph-based architecture encoders apply graph convolutional networks to encode neural network architectures. Architecture predictors can generally give an approximate performance estimate of architecture in milliseconds and are, therefore, very efficient.

**The process of encoding the architecture is itself the process of modeling the architecture. With limited data, the predictive power of architecture performance predictors is closely related to the applied architectural encoders.** Figure 3 compares four architecture encoders on four benchmarks. It can be observed that trained on the same training samples, the prediction ability of predictors using different architecture encoders is significantly different. For example, GNN-based TA-GATES achieves significantly better prediction precision than the most naive MLP (sequence encoding scheme).

Why is the architecture modeling in predictor-based NAS such significant?

- On the one hand, an exemplary architecture modeling scheme can extract and integrate valid information from the architectural representation.
- On the other hand, prior knowledge can be introduced to reduce the data requirement further.

Take the sequence-based schemes as an example. As mentioned above, although sequence-based can accurately represent a specific architecture, it can hardly reflect the inherent properties of the architecture. For example, they are challenging to reflect topological information, and numerical distances between encodings cannot reflect fundamental performance differences. Such problems make it difficult to mine enough adequate information from limited data and not rank the performance of different architectures well, thus affecting the search results.

Figure 4 illustrates the gerneral working flow and various applications of architecture modeling：

- Architecture search acceleration by sampling architectures that are more worth exploring (as discussed above).
- More accurate architecture performance estimation that benefits architecture search.
- Architecture transformation that tells us how to transform an architecture for better performance under a certain application scenario.
- Architecture understanding that conducts model-based explanation of the relationship between architecture pattern and performance.

For the first application, we conduct a series of studies (GATES, TA-GATES, GATES++, DELE, Gibbon) to design advanced architecture encoders or training strategies. For the second application, we propose a novel CLOSENet that decouples the parameters from operations to improve one-shot estimation quality in CLOSE. And for the third application, we transform the architectures of given networks to improve the AI security under various attacks in DeepGuiser. For the last application, some studies (e.g, NAS-Bowl [Ru et al., ICLR 2021]) gain understanding of the relationship between architecture and performance in a search space.

In the following sections, we will further introduce the application of architecture modeling to these tasks.

### Architecture Modeling Application：Architecture Search
As discussed above, architecture modeling plays a crucial role in neural architecture search. Our work centers around a core idea: Through learnable modeling of the search space (i.e., a performance predictor of architectures), we can know which search space regions are worth exploring, and thus accelerate the exploration process. This section will take our researches as the main thread and share our views on the application of architecture modeling to architecture search tasks. Specifically, we'll introduce:

- The role of architecture modeling in predictor-based NAS.
- The significance of architecture modeling for improving one-shot evaluation.
- The role of architecture modeling in architecture-hardware joint search.

#### Architecture Modeling in Predictor-based Neural Architecture Search
Predictor-based NAS trains an approximate performance predictor and utilizes it to rank unseen architectures without actually training them. Therefore, once we have a predictor that can reliably rank the performance of unseen architectures, the architecture exploration can be significantly accelerated. However, predictor-based NAS suffers from the severe “cold-start” problem: It usually takes quite a considerable cost to acquire the architecture-performance data needed for training a working predictor from scratch.

To alleviate the need for training data for predictor-based neural architecture search, we sequentially conduct three studies： GATES, TA-GATES, and DELE. As shown in Figure 5, while the former two studies focus on designing more reasonable architecture encoders, the latter focuses on designing a more data-efficient training manner for better architecture modeling. We will briefly introduce them below.

<div align=center>
<img src="./figures/arch_modeling.png">
<br>Figure 4：Architecture modeling can enable a variety of applications.</div>
