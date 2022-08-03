---
layout: archive
title: "Curriculum Vitae"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

If you prefer a concise résumé, please see find it [here](/files/param_hanji_resume.pdf).

Education
======
* PhD in Computer Vision, University of Cambridge, 2023 (expected). Funded by the ERC ERC Grant Horizon 2020: Project <a href="https://erc.europa.eu/projects-figures/erc-funded-projects/results?search_api_views_fulltext=eyecode" target="_blank">Eyecode</a>.
* B.Tech. in Information Technology, National Institute of Technology Karnataka, India, 2018.

Work experience
======
* Feb 2019 - Present: Research Assistant at University of Cambridge
  * Supervisor: [Dr Rafał Mantiuk](https://www.cl.cam.ac.uk/~rkm38/)
  * Working to capture and render images on a novel multi-focalplane, stereoscopic, high dynamic range display. We aim to produce images that will successfully pass the Visual Turing Test.
  * Responsible for reconstructing HDR images and estimating per-view depth maps. MLE-based estimators obtained by assuming a statistical camera noise model produced the best HDR images. Preliminary results were presented at an ECCV workshop, with a more elaborate journal version currently under review.
  * Also worked on optical flow, differentiable rendering, neural view synthesis

* Sept 2020 - Feb 2021: Part-time project with Huawei Research, Munich
  * Exploring the effect of different tone-curves (encoding functions) on the performance of state-of-the-art Computer Vision methods.
  * Conducted an evaluation of state-of-the-art face and object detectors as well as optical flow with the help of a new adversarial illumination dataset. The dataset and preliminary study were accepted to be published in the Journal of Imaging Science and Technology (JIST).

* July 2018 - Jan 2019: Research Assistant at National University of Singapore
  * Supervisor: [Prof. Gary Tan](https://www.comp.nus.edu.sg/cs/bio/gtan/)
  * Involved in the development of agent-based traffic simulator, at the Singapore-MIT Alliance for Research and Technology (SMART) Lab. Contributed to the first release of [SimMobility](https://github.com/smart-fm/simmobility-prod).
  * Worked to improve the runtime performance of an on-demand vehicle controller that handles shared taxi requests. On average, the simulation time was reduced to a third of the original.


Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

Supervisions
======
Please see [Teaching](/teaching/)
