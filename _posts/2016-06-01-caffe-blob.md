---
layout: post
title: caffe-blob
category: DL
comments: false
---
######  

blob的主要数据成员：data_，diff_，shape_

data_：指向SyncedMemory的智能指针，SyncedMemory是负责内存操作的类，该数据成员表示前向数据  

diff_：指向SyncedMemory的智能指针，SyncedMemory是负责内存操作的类，该数据成员表示后向数据（梯度）

shape_：vertor<int>,blob由于看作为存储数据的4维数组，所以shape_表示4维数组的d的大小，如下：   
对于数据:num(输入数据量,比如sgd时,mini-batch的大小),channels(通道数量),height(图 片的高度),width(图片的宽度)   
对于卷积权重:output*input*height*width  
对于卷积偏置:output*1*1*1   
对于卷积层输出:输入图片数量*对应feature maps数量*输出图片的高度*输出图片的宽度  
此外，Blob是row-major保存的,因此在(n, k, h, w)位置的值物理位置为((n x K + k) x H + h) x  W + w
