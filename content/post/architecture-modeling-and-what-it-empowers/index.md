---
title: Architecture Modeling and What It Empowers
summary: A brief discussion about architecture modeling in neural architecture search.
date: 2023-12-10
share: false
pager: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**https://www.automl.org/nas-overview**](https://www.automl.org/nas-overview)'

authors:
  - admin

featured: true

tags:
  - AutoML
  - NAS
---

<div style="text-align:justify; font-size:14px;">

With the wide application of deep learning technology, the updated speed of neural architecture and demand for computing power is increasing daily. As a result, there is an urgent need for multi-level optimization for task performance and hardware overhead, from neural network architecture to hardware architecture.

However, the search space is always so huge that efficient evaluation methods are required to accelerate the exploration. For example, use a simulator or look-up table to evaluate hardware overhead and apply weight sharing approach to evaluate neural architecture performance. Unfortunately, efficiency and accuracy do not always go hand in hand. In other words, acceleration of the search process usually comes at the expense of discovering sub-optimal architectures. To solve this problem, learning architecture modeling to accelerate exploration and improve evaluation is an important research direction.

## Architecture Representation
Before formally discussing architecture modeling, we first briefly describe the architecture representation in neural architecture search. The so-called architecture representation is a structure that can represent a certain architecture in the search space. This is helpful for understanding the methodology of architectural modeling and its implications.

### Squential Architecture Representation
Considering the search space as shown in Figure 1, we search for operations between nodes from four candidate operations. The simplest architecture representation method is to serialize the architecture with the operation index on each edge sequentially. For example, the discovered architecture shown in Figure 1(d) can be represented as [3, 0, 1, 3, 2, 2].

This representation schema is called sequential architecture representation. On the one hand, sequential representation is so simple and general that it can represent architectures in any search space (topological or non-topological) as long as all the decisions are placed together in order. On the other hand, sequential representation also supports searching for other automatic machine learning elements such as learning rate, training epoch, optimizer type, etc. by simply adding the element to the list.

However, the drawbacks of sequential architecture representation are also prominent. On the one hand, this schema does not model the architecture topology. Unless knowing the specific encoding rules, we cannot infer the architecture from the architecture representation. On the other hand, decision representation also creates confusion. For example, considering the search space shown in Figure 1, although the difference between 3 and 0 is more significant than that between 2 and 0 numerically, it does not indicate that ''blue'' is more different from ''none'' than ''green''.

<div align=center>
<img src="./figures/darts.png">
Figure 1：An example search space. We search for operations on the edges from four candidate operations: none (denoted by 0), red (denoted by 1), green (denoted by 2) and blue (denoted by 3).</div>

### Adjacent Matrix Architecture Representation
Neural architecture search is always performed in cell-based topological search spaces (e.g., DARTS [Liu et al., ICLR 2019] , ENAS [Pham et al., ICML 2018]). Since the cell can be modeled as a directed acyclic graph (DAG), we can represent the architecture with an adjacency matrix. For example, the architecture in Figure 1(d) can be represented as a 4 x 4 matrix A in Figure 1 (e), where A[i][j] denotes the selected operation index from node j to node i.

To a certain extent, the adjacent matrix architecture representation models architecture topology. However, many problems of the sequential architecture representation remain unsolved. Last but not least, this representation schema is not applicable in non-topological search spaces.

## Architecture Modeling
### Significance of Architecture Modeling
In this section, we start with the predictor-based neural architecture search to understand the significance of architecture modeling.

**Predictor-based NAS** methods utilize an approximate architecture performance predictor to sample architectures more worthy of evaluation, thereby reducing the number of architectures that need to be evaluated for more accurate but slower evaluation strategies. As show in Figure 2, a typical architecture performance predictor takes an architecture representation (such as an adjacency matrix) as input and predicts performance scores as output. It usually consists of two parts, an architecture encoder and a multilayer perceptron (MLP). Among them, the architecture encoder encodes the input architecture representation into a latent representation, and the multilayer perceptron maps the latent representation to the prediction score.

<div align=center>
<img src="./figures/predictor-framework.png">
Figure 2: Typical architecture performance predictor framework. The architecture is first encoded by an architecture encoder and then fed into the MLP to output the final prediction score.</div>

<br>

For predictor-based neural architecture search, existing architecture encoding schemes mainly include sequence and graph-based schemes. Sequence-based schemes use specific serialization methods to convert architectural decisions into sequences (sequential architecture representation), which are then fed into a multilayer perceptron or recurrent neural network to obtain architectural representations. Graph-based architecture encoders apply graph convolutional networks to encode neural network architectures. Architecture predictors can generally give an approximate performance estimate of architecture in milliseconds and are, therefore, very efficient.

