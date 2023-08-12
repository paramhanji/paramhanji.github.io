---
title: "How to cheat with metrics in single-image HDR reconstruction
"
collection: publications
date: 2021-10-17
permalink: /publication/2021-10-17-si-hdr-cheat
venue: '2nd Learning for Computational Imaging (LCI) Workshop @ ICCV'
paperurl: 'https://arxiv.org/abs/2108.08713'
citation: 'Gabriel Eilertsen, Saghi Hajisharif, <strong>Param Hanji</strong> , Apostolia Tsirikoglou, Rafał K. Mantiuk and Jonas Unger. &quot;How to cheat with metrics in single-image HDR reconstruction&quot;. In <i>Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV) Workshops</i>. 2021.'
---

Single-image high dynamic range (SI-HDR) reconstruction has recently emerged as a problem well-suited for deep learning methods. Each successive technique demonstrates an improvement over existing methods by reporting higher image quality scores. This paper, however, highlights that such improvements in objective metrics do not necessarily translate to visually superior images. The first problem is the use of disparate evaluation conditions in terms of data and metric parameters, calling for a standardized protocol to make it possible to compare between papers. The second problem, which forms the main focus of this paper, is the inherent difficulty in evaluating SI-HDR reconstructions since certain aspects of the reconstruction problem dominate objective differences, thereby introducing a bias. Here, we reproduce a typical evaluation using existing as well as simulated SI-HDR methods to demonstrate how different aspects of the problem affect objective quality metrics. Surprisingly, we found that methods that do not even reconstruct HDR information can compete with state-of-the-art deep learning methods. We show how such results are not representative of the perceived quality and that SI-HDR reconstruction needs better evaluation protocols.
