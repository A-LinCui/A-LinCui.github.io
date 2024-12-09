---
title: 'Dynamic Ensemble of Low-Fidelity Experts: Mitigating NAS "Cold-Start"'

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - admin
  - Xuefei Ning
  - Enshu Liu
  - Binxin Ru
  - Zixuan Zhou
  - Tianchen Zhao
  - Chen Chen
  - Jiajin Zhang
  - Qingmin Liao
  - Yu Wang

# Author notes (optional)
author_notes:
  - 'Equal contribution'
  - 'Equal contribution'

share: false
pager: false

date: '2023-06-26T00:00:00Z'
doi: 'https://doi.org/10.1609/aaai.v37i9.26339'

# Schedule page publish date (NOT publication's date).
publishDate: '2023-12-10T00:00:00Z'

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ['paper-conference']

# Publication name and optional abbreviated publication name.
publication: In *The 38th Annual AAAI Conference on Artificial Intelligence*
publication_short: In *AAAI 2023*

abstract: Predictor-based Neural Architecture Search (NAS) employs an architecture performance predictor to improve the sample efficiency. However, predictor-based NAS suffers from the severe ``cold-start'' problem, since a large amount of architecture-performance data is required to get a working predictor. In this paper, we focus on exploiting information in cheaper-to-obtain performance estimations (i.e., low-fidelity information) to mitigate the large data requirements of predictor training. Despite the intuitiveness of this idea, we observe that using inappropriate low-fidelity information even damages the prediction ability and different search spaces have different preferences for low-fidelity information types. To solve the problem and better fuse beneficial information provided by different types of low-fidelity information, we propose a novel dynamic ensemble predictor framework that comprises two steps. In the first step, we train different sub-predictors on different types of available low-fidelity information to extract beneficial knowledge as low-fidelity experts. In the second step, we learn a gating network to dynamically output a set of weighting coefficients conditioned on each input neural architecture, which will be used to combine the predictions of different low-fidelity experts in a weighted sum. The overall predictor is optimized on a small set of actual architecture-performance data to fuse the knowledge from different low-fidelity experts to make the final prediction. We conduct extensive experiments across five search spaces with different architecture encoders under various experimental settings. For example, our methods can improve the Kendall's Tau correlation coefficient between actual performance and predicted scores from 0.2549 to 0.7064 with only 25 actual architecture-performance data on NDS-ResNet. Our method can easily be incorporated into existing predictor-based NAS frameworks to discover better architectures. Our method will be implemented in Mindspore (Huawei 2020), and the example code is published at https://github.com/A-LinCui/DELE.

# Summary. An optional shortened abstract.
summary: Leverage the beneficial knowledge implicit in low-fidelity information (e.g., one/zero-shot estimation) to mitigate the "cold-start" problem of predictor-based NAS.

tags:
  - AutoML
  - NAS

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: 'https://arxiv.org/pdf/2302.00932.pdf'
url_code: 'https://github.com/A-LinCui/DELE'
url_poster: 'https://underline.io/events/380/sessions/14443/lecture/68309-dynamic-ensemble-of-low-fidelity-experts-mitigating-nas-cold-start?tab=Poster'
url_slides: 'https://drive.google.com/file/d/1Zsrk0Ux27Jh8-SnWUThgPvtHU9U7mXhU/view?usp=sharing'
url_video: 'https://www.bilibili.com/video/BV1cj411H7tt/?spm_id_from=333.337.search-card.all.click'

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  caption: 'Illustration of the proposed dynamic ensemble predictor framework.'
  focal_point: ''
  preview_only: false

---