**The process of encoding the architecture is itself the process of modeling the architecture. With limited data, the predictive power of architecture performance predictors is closely related to the applied architectural encoders.** Figure 3 compares four architecture encoders on four benchmarks. It can be observed that trained on the same training samples, the prediction ability of predictors using different architecture encoders is significantly different. For example, GNN-based TA-GATES achieves significantly better prediction precision than the most naive MLP (sequence encoding scheme).

<div align=center>
<img src="./figures/TA-GATES-precision.png">
Figure 3：Precision@K comparison on the validation split of four benchmarks. X-axis：K; Y-axis: Precision. The training proportion is 5% on NB101 [Ying et al., ICML 2019], NB201 [Dong et al., ICLR 2020] , and NB301 [Siems et al., 2020], and 50% on NDS ENAS [Radosavovic et al., 2019].</div>

<br>

Why is the architecture modeling in predictor-based NAS such significant?

1) On the one hand, an exemplary architecture modeling scheme can extract and integrate valid information from the architectural representation.

2) On the other hand, prior knowledge can be introduced to reduce the data requirement further.

Take the sequence-based schemes as an example. As mentioned above, although sequence-based can accurately represent a specific architecture, it can hardly reflect the inherent properties of the architecture. For example, they are challenging to reflect topological information, and numerical distances between encodings cannot reflect fundamental performance differences. Such problems make it difficult to mine enough adequate information from limited data and not rank the performance of different architectures well, thus affecting the search results.

<div align=center>
<img src="./figures/arch_modeling.png">
Figure 4：Architecture modeling can enable a variety of applications.</div>

<br>

Figure 4 illustrates the gerneral working flow and various applications of architecture modeling：

1) Architecture search acceleration by sampling architectures that are more worth exploring (as discussed above).

2) More accurate architecture performance estimation that benefits architecture search.

3) Architecture transformation that tells us how to transform an architecture for better performance under a certain application scenario.

4) Architecture understanding that conducts model-based explanation of the relationship between architecture pattern and performance.

For the first application, a series of studies (GATES, TA-GATES, GATES++, DELE, Gibbon) are conducted to design advanced architecture encoders or training strategies. For the second application, a novel CLOSENet that decouples the parameters from operations is proposed to improve one-shot estimation quality in CLOSE. And for the third application, the architectures of given networks are transformed to improve the AI security under various attacks in DeepGuiser. For the last application, some studies (e.g, NAS-Bowl [Ru et al., ICLR 2021]) gain understanding of the relationship between architecture and performance in a search space.

In the following sections, we will further introduce the application of architecture modeling to these tasks.

### Architecture Modeling Application：Architecture Search
As discussed above, architecture modeling plays a crucial role in neural architecture search. A series of studies center around a core idea: Through learnable modeling of the search space (i.e., a performance predictor of architectures), we can know which search space regions are worth exploring, and thus accelerate the exploration process. This section will take several researches as the main thread and share our views on the application of architecture modeling to architecture search tasks. Specifically, we'll introduce:

1) The role of architecture modeling in predictor-based NAS.

2) The significance of architecture modeling for improving one-shot evaluation.

3) The role of architecture modeling in architecture-hardware joint search.

#### Architecture Modeling in Predictor-based Neural Architecture Search
Predictor-based NAS trains an approximate performance predictor and utilizes it to rank unseen architectures without actually training them. Therefore, once we have a predictor that can reliably rank the performance of unseen architectures, the architecture exploration can be significantly accelerated. However, predictor-based NAS suffers from the severe “cold-start” problem: It usually takes quite a considerable cost to acquire the architecture-performance data needed for training a working predictor from scratch.

To alleviate the need for training data for predictor-based neural architecture search, three studies are conducted sequentially： GATES, TA-GATES, and DELE. As shown in Figure 5, while the former two studies focus on designing more reasonable architecture encoders, the latter focuses on designing a more data-efficient training manner for better architecture modeling. We will briefly introduce them below.

<div align=center>
<img src="./figures/improve_search.jpg">
Figure 5：Researches  (Left: GATES; Middle: TA-GATES; Right: DELE) on architecture modeling in predictor-based neural architecture search.</div>

<br>

<a href="https://arxiv.org/abs/2004.01899" class="no-underline" style="color:blue">A Generic Graph-based Neural Architecture Encoding Scheme for Predictor-based NAS (GATES)</a>

