---
layout: post
title: A Survey of Autonomous Driving Common Practices and Emerging Technologies
date: 2022-11-10
description: This paper discusses unsolved problems and surveys the technical aspect of automated driving. Studies regarding present challenges, high-level system architectures, emerging methodologies and core functions including localization, mapping, perception, planning, and human machine interfaces, were thoroughly reviewed
tag: Autonomous Vehicle
---   

# Basic Information

**Authors**
EKIM YURTSEVER, JACOB LAMBERT, ALEXANDER CARBALLO, AND KAZUYA TAKEDA

**Organization**
Graduate School of Informatics, Nagoya University, Nagoya 464-8603, Japan
Tier IV Inc., Nagoya 113-0033, Japan

**Source**
IEEE access journal

# Content 
## Intorduction 
According to a recent technical report by the National Highway Traffic Safety Administration (NHTSA), 94% of road accidents are caused by human errors. Against this backdrop, Automated Driving Systems (ADSs) are being developed with the promise of preventing accidents, reducing emissions, transporting the mobility-impaired and reducing driving related stress. If widespread deployment can be realized, annual social benefits of ADSs are projected to reach nearly $800 billion by 2050 through congestion mitigation, road casualty reduction, decreased energy consumption and increased productivity caused by the reallocation of driving time.

The Society of Automotive Engineers (SAE) refers to hardware-software systems that can execute dynamic driving tasks (DDT) on a sustainable basis as ADS

## Prospects and challenges
### Social impact 
1. Solving problems: preventing traffic accidents, mitigating traffic congestions, reducing emissions
2. Arising opportunities: reallocation of driving time, transporting the mobility impaired
3. New trends: consuming Mobility as a Service (MaaS), logistics revolution

### Challenges
ADSs are complicated robotic systems that operate in indeterministic environments. As such, there are myriad scenarios with unsolved issues.

The Society of Automotive Engineers (SAE) defined five levels of driving automation. 

1. __Level zero__ stands for no automation at all.
2. __Level one__: Primitive driver assistance systems such as adaptive cruise control, anti-lock braking systems and stability control.
3. __level two__: partial automation to which advanced assistance systems such as emergency braking or collision avoidance are integrated. The real challenge starts above this level.
4. __Level three__ is conditional automation; the driver could focus on tasks other than driving during normal operation, however, s/he has to quickly respond to an emergency alert from the vehicle and be ready to take over. In addition, level three ADS operate only in limited operational design domains (ODDs) such as highways
5. __Level four__: Human attention is not needed in any degree at level four and five. However, level four can only operate in limited ODDs where special infrastructure or detailed maps exist.

Level four and above driving automation in urban road networks is an open and challenging problem.


## System components and architecture
<p align = "center">
<img src = "/images/posts/SurveyofAuto/overviewofarchitecture.png">
</p>
<p align = "center">
Figure 1. A high level classification of automated driving system architectures.
</p>

ADSs are designed either as standalone, ego-only systems or connected multi-agent systems. Furthermore, these design philosophies are realized with two alternative approaches: modular or endto-end driving.


### Ego-only systems
The ego-only approach is to carry all of the necessary automated driving operations on a single self-sufficient vehicle at all times, whereas a connected ADS may or may not depend on other vehicles and infrastructure elements given the situation.


### modular systems
Modular systems, referred as the mediated approach in some works, are structured as a pipeline of separate components linking sensory inputs to actuator outputs. Core functions of a modular ADS can be summarized as: localization and mapping, perception, assessment, planning and decision making, vehicle control, and human-machine interface.

The major disadvantages of modular systems are being prone to error propagation and over-complexity. For example, in the unfortunate Tesla accident, an error in the perception module in the form of a misclassification of a white trailer as sky, propagated down the pipeline until failure, causing the first ADS related fatality

### End-to-end driving
End-to-end driving, referred as direct perception in some studies, generate ego-motion directly from sensory inputs.There are three main approaches for end-to-end driving: d**irect supervised deep learning**, **neuroevolution** and the more recent **deep reinforcement learning**.

<p align = "center">
<img src = "/images/posts/SurveyofAuto/informationFlowDiagrams.png">
</p>
<p align = "center">
Figure 2. Information flow diagrams of: (a) a generic modular system, and (b) an end-to-end driving system.
</p>


| Related works | Learning/training strategy | Pros/cons |
|-------|--------|---------|
|  | Direct supervised deep learning | imitates the target data. Can be trained offline. Poor generalization performance |
|  | Deep reinforcement learning | Learning the optimum way of driving. Require online interaction. Urban driving has not been achieved yet |
|  | Neuroevolution | No backpropagation. Requires online interaction. Real world driving has not been achieved yet. |

<p align = "center">
Table 1. Common end-to-end driving approaches.
</p>


End-to-end driving is promising, however it has not been implemented in real-world urban scenes yet, except limited demonstrations. The biggest shortcomings of end-to-end driving in general are the lack of hard coded safety measures and interpretability

### Connected systems
V2X is a term that stands for ‘‘vehicle to everything.’’ From mobile devices of pedestrians to stationary sensors on a traffic light, an immense amount of data can be accessed by the vehicle with V2X

By sharing detailed information of the traffic network amongst peers, shortcomings of the ego-only platforms such as sensing range, blind spots, and computational limits may be eliminated.However, without reducing huge amounts of continuous driving data, sharing information between hundreds of thousand vehicles in a city could not become feasible. A semiotic framework that integrates different sources of information and converts raw sensor data into meaningful descriptions.

## localization and mapping

Localization is the task of finding ego-position relative to a reference frame in an environment, and it is fundamental to any mobile robot. There are three common approaches: Global Positioning System and Inertial Measurement Unit (GPS-IMU) fusion, Simultaneous Localization And Mapping (SLAM), and state-of-the-art a priori map-based localization. A comparison of localization methods is given in Table 2.

<p align = "center">
<img src = "/images/posts/SurveyofAuto/localizationTech.png">
</p>
<p align = "center">
Table 2. Localization techniques.
</p>

### GPS-IMU fusion
The main principle of GPS-IMU fusion is correcting accumulated errors of dead reckoning in intervals with absolute position readings. GPS-IMU systems by themselves cannot be used for vehicle localization as they do not meet the performance criteria. But they are used for initial pose estimation in tandem with lidar and other sensors in state-ofthe-art localization systems.

### Simultaneous Localization And Mapping
SLAM is the act of online map making and localizing the vehicle in it at the same time. However, due to the high computational requirements and environmental challenges, running SLAM algorithms outdoors, which is the operational domain of ADSs, is less efficient than localization with a pre-built map. 

SLAM techniques have one major advantage over a priori methods: they can work anywhere.

### A Priori Map-based Localization
The core idea of a priori map-based localization techniques is matching: localization is achieved through the comparison of online readings to the information on a detailed pre-built map and finding the location of the best possible match.

There are two different map-based approaches; landmark search and matching.

#### landmark search 
Landmark search is computationally less expensive in comparison to point cloud matching. It is a robust localization technique as long as a sufficient amount of landmarks exists

A road marking detection method using lidar and Monte Carlo Localization (MCL). In this method, road markers and curbs were matched to a 3D map to find the location of the vehicle.

This approach has one major disadvantage; landmark dependency makes the system prone to fail where landmark amount is insufficient.

#### Point cloud matching
The state-of-the-art localization systems use multi-modal point cloud matching based approaches.

#### 2D to 3D matching
Matching online 2D readings to a 3D a priori map is an emerging technology.This approach requires only a camera on the ADS equipped vehicle instead of the more expensive lidar. The a priori map still needs to be created with a lidar.


## Perception 
Perceiving the surrounding environment and extracting information which may be critical for safe navigation is a critical objective for ADS. A variety of tasks, using different sensing modalities, fall under the category of perception.

### Detection
#### Image-based object detection 
Object detection refers to identifying the location and size of objects of interest.

While state-of-the-art methods all rely on DCNNs, there currently exist a clear distinction between them:
1. Single stage detection frameworks use a single network to produce object detection locations and class prediction simultaneously
2. Region proposal detection frameworks use two distinct stages, where general regions of interest are first proposed, then categorized by separate classifier networks.

Region proposal methods are currently leading detection benchmarks, but at the cost requiring high computation power. Meanwhile, single stage detection algorithms tend to have fast inference time and low memory cost, which is well-suited for real-time driving automation. YOLO (You Only Look Once) is a popular single stage detector the full model operating at 45 FPS and a smaller model operating at 155 FPS for a small accuracy trade-off. Another widely used algorithm, even faster than YOLO, is the Single Shot Detector (SSD).However, region proposal networks (RPN) have proven to be unmatched in terms of object recognition and localization accuracy, and computational cost has improved greatly in recent years, which can replace single stage detection networks for ADS applications in the near future.

##### Omnidirectional and Event Camera-based Perception

