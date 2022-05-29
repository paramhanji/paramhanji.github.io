---
title: "Comparison of single image HDR reconstruction methods — the caveats of quality assessment
"
collection: publications
date: 2022-07-24
permalink: /publication/2022-07-24-sihdr-benchmark
venue: 'ACM SIGGRAPH Conference Proceedings'
paperurl: 'https://www.cl.cam.ac.uk/research/rainbow/projects/sihdr_benchmark/2022-sihdr-benchmark-raw.pdf'
citation: '<strong>Param Hanji</strong>, Rafał K. Mantiuk., Gabriel Eilertsen, Saghi Hajisharif, and Jonas Unger &quot;Comparison of single image HDR reconstruction methods — the caveats of quality assessment&quot;. In <i>ACM SIGGRAPH Conference Proceedings</i>. 2022.'
---

As the problem of reconstructing high dynamic range (HDR) images from a single exposure has attracted much research effort, it is essential to provide a robust protocol and clear guidelines on how to evaluate and compare new methods. In this work, we compared six recent single image HDR reconstruction (SI-HDR) methods in a subjective image quality experiment on an HDR display. We found that only two methods produced results that are, on average, more preferred than the unprocessed single exposure images. When the same methods are evaluated using image quality metrics, as typically done in papers, the metric predictions correlate poorly with subjective quality scores. The main reason is a significant tone and color difference between the reference and reconstructed HDR images. To improve the predictions of image quality metrics, we propose correcting for the inaccuracies of the estimated camera response curve before computing quality values. We further analyze the sources of prediction noise when evaluating SI-HDR methods and demonstrate that existing metrics can reliably predict only large quality differences. 

Please view the [project page](https://www.cl.cam.ac.uk/research/rainbow/projects/sihdr_benchmark/) for more information.