Recognizing the high cost of getting actual architecture performance data as the major challenge for predictor-based NAS, to exploit information in the limited data more efficiently, we first GATES is proposed to learn the predictor in a more data-efficient way.

As described above, traditional architecture encoders treat architecture as a sequence or a simple DAG. However, they ignore that architecture should describe how the data flow and get processed. Following this intuition, the core of the proposed method is to mimic the data flow and processing process to encode the architecture. Specifically, in the encoding process of GATES, a piece of "virtual information" is taken as the input node embedding, and each operation is a transformation of the propagated information. Figure 6 shows an example of the encoding flow; more details can be found in our paper. The proposed GATES is a more suitable encoding method for data-processing directed acyclic graphs (DAGs) which matches the nature of this type of data. Indeed, it can intrinsically encode equivalent/isomorphic architectures to the same embedding.

<div align=center>
<img src="./figures/GATES-flow.png">
Figure 6：Illustration of the GATES encoding flow.</div>

<br>

<a href="https://a-lincui.github.io/publication/tagates/" class="no-underline" style="color:blue">TA-GATES: An Encoding Scheme for Neural Network Architectures</a>

Even if two operations are of the same type, they have different functionalities according to their architectural context. However, plain GATES does not give contextualized embeddings for different operations of the same type. To solve this problem, TA-GATES, a follow-up improvement to GATES, gets contextualized embeddings for different operations (even those of the same type).

To get a more discriminative encoding, the intuition behind the principled design of TA-GATES is "an architecture not only describes how the data flow and get processed in the forward propagation, but it also decides the learning dynamics of the model." Accordingly, it improves GATES from two aspects:

1) Analogy to Iterative Parameter Training: Mimicking the architecture training process to encode it by conducting several forward and backward passes on the architecture DAG and updating the operation embedding in each backward pass (as illustrated in Figure 5 (Middle)).

2) Analogy to Random Parameter Initialization: Applying symmetry breaking to the initial operation embeddings to enable TA-GATES to distinguish between symmetric operations (as illustrated in Figure 7).

<div align=center>
<img src="./figures/symmetry_breaking.png">
<br>Figure 7：A single forward pass in the encoding process without  / with  the symmetry-breaking.</div>

<div align=center>
<img src="./figures/TA-GATES-search.png">
Table 1：Accuracy of the discovered architectures on NB101, NB201, and NB301 with different encoders. On all benchmarks, the predictors are trained with a training proportion of 1%.</div>

<br>

Table 1 shows the accuracies of the discovered architectures on NAS-Bench-101 / 201 / 301 with different architectures. Compared with MLP, LSTM, and GCN, GATES discovered better architectures across different search spaces. Further, TA-GATES achieves better results than GATES. For example, on NAS-Bench-201, the average accuracy of discovered architectures by TA-GATES (0.9393) is 0.44% better than that by GATES (0.9349). These results demonstrate the effectiveness of GATES and TA-GATES.

<a href="https://a-lincui.github.io/publication/dele/" class="no-underline" style="color:blue">Dynamic Ensemble of Low-fidelity Experts: Mitigating NAS "Cold-Start"</a>

Based on the intuition that "low-fidelity information can be beneficial for learning the modeling," DELE focuses on exploiting more information in other cheaper-to-obtain performance estimations (i.e., low-fidelity information) to mitigate the data requirements of predictor training. However, the types of low-fidelity information helpful for performance prediction are unclear to practitioners beforehand. In addition, different types of low-fidelity information could provide beneficial information from different aspects, but the naive method described above can only utilize one type of low-fidelity information.

To solve the problem, a dynamic mixture-of-expert predictor framework (DELE) is proposed to fuse beneficial knowledge from different low-fidelity experts. Specifically, as shown in Figure 5 (Right), each low-fidelity expert is trained with one type of low-fidelity information (e.g., zero-shot evaluation scores, complexity scores, and so on). Then a dynamic ensemble of these experts is trained using only a small set of ground-truth performance data.

Compared to previous studies that improve architecture modeling by designing specialized predictor architectures or training losses, DELE can be combined with these studies to boost performance further. For example, as shown in Table 2, with 1% training samples on NAS-Bench-201, DELE increases Kendall's Tau correlation between predicted scores (GATES) and actual performance from 0.7332 to 0.8244. The increasing ranking correlation will further lead to better-discovered architectures.

<div align=center>
<img src="./figures/LFNAS-KD.png">
Table 2：The Kendall’s Tau (average over five runs) of using different encoders on NAS-Bench-201. And the standard deviation is in the subscript. “Vannila” represents directly training predictor with ground-truth accuracies without low-fidelity information utilization.</div>

