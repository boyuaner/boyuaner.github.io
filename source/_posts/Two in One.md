---
title: "Two-in-One: Unified Multi-Person Interactive Motion Generation by Latent Diffusion Transformer"
date: 2024-09-13
featured: true
recruit: UnderReview
thumbnail: https://ericblog.oss-cn-beijing.aliyuncs.com/img/twoinone.png
---
***Boyuan Li***, Xihua Wang, Ruihua Song and Wenbing Huang
<!-- more -->
## Abstract
Multi-person interactive motion generation, a critical yet under-explored domain in computer character animation, poses significant challenges such as intricate modeling of inter-human interactions beyond individual motions and generating two motions with huge differences from one text condition.
Current research often employs separate module branches for individual motions, leading to a loss of interaction information and increased computational demands. 
To address these challenges, we propose a novel, unified approach that models multi-person motions and their interactions within a single latent space.
Our approach streamlines the process by treating interactive motions as an integrated data point, utilizing a Variational AutoEncoder (VAE) for compression into a unified latent space, and performing a diffusion process within this space, guided by the natural language conditions.
Experimental results demonstrate our method's superiority over existing approaches in generation quality, performing text condition in particular when motions have significant asymmetry, and accelerating the generation efficiency while preserving high quality. 
![img](https://ericblog.oss-cn-beijing.aliyuncs.com/img/twoinone.png)