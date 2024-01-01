---
layout: post
title: Learning for Vehicle-to-Vehicle Cooperative Perception under Lossy Communication
date: 2023-02-18
description: All the existing cooperative perception algorithms assume the ideal V2V communication without considering the possible lossy shared features because of the Lossy Communication (LC) which is common in the complex real-world driving scenarios. In this paper, we first study the side effect (e.g., detection performance drop) by the lossy communication in the V2V Cooperative Perception, and then we propose a novel intermediate LCaware feature fusion method to relieve the side effect of lossy communication by a LC-aware Repair Network (LCRN) and enhance the interaction between the ego vehicle and other vehicles by a specially designed V2V Attention Module (V2VAM) including intra-vehicle attention of ego vehicle and uncertaintyaware inter-vehicle attention.
tag: Autonomous Vehicle
---   

# Basic Information

**Authors**:
Jinlong Li, Runsheng Xu, Xinyu Liu, Jin Ma, Zicheng Chi, Jiaqi Ma, Hongkai Yu


**Organization**:
Cleveland State University, University of California, Los Angeles

**Source**:
Arxiv

**Source Code**:
None

**Cite**:
Li, Jinlong, et al. "Learning for Vehicle-to-Vehicle Cooperative Perception under Lossy Communication." arXiv preprint arXiv:2212.08273 (2022).

# Content 
## Motivation
Many intermediate fusion methods have been recently proposed for the V2V cooperative perception, however all of them assume the ideal communication. The only V2V cooperative perception work that does not assume the ideal communication just studied the communication delay. Lossy communication is common in the realworld V2V communication, which might introduce incomplete or inaccurate shared intermediate features during the V2V communication. Failing to handle lossy communication will make the V2V cooperative perception system vulnerable and inefficient, as shown in Fig. 1.
<p align = "center">
<img src = "/images/posts/LCRN/motivation.png" width="600">
</p>
<p align = "center">
Figure 1. Illustration of the V2V cooperative perception pipeline and its detection performance drop suffering from lossy communication on the public digital-twin CARLA simulator based OPV2V dataset, where three intermediate fusion methods all trained under ideal communication are displayed: CoBEVT, F-Cooper, and V2X-ViT.
</p>

## Evaluation
**Metric:** Average precision (AP) at IOU=0.5, 0.7. the evaluation range as $x \in [−140, 140]$ meters, $y\in [−40, 40]$ meters.

**Experiment details:** 1) Ideal Communication, where all data transmissions are under perfect communication; 2) Lossy Communication, where all intermediate features from other CAVs suffer from the lossy communication except the ego vehicle feature. To simulate the lossy communication, the shared intermediate features are randomly selected by a random probability $p \in [0, 1]$, then replaced by random noise, which is generated by a uniform distribution within the range of original intermediate features.

**Baseline:** No Fusion, F-Cooper, V2VNet, OPV2V, CoBEVT, V2X-ViT. 

## Datasets
OPV2V

## Result
<p align = "center">
<img src = "/images/posts/LCRN/result1.png" width="500">
</p>
<p align = "center">
Table 1. 3D detection performance comparison on two testing sets of OPV2V based on the training of scheme II.
</p>


## Method
This paper proposes a novel intermediate LCaware feature fusion framework. The overall architecture of the proposed framework is illustrated in Fig. 2, which includes five components: 1) V2V metadata sharing, 2) LIDAR feature extraction, 3) Feature sharing, 4) LC-aware repair network and V2V Attention module, 5) classification and regression headers.

<p align = "center">
<img src = "/images/posts/LCRN/framework.png" width="500">
</p>
<p align = "center">
Figure 2. The architecture of LC-aware feature fusion framework. The proposed model includes five components: 1) V2V metadata sharing. 2) LIDAR feature extraction. 3) Feature sharing, 4) LC-aware Repair Network and V2V Attention Module. 5) classification and regression header.
</p>

### LC-aware Repair Network
<p align = "center">
<img src = "/images/posts/LCRN/LC-aware Repair Network.png" width="500">
</p>
<p align = "center">
Figure 3. Illustration of LC-aware Repair Network. The LC-aware Repair architecture for feature recover is based on the encoder-decoder structure, which outputs per-tensor feature kernels. These kernels then are applied to the input lossy features.
</p>
The framework of the CL-aware repair network is shown in Fig. 3, which is an encoder-decoder architecture with skip connections. This network generates a specific per-tensor filter kernel to jointly align and recover the input damaged feature to produce a recovered version of the output feature. The input feature for CL-aware repair network is $S \in R^{c \times h \times w}$, then a tensor-wise kernel K is generated and applied to $S$ to produce the recovered output feature $\hat{S} \in R^{c \times h \times w}$. the specific tensorwise filter kernel could be simply formulated as

$$K = Conv(S),$$

and the value at each tensor t in our output feature $\hat S$ is

$$\hat S^t = K^t \circledast S^t$$

### V2V Attention Module
<p align = "center">
<img src = "/images/posts/LCRN/attention.png" width="700">
</p>
<p align = "center">
Figure 4. The architecture of V2V Attention Module including the intra-vehicle attention of ego vehicle and uncertainty-aware inter-vehicle attention. The final output is a fusion feature with interaction between ego feature and other shared featured from other CAVs.
</p>

# Comments
##  Pros
1. lossy communication is interesting.


## Cons
(need further experiment)

## Further work
(need further experiment)

## Comments
(need further experiment)

# References