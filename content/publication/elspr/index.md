---
title: "Ensemble-Based Reliability Enhancement for Edge-Deployed CNNs in Few-Shot Scenarios"
authors:
- Zhen Gao
- Shuang Liu
- admin
- Xiaofei Wang
- Yu Wang
- Zhu Han

date: "2024-07-29T00:00:00Z"
doi: "10.1109/TMLCN.2024.3435168"

# Schedule page publish date (NOT publication's date).
publishDate: "2024-12-10T00:00:00Z"

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ["article-journal"]

# Publication name and optional abbreviated publication name.
publication: In *IEEE Transactions on Machine Learning in Communications and Networking*
publication_short: In *TMLCN 2024*

abstract: Convolutional Neural Networks (CNNs) have been applied in wide areas of computer vision, and edge intelligence is expected to provide instant AI service with the support of broadband mobile networks. However, the deployment of CNNs on network edge faces severe challenges. First, edge or embedded devices are usually not reliable, and hardware failures can corrupt the CNN system, which is unacceptable for critical applications, such as autonomous driving and object detection on space platforms. Second, edge or embedded devices are usually resource-limited, and therefore traditional redundancy-based protection methods are not applicable due to huge overhead. Although network pruning is effective to reduce the complexity of CNNs, we cannot have sufficient data for performance recovery in many scenarios due to privacy and security concerns. To enhance the reliability of CNNs on resource-limited devices with the few-shot constraint, we propose to construct an ensemble system with weak base CNNs pruned from the original strong CNN. To improve the ensemble performance with diverse base CNNs, we first propose a novel filter importance evaluation method by combining the amplitude and gradient information of the filter. Since the gradient part is related to the input data, different subsets of data are used for layer sensitivity analysis for different base CNNs, so that the different pruning configurations can be obtained for each base CNN. On this basis, a modified ReLU function is proposed to determine the final pruning rate of each layer in each base CNN. Extensive experiments prove that the proposed solution can effectively improve the reliability of CNNs with much less resource requirement for each edge server.

tags:
- Efficiency
- Pruning

featured: false

url_pdf: 'https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10614218'

---
