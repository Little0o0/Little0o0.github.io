---
layout: post
title: Support Vector Machines (SVM)
date: 2020-08-14
categories: blog
tags: [Mechine Learning,Deep Learning,SVM]
description: the introduction for Support Vector Machines (SVM).
---

A Support Vector Machines (SVM) is usually used for classification problems. In the case of support-vector machines, a data point is viewed as a _p_-dimensional vector (a list of _p_ numbers), and we want to know whether we can separate such points with a _(p-1)_-dimensional hyperplane.

However, there are many hyperplanes which can classify the data. So we choose the hyperplane so that the distance from it to the nearest data point on each side is maximized. This hyperplane is called _maximum-margin hyperplane_. 

![maximum-margin-hyperplane center 100x100](https://monkeylearn.com/blog/wp-content/uploads/2017/06/plot_hyperplanes_annotated.png )

The Support Vector Machines is used to find the _maximum-margin hyperplane_. And it concludes Linear SVM and Nonlinear classification.

### Linear SVM
We are given a training dataset of n points:
![](http://latex.codecogs.com/gif.latex?\(x_1,y_1\),\dots,\(x_n,y_n\))






