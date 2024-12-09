---
title: "Discovering Robust Convolutional Architecture at Targeted Capacity: A Multi-Shot Approach"

authors:
  - Xuefei Ning
  - admin
  - Wenshuo Li
  - Tianchen Zhao
  - Yin Zheng
  - Huazhong Yang
  - Yu Wang

# Author notes (optional)
author_notes:
  - 'Equal contribution'
  - 'Equal contribution'

share: false
pager: false

date: "2020-12-22T00:00:00Z"
doi: "https://doi.org/10.48550/arXiv.2012.11835"

# Schedule page publish date (NOT publication's date).
publishDate: "2023-12-10T00:00:00Z"

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ["article"]

# Publication name and optional abbreviated publication name.
publication: ""
publication_short: ""

abstract: Convolutional neural networks (CNNs) are vulnerable to adversarial examples, and studies show that increasing the model capacity of an architecture topology (e.g., width expansion) can bring consistent robustness improvements. This reveals a clear robustness-efficiency trade-off that should be considered in architecture design. In this paper, considering scenarios with capacity budget, we aim to discover adversarially robust architecture at targeted capacities. Recent studies employed one-shot neural architecture search (NAS) to discover robust architectures. However, since the capacities of different topologies cannot be aligned in the search process, one-shot NAS methods favor topologies with larger capacities in the supernet. And the discovered topology might be suboptimal when augmented to the targeted capacity. We propose a novel multi-shot NAS method to address this issue and explicitly search for robust architectures at targeted capacities. At the targeted FLOPs of 2000M, the discovered MSRobNet-2000 outperforms the recent NAS-discovered architecture RobNet-large under various criteria by a large margin of 4%-7%. And at the targeted FLOPs of 1560M, MSRobNet-1560 surpasses another NAS-discovered architecture RobNet-free by 2.3% and 1.3% in the clean and PGD-7 accuracies, respectively. All codes are available at https://github.com/walkerning/aw_nas.

# Summary. An optional shortened abstract.
summary: Considering scenarios with capacity budget, we aim to discover adversarially robust architecture at targeted capacities. However, one-shot NAS methods favor topologies with larger capacities in the supernet. And the discovered topology might be suboptimal when augmented to the targeted capacity. We propose a novel multi-shot NAS method to address this issue and explicitly search for robust architectures at targeted capacities.

tags:
- AutoML
- NAS
- Adversarial Robustness
- AI Safety

featured: true

url_pdf: 'https://arxiv.org/pdf/2012.11835.pdf'
url_code: 'https://github.com/walkerning/aw_nas'

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
image:
  caption: 'The overall workflow.'
  focal_point: ""
  preview_only: false

---
