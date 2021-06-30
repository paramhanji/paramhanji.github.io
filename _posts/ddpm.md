---
title: 'Denoising diffusion probabilistic models'
date: 2021-06-30
permalink: /posts/2016/06/ddpm/
tags:
  - diffusion models
  - evidence lower bound
  - variational methods
---

I wrote this to improve my understanding of a new class of deep generative models called denoising diffusion probabilistic models (DDPM). I read about them while preparing to present to my colleagues at our weekly [Rainbow ML reading group](https://www.cl.cam.ac.uk/research/rainbow/readingclub/). I spent way more time than I normally would since the procedure described and derivations were not at all trivial. Moreover, a detailed explanation can only be found in a few fairly involved papers. So I hope this post will provide a general understanding to a wider audience.

State-of-the-art generative modeling
------
The majority of deep generative models proposed in the last few years have broadly fallen under three categories---generative adversarial networks (GANs), variational autoencoders (VAEs), and normalizing flows. I have excluded a few architectures such as autoregressive ones and those based on transformers since they are much slower to sample from, especially when the distribution being modeled is high-dimensional.

In the first half of 2021, a few papers popped up on arXiv claiming that a new method, motivated by non-equilibrium statistical physics{% cite sohl2015deep %}, achieved sample qualities better than contemporary state-of-the-art models. Here are some teasers from these papers,

| ![imagenet](/files/images/ddpm/imagenet.png) | ![pointcloud](/files/images/ddpm/pointcloud.png) | 
| :------------: | :----------------: |
| *ImageNet 256{% cite dhariwal2021diffusion %}* | *ShapeNet dataset{% luo2021diffusion %}* |


High-level overview
------
Given a data sample $$\boldsymbol{x_0}$$, DDPM attempts to model the data distribution by introducing $$T$$ latents denoted $$\boldsymbol{x}_1, \boldsymbol{x}_2, ..., \boldsymbol{x}_T$$, using a model parameterized by $$\theta$$,

$$ p_\theta(\boldsymbol{x}) = \int p_\theta(\boldsymbol{x}_{0:T})\,d\boldsymbol{x}_{1:T} $$

Using a Markov chain, with the following probabilistic graphical model (PGM), we can easily factorize the joint distribution.

![ddpm_pgm](/files/images/ddpm/pgm.png)
*Graphical model for diffusion methods. The convention is to represent latent variables with white circles and observed variables with grey shaded circles.*

Although the tool employed for optimization is variational inference, the manner it is used is confusing at first glance. As a reminder, here is what is typically done in variational inference.

A primer on variational inference
------
The forward model is often quite simple, consisting of a single latent and a single observed variable. Again, here is the graphical model,

![vi_pgm](/files/images/ddpm/vi_pgm.png)
*We stick to the same conventions and use white circles for latent and grey circles for observed variables*

The posterior $$p(\boldsymbol{z}|\boldsymbol{x})$$ is in general intractable and we resort to using an approximation $$q_\phi(\boldsymbol{z}|\boldsymbol{x})$$, parameterized by $$\phi$$. We optimize $$\phi$$ by minimizing the KL-divergence between our approximate distribution and the true posterior $$D_\textrm{KL}(q_\phi(\boldsymbol{z}|\boldsymbol{x})\,||\,p(\boldsymbol{z}|\boldsymbol{x}))$$. This is equivalent to the maximizing the evidence lower-bound (ELBO) given by,

$$ \textrm{ELBO} = \mathbb{E}_{\boldsymbol{z} \sim q_\phi(\boldsymbol{z}|\boldsymbol{x})} \left[\log{p(\boldsymbol{x}|\boldsymbol{z})} - D_\textrm{KL}(q_\phi(\boldsymbol{z}|\boldsymbol{x})\,||\,p(\boldsymbol{z}))\right]\,.$$

The analogy I like to work with is that of trying to guess the mental state $$\boldsymbol{z}$$ of a cat (or a dog if you prefer) based solely on its observed behavior $$\boldsymbol{x}$$. None of us have any experience of being a cat and thus, have no priors about its mental states given its behavior $$p(\boldsymbol{z}|\boldsymbol{x})$$. Instead, we anthropomorphize our furry friends and try to assign human-like mental states $$q(\boldsymbol{z}|\boldsymbol{x})$$ because we know how to model these. The only thing left to do is assign a mapping between certain mental states and certain observed behaviors, which is roughly equivalent to learning model parameters from data.

Forward diffusion process
------
The DDPM papers flip the "direction", to better motivate the diffusion process. This is the picture you will see in most papers:

![ddpm][/files/images/ddpm/ddpm.png]
*The forward and reverse diffusion processes*

Here the forward or *diffusion* process refers to extracting latents $$\boldsymbol{x}_1, \boldsymbol{x}_2, ..., \boldsymbol{x}_T$$ from a given data sample $$\boldsymbol{x}_0 \sim q(\boldsymbol{x}_0)$$. This is a well-defined process defined by

$$ q(\boldsymbol{x}_t | \boldsymbol{x}_{t-1}) = \mathcal{N}(\boldsymbol{x}_t;\, \sqrt{1 - \beta_t}\boldsymbol{x}_{t-1}, \beta_t \mathbf{I}), $$

where $$\beta_t$$ is given by a pre-determined variance schedule. After an infinite number of such diffusion steps, the final latent follows a standard Gaussian distribution $$\boldsymbol{x}_T \sim \mathcal{N}(0, \mathbf{I}})$$, a distribution we know how to efficiently sample from. In practice, it has been shown that between 1000 and 4000 steps are sufficient.

Reverse diffusion process
------
The reverse process of generating a new sample is given by the posterior $$q(\boldsymbol{x}_{t-1} | \boldsymbol{x}_t, \boldsymbol{x}_0)$$ which actually requires knowledge of the original data distribution. To understand why, it helps to pay attention to a single diffusion step in isolation. The forward process involves takes as input a slightly noisy image, and adding a small amount of additional noise to it. The reverse is thus the process of inverting this operation---estimating and removing the noise added in this diffusion step, a significantly harder problem. To successfully reduce noise in an image, the model used requires a prior understanding of what noise-free image looks like. Obviously this is not available, since $$q(\boldsymbol{x}_0)$$ is precisely what we are interested in obtaining.

The choice of the forward diffusion operation, however, ensures that the posterior of each step is Gaussion, enabling us to easily approximate it. The Gaussian has only two parameters that need to be estimated using a model parameterized by $$\theta$$,

$$p_\theta(\boldsymbol{x}_{t-1}|\boldsymbol{x}_t) \sim \mathcal{N}(\boldsymbol{x}_t;\, \boldsymbol{\mu}_\theta(\boldsymbol{x}_t, t), \boldsymbol{\Sigma}_\theta(\boldsymbol{x}_t))$$

This description is slightly different from the typical variational inference description where an approximate model is used to obtain latents given the data. Here, the approximate model provides the data given the latents. The procedure is still the same---obtain a lower bound on the log-likelihood and update parameters of a model to maximize it.

Evidence lower-bound
------
Generally, latent variable models approximate the posterior of the latents, $$p(\boldsymbol{x}_{1:T}|\boldsymbol{x}_0)$$ with a parameterized function. However, as we saw in the previous section, this approximation is predetermined when using diffusion methods. Instead, the inversion of this process, $$p_\theta(\boldsymbol{x}_0|\boldsymbol{x}_{1:T})$$ is learnt using the ELBO. Pay attention to the fact the parameterization is now on the generative probability while the "approximate" posterior probability is well-defined due to diffusion. To derive the ELBO, we start with the objective of minimizing the KL-divergence between the approximate and true posteriors,

$$\begin{aligned}
  D_{KL} (q(\boldsymbol{x}_{1:T} | \boldsymbol{x}_0) \, || \, p_\theta(\boldsymbol{x}_{1:T} | \boldsymbol{x}_0))
  &= \mathbb{E} _q \left[ \log \frac{q(\boldsymbol{x}_{1:T} | \boldsymbol{x}_0)}{p_\theta(\boldsymbol{x}_{1:T} | \boldsymbol{x}_0)} \right] \\
  &= \mathbb{E}_q \left[ \log \frac{p_\theta(\boldsymbol{x}_0)}{p_\theta(\boldsymbol{x}_{0:T})} \cdot q(\boldsymbol{x}_0|\boldsymbol{x}_{1:T}) \right] \\
  \implies \log p_\theta(\boldsymbol{x}_0) &= \underbrace{\mathbb{E}_q \left[ \log \frac{p_\theta(\boldsymbol{x}_{0:T})}{q(\boldsymbol{x}_{1:T} | \boldsymbol{x}_0)} \right]}_\textrm{ELBO} + \underbrace{\vphantom{\mathbb{E}_q \left[ \log \frac{p_\theta(\boldsymbol{x}_{0:T})}{q(\boldsymbol{x}_0|\boldsymbol{x}_{1:T})} \right]} D_{KL} (q(\boldsymbol{x}_{1:T} | \boldsymbol{x}_0) \, || \, p_\theta(\boldsymbol{x}_{1:T} | \boldsymbol{x}_0))}_\textrm{non-negative divergence}
\end{aligned}$$

where the final step is obtained by extracting the marginal log-likelihood (since it does not depend on $$q$$), flipping the fraction inside the log function, and rearranging the terms. The ELBO turns out to be a summation of several terms,

$$\begin{aligned}
  -\textrm{ELBO} &= -\mathbb{E}_q \left[ \log \frac{p(\boldsymbol{x}_T) p_\theta(\boldsymbol{x}_0 | \boldsymbol{x}_1) \prod_{t=2}^T 
p_\theta(\boldsymbol{x}_{t-1} | \boldsymbol{x}_t)}{q(\boldsymbol{x}_1 | \boldsymbol{x}_0) \prod_{t=2}^T 
q(\boldsymbol{x}_t | \boldsymbol{x}_{t-1}, \boldsymbol{x}_0)} \right] \\
  &= -\mathbb{E}_q \left[ \log \frac{(\boldsymbol{x}_T) p_\theta(\boldsymbol{x}_0 | \boldsymbol{x}_1)}{q(\boldsymbol{x}_1 | \boldsymbol{x}_0)} + \sum \limits_{t=2}^T \log \frac{p_\theta(\boldsymbol{x}_{t-1} | \boldsymbol{x}_t)}{q(\boldsymbol{x}_{t-1} | \boldsymbol{x}_t, \boldsymbol{x}_0)} \cdot \frac{q(\boldsymbol{x}_{t-1}, \boldsymbol{x}_0)}{q(\boldsymbol{x}_t, \boldsymbol{x}_0)} && \text{(Bayes' rule)} \right] \\
  &= -\mathbb{E}_q \left[ \log p_\theta(\boldsymbol{x}_0 | \boldsymbol{x}_1) + \sum \limits_{t=2}^T \log \frac{p_\theta(\boldsymbol{x}_{t-1} | \boldsymbol{x}_t)}{q(\boldsymbol{x}_{t-1} | \boldsymbol{x}_t, \boldsymbol{x}_0)} + \frac{p(\boldsymbol{x}_T)}{q(\boldsymbol{x}_T | \boldsymbol{x}_0)} \right] \\
  &= \underbrace{\vphantom{\sum \limits_{t=2}^T} -\mathbb{E}_q \left[ \log p_\theta(\boldsymbol{x}_0 | \boldsymbol{x}_1) \right]}_{L_0} + \underbrace{\sum \limits_{t=2}^T D_\textrm{KL}(q(\boldsymbol{x}_{t-1} | \boldsymbol{x}_t, \boldsymbol{x}_0) \, || \, p_\theta(\boldsymbol{x}_{t-1} | \boldsymbol{x}_t))}_{L_1, L_2, ..., L_{T-1}} + \underbrace{\vphantom{\sum \limits_{t=2}^T} D_\textrm{KL}(q(\boldsymbol{x}_T | \boldsymbol{x}_0) \, || \, p(\boldsymbol{x}_T))}_{L_T} \,.
\end{aligned}$$

The formulation of diffusion models means that each intermediate probability $$p_\theta(\boldsymbol{x}_t)$$ as well as $$q(\boldsymbol{x}_t)$$ is a Gaussian. The final loss $$L = \Sigma_{t=0}^TL_t$$ is thus a sum of many terms where all but one are KL-divergences between Gaussians. There are a few more caveats to optimizing this loss, but you will have to check out the papers for details.

Practical considerations
------
- Since each diffusion step just adds Gaussian noise according to predetermined schedule, an intermediate noisy image can be sampled directly from $$q(\boldsymbol{x}_t | \boldsymbol{x}_0)$$ since Gaussians are closed under multiplication. This vastly speeds up training.
- For generating a sample, initial papers recommended starting with randomly drawn sample and going through the entire reverse diffusion operation iteratively. However, recent works demonstrate that this is not essentially and good novel samples can be obtained with as few as 50 steps.

{% bibliography --cited %}