<br>

#### Architecture Modeling for Improving One-shot Estimation
Besides predictor-based NAS, architecture modeling is also significant to one-shot evaluation.

One-shot NAS shares operation parameters among candidate architectures in a "supernet" and trains this supernet to evaluate all sampled candidate architectures. However, one-shot NAS suffers from the poor ranking correlation between one-shot estimations and stand-alone estimations. And the excessive sharing of parameters, i.e., the large sharing extent, has been widely regarded as the most critical factor causing unsatisfying performance estimation.

<a href="https://arxiv.org/abs/2207.07868" class="no-underline" style="color:blue">CLOSE: Curriculum Learning On the Sharing Extent Towards Better One-shot NAS</a>

To solve the problem brought by the traditional sharing mechanism, CLOSE achieves a win-win scenario of supernet training efficiency and high-ranking quality by adapting the sharing extent during the supernet training process.

The key idea behind CLOSENet is to decouple the parameters from operations to enable flexible sharing scheme and adjustable sharing extent. Specifically, as shown in Figure 8, CLOSE designs a novel CLOSENet, whose sharing extent can be easily adjusted to enable the adaption of the sharing extent during the training process. And CLOSE borrows the idea of curriculum learning to design a novel supernet training strategy, which not only accelerates the supernet training but also improves the saturating ranking quality of the supernet.

<div align=center>
<img src="./figures/CLOSE.png">
Figure 8：CLOSENet contains GLOW blocks to store the parameters of candidate operations, and the GATE module to assign GLOW blocks to operations.</div>

<br>

Figure 9 compares CLOSENet with vanilla supernets on four NAS benchmarks. CLOSENet achieves a higher KD and P@top5% on all the NAS benchmarks. Moreover, throughout the training process, CLOSENet consistently achieves higher ranking quality, which implies CLOSENet’s superiority to the vanilla supernet under any budget for supernet training.

<div align=center>
<img src="./figures/CLOSE-KD.png">
Figure 9：Comparison of different criteria with the vanilla one-shot supernet on four NAS benchmarks. X-axis: Training epochs. Y-axis: Evaluation criteria.</div>

<br>

#### Architecture Modeling in Architecture-Hardware Joint Search
Architecture modeling is not only for neural network architectures but also for hardware architectures.

As we know, neural network architecture and hardware architecture are two important aspects of the actual deployment of neural networks. However, most existing works either fix neural network architectures to optimize hardware architectures or design neural network architectures for a given hardware architecture, ignoring both co-design.

Neural Architecture Search (NAS) can be applied to explore the NN model and hardware architecture co-design space automatically. However, the vanilla NAS methods suffer from low exploration efficiency and long search time because of the explosive search space expansion and the time-consuming simulation of hardware architectures. To solve these problems, we need an efficient co-exploration framework for the NN model and hardware architecture by leveraging the power of architecture modeling.

<a href="https://ieeexplore.ieee.org/document/9774605" class="no-underline" style="color:blue">Gibbon: Efficient Co-Exploration of NN Model and Processing-In-Memory Architecture</a>

Gibbon proposes an evolutionary search algorithm with adaptive parameter priority to co-design the neural network architectures and the memristor-based Processing-In-Memory (PIM) architectures. Specifically, as shown in Figure 10, Gibbon models the neural network architecture and the PIM architecture jointly, and uses the difference form to model the impact of hardware irrational factors on the performance of the algorithm. The core of evaluation acceleration is an RNN-based NN accuracy and PIM performance predictor, which substitutes for a large part of the PIM simulator workload and reduces the long simulation time.

<div align=center>
<img src="./figures/GIBBON.png">
Figure 10: Illustration of the Gibbon framework.</div>

<br>

### Architecture Modeling Application：Architecture Transformation
Another application is the use of architectural modeling to disguise the transformation of the architecture.

In AI security, existing architecture stealing methods can steal network architecture through various side-channel information. Architecture stealing attacks such as these bring significant security risks. Attackers can steal the target model architecture and use it to migrate and confront attacks. This makes it easy to generate adversarial examples that mislead the target model.

<a href="https://openreview.net/forum?id=PArJcOptzg" class="no-underline" style="color:blue">DeepGuiser</a> proposes an architecture camouflage transformer to transform the architecture of a trained model while ensuring that the weights can be equivalently transformed to the new architecture without training. For example, changing conv3x3 into conv5x5 by padding zero on the outside of the convolution kernel, changing skip-connection into convolution by filling in appropriate weights. To realize this ideal usage scenario, DeepGuiser predicts whether each operation transformation can improve the defense against steal-migration attacks based on the modeling of the architecture operation level and directly gives the architecture transformation decision.

