---
title: "Robust estimation of exposure ratios in multi-exposure image stacks"
collection: publications
date: 2023-07-31
permalink: /publication/2023-07-31-exposure-estimation
venue: 'IEEE Transactions on Computational Imaging (TCI)'
# paperurl: 'https://arxiv.org/abs/2209.15165'
# citation: 'Mustafa, Aamir, <strong>Param Hanji</strong>, and Rafał K. Mantiuk. &quot;Distilling Style from Image Pairs for Global Forward and Inverse Tone Mapping.&quot; arXiv preprint arXiv:2209.15165 (2022).'
---

Merging multi-exposure image stacks into a high dynamic range (HDR) image requires knowledge of accurate exposure times. When exposure times are inaccurate, for example, when they are extracted from a camera's EXIF metadata, the reconstructed HDR images reveal banding artifacts at smooth gradients. To remedy this, we propose to estimate exposure ratios directly from the input images. We derive the exposure time estimation as an optimization problem, in which pixels are selected from pairs of exposures to minimize estimation error caused by camera noise. When pixel values are represented in the logarithmic domain, the problem can be solved efficiently using a linear solver. We demonstrate that the estimation can be easily made robust to pixel misalignment caused by camera or object motion by collecting pixels from multiple spatial tiles. The proposed automatic exposure estimation and alignment eliminates banding artifacts in popular datasets and is essential for applications that require physically accurate reconstructions, such as measuring the modulation transfer function of a display. The code for the method is available.

**Stay tuned for the project page containing the full paper.**
