---
title: "TeViS: Translating Text Synopses to Video Storyboards"
date: 2023-04-23
featured: true
recruit: CVPR2024
thumbnail: https://ericblog.oss-cn-beijing.aliyuncs.com/img/tevis.png?Expires=1734006430&OSSAccessKeyId=TMP.3KjuRoXKWwbh3Zj5x2EC5zYSL3KBFPPqmUBZC2KWiLpkSXN1QTHsTAnAtC7eodbLiw2zaY6M2uy6z6atbp2XsJjQGSurzX&Signature=cwhczFnUXqzn0NVWJEEMGtrV1dY%3D
---
Xu Gu, Yuchong Sun, Feiyue Ni, Shizhe Chen, Xihua Wang, Ruihua Song, ***Boyuan Li***, Xiang Cao
<!-- more -->
A storyboard is a roadmap for video creation which consists of shot-by-shot images to visualize key plots in a text synopsis. Creating video storyboards however remains challenging which not only requires association between high-level texts and images, but also demands for long-term reasoning to make transitions smooth across shots. We propose a new task called Text synopsis to Video Storyboard (TeViS), which aims to retrieve an ordered sequence of images to visualize the text synopsis.
Previous works on text-to-image retrieval can only produce static images without considering the dynamics of shots in the video. Text-to-video works are able to retrieve or generate videos. Yet, most of them focus on short-term video clips with only a few seconds as shown in Fig. 1 (a). The images in these videos are highly redundant and cannot satisfy the requirement of a video storyboard for coherent keyframes. Previous works on visual storytelling works are proposed to visualize text with a sequence of images, but they care more about the text-image relevancy while omitting long-term reasoning to make transition smooth across keyframes (see Fig. 1 (b)). Moreover, the query texts in existing works are visually concrete and descriptive, making the models less generalizable to more abstract and high-level text synopses such as the synopsis in Fig. 1 (c).
![tevis](https://ericblog.oss-cn-beijing.aliyuncs.com/img/tevis.png)

