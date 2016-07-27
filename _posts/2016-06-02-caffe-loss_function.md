---
layout: post
title: caffe loss function
category: DL
comments: false
---
##### 结合caffe的实现分析一下常用得loss function

![sunshi](https://raw.githubusercontent.com/glbing/blogs/gh-pages/images/sunshi.png)   
###### 欧式距离损失函数（Euclidean Loss）
输入：  
预测的值： y^∈[−∞,+∞] , shape为：N×C×H×W,N为batch的大小   
标签的值： y∈[−∞,+∞] , shape为：N×C×H×W   
输出：  
Loss=(1/2N)∑ ∥y^n −yn ∥2，shape为:1x1x1x1  

在caffe中，以softmax为例：  
在Train阶段，后面接softmax_loss_layer，在forword时，计算loss(由prob_和定义的loss_function),在backword时，计算梯度，并将梯度存储在bottom[0].diff_中  
在Test阶段，后面接soft_layer，在forword时，计算概率prob_
