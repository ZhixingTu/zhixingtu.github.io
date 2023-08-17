---
layout:     post
title:      "🤔Paper Reading on Deep Semantic Communications"
subtitle:   "👣Long way to go"
date:       2023-02-10 23:59:00
author:     "Shicong Liu"
header-img: "img/home-dark.jpg"
catalog: true
mathjax: true
comment: true
hide-in-nav: true
tags:
    - 学习
    - feed
---


# Comments on "Deep Semantic Communications[^1]"

I am recently working on some so-called "Semantic Communications", which from my personal understanding is to encode the data from a higher level (compared to current binarized streams). The decoder needs to be able to extract the "semantics" from the received data, which therefore contributes to the transmission with lower overheads.




In fact, we have been stucked in the level A[^2] communication for a long time. Since Shannon firstly proposed the idea of digital communications, people were busy seeking the coding methods according to the information theory, which is established based on binarized data stream. However, maybe we dont really need such bits to achieve the encoding procedure, as long as we tried to accomplish encoding from another perspective, i.e., the so-called "**semantic**". 



Promising as it may seem to be, currently we do not have a criterion to measure or to prove neither the optimality or the amount of information, which means that such kind of encoding methods can not be directed by any existing principles. Before related theories come out, researchers are now trying to use some learning-based methods, hoping that they can capture the keys of encoding by themselves.



Transmission tasks that can fully utilize semantic encoding can be categorized as (a) text (b) images and (c) video stream. Among all these kinds of payloads, text is the one I think the hardest. The information density of such payloads are extremely high, which makes it more difficult than image and video transmission. Since I have heard about the paper entitled `Deep Learning Enabled Semantic Communication Systems`, I am quite confident that related research is on the right direction. However, after many days' hard work on searching for the proper datasets and implementing several models, I found that <mark>none of these models can satisfy my requirements</mark>.



I have rechecked that paper immediately and get some of the clues as (anyway I would still like to thank the authors for their open-sourced codes[^3])

- The authors are standing on a very **tricky** perspective. Previous work on "semantics" mainly focused on the joint coding schemes, while authors here decided to do the whole tx/rx design by neural networks. For days of validation I have found the truth that joint coding schemes for texts can not provide a satisfying result, while taking the other parts into consideration may solve it. 

- The performance comparison is **not that convincing**. For one thing, I am somehow confident that the authors know little about transformer models. They do not show the recursive features of decoders in model figure, and mention nothing about the training details. Moreover, the authors have no QUANTIZATION setup in the codes, but the Reed-Solomon or other coding schemes are processing from binarized data.

- BERT models are not that suitable for revealing the similarities, even the most advance SimCSE and its variations. Take a very simple but re-implementable example:

  ```
  src: "The sun is rising in the moring."
  trg: "The sun is setting in the evening"
  
  BERT similarity: 0.96
  SimCSE similarity: 0.76
  ```

  Unfortunately, we dont seem to have a reasonable criterion to judge the semantic similarity.



I am also having some other issues with regard to this paper, but not as significant as the one above hence not presented. This post is mainly given to record my reading reactions and thinkings and have no offense purpose to the authors. Anyway, maybe most of the issues can be well-explained =).



---

# References


[^1]:[Deep Learning Enabled Semantic Communication Systems](10.1109/TSP.2021.3071210)

[^2]:[Recent Contributions to The Mathematical Theory of Communication](https://www.jstor.org/stable/42581364)

[^3]:[DeepSC repo](https://github.com/13274086/DeepSC)