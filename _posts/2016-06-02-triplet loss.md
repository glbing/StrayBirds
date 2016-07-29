---
layout: post
title: triplet loss
category: DL
comments: false
---
#####  triplet

triplet是一个三元组，这个三元组是这样构成的：从训练数据集中随机选一个样本，该样本称为I，然后再随机选取一个和I属于同一类的样本I+和不同类的样本I-,这两个样本对应的称为Positive (记为I_p)和Negative (记为I_n)，由此构成一个（I，I+，I-)三元组。   

#####  triplet loss
针对三元组中的每个元素（样本），训练一个参数共享或者不共享的网络，得到三个元素的特征表达，分别记为：**f(I)、f(I+)、f(I-)**   
triplet loss的目的就是通过学习，调整网络参数，让I和I+特征表达之间的距离尽可能I小，而I和I-的特征表达之间的距离尽可能大，并且要让I与I+之间的距离和I与I-之间的距离(L2范数)之间有一个最小的间隔a:  
**L2||f(I)-f(I+)||2+a << L2||f(I)-f(I-)||2**

所以，对应的 loss function 为：  
loss=**∑(1,N)max(0, L2||f(I)-f(I+)||2 - L2||f(I)-f(I-)||2 +a)**  
N为batch size；也就是说，当I和I-之间的距离小于I和I+之间的距离加a时，loss的值大于零，就会产生损失。当I和I-之间的距离 大于等于I与I+之间的距离加a时，loss为零。convex凸   

#####  triplet loss梯度推导
分别对f(I)、f(I+)、f(I-)求导即可



#####  在caffe中添加triplet_loss_layer
[参考](http://blog.csdn.net/tangwei2014/article/details/46815231)



————————————————————————————————————————————————————————————
自己在写triplet_loss_layer的时候，在google triplet loss的时候，发现github上已经有人实现了triplet_loss_layer:  
1.针对triplet_loss_layer对image数据层的讨论：[链接](https://github.com/BVLC/caffe/issues/2982)  
2.github上关于triplet_loss_layer的相关实现：[链接](https://github.com/BVLC/caffe/commit/954689e4980b06a53c210ba27d0a3b0f22ea3d06)    [github](https://github.com/BVLC/caffe/tree/954689e4980b06a53c210ba27d0a3b0f22ea3d06)