#### Architecture Modeling Application：Architecture Understanding
The third application of architecture modeling is architecture understanding. It is well known that different architectures have different performances. Some architectures can outperform humans, while others are entirely incompetent for the target task. Architecture search can help us find excellent architectures. However, the reason why the performance of architecture is good or bad still needs to be understood by researchers with the help of architecture modeling.

To understand the relationship between architectural patterns and performance, researchers need to analyze a large amount of architecture-performance data. However, the acquisition of architecture-performance data is often prohibitively expensive. Fortunately, some open source neural architecture search benchmarks and architecture-performance predictors provide convenience for researchers to understand architectures.

#### Utilizing Open-Source Benchmarks
Open-source benchmarks (e.g., NAS-Bench-101, NAS-Bench-201, NAS-Bench-301) provide researchers with a large amount of architecture-performance data. With the help of these benchmarks, EEPE evaluates two fast architecture evaluation strategies: one-shot and zero-shot. Specifically, the correlation and deviation between the predicted scores and actual performance and the relationship between architectural patterns and performance are analyzed in detail.

#### Utilizing Architecture-Performance Predictor
Architecture performance predictors provide another efficient method for large-scale analysis. Although such predictor-based analysis inevitably introduces predictor bias, it is also an efficient analysis method. Thanks to high-performance architecture encoders (e.g., GATES and TA-GATES), researchers can gain some understanding of the relationship between architecture and performance in a search space extremely fast and thoroughly.

For example, NASBOWL [Ru et al., ICLR 2021] combines Weisfeiler-Lehman kernels with Bayesian optimization to optimize the validation performance of the network and utilizes WL kernels for a useful interpretation of good/bad network structures by using the derivative of Gaussian process.

## Conclusion
This post builds on our research and shares our understanding of architecture modeling. Architecture modeling can empowers a lot of applications, from architecture search to architecture transformation and understanding, On the one hand, architecture modeling plays a crucial role for neural architecture search. On the other hand, looking at the entire field of deep learning, architecture modeling is also very important for researchers to open the black box of neural networks. All in all, we believe that architecture modeling is a very meaningful and valuable research direction.

Finally, we sincerely hope to further share mutual thoughts with interested readers. Welcome to contact us!

## References
[Liu et al., ICLR 2019] Liu, Hanxiao, Karen Simonyan, and Yiming Yang. <a href="https://arxiv.org/abs/1806.09055" class="no-underline" style="color:blue">DARTS: Differentiable Architecture Search.</a> International Conference on Learning Representations. 2019.

[Pham et al., ICML 2018] Pham, Hieu, et al. <a href="https://arxiv.org/abs/1802.03268" class="no-underline" style="color:blue">Efficient neural architecture search via parameters sharing.</a> International conference on machine learning. PMLR, 2018.

[Ying et al., ICML 2019] Chris Ying, Aaron Klein, Eric Christiansen, Esteban Real, Kevin Murphy, and Frank Hutter. <a href="https://arxiv.org/abs/1902.09635" class="no-underline" style="color:blue">Nas-bench-101: Towards reproducible neural architecture search.</a> ICML 2019.


[Dong et al., ICLR 2020] Xuanyi Dong and Yi Yang. <a href="https://arxiv.org/abs/2001.00326" class="no-underline" style="color:blue">Nas-bench-201: Extending the scope of reproducible neural architecture search.</a> ICLR 2020.

[Siems et al., 2020] Julien Siems, Lucas Zimmer, Arber Zela, Jovita Lukasik, Margret Keuper, and Frank Hutter. <a href="https://arxiv.org/abs/2008.09777" class="no-underline" style="color:blue">Nas-bench-301 and the case for surrogate benchmarks for neural architecture search.</a> 2020.


[Radosavovic et al., 2019] Radosavovic, I.; Johnson, J.; Xie, S.; Lo, W.-Y.; and Dollar, P. <a href="https://arxiv.org/abs/1905.13214" class="no-underline" style="color:blue">On network design spaces for visual recognition.</a> ICCV 2019.

[Ru et al., ICLR 2021] Ru B, Wan X, Dong X, et al. <a href="https://arxiv.org/abs/2006.07556" class="no-underline" style="color:blue">Interpretable neural architecture search via bayesian optimisation with weisfeiler-lehman kernels.</a> International Conference on Learning Representations. 2021.

</div>
