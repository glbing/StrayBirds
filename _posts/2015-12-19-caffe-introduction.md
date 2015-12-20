---
layout: post
title: caffe-introduction
category: DL
comments: true
---

***
##### Caffe能做什么？

定义网络结构     
训练网络     
C++/CUDA 写的结构     
cmd/python/Matlab接口     
CPU/GPU工作模式      
给了一些参考模型&pretrain了的weights     
***
#### 为什么选择caffe?

模块化做的好  
简单：修改结构无需该代码  
开源：共同维护开源代码  
***
#### caffe整体结构：

定义CAFFE为caffe跟目录，caffe的核心代码都在$CAFFE/src/caffe 下，主要有以下部分：net, blob, layer, solver.

**net.cpp:**  
net定义网络， **整个网络中含有很多layers**， net.cpp负责计算整个网络在训练中的forward, backward过程， 即计算forward/backward 时各layer的gradient。


**layers:**   
在$CAFFE/src/caffe/layers中的层，在protobuffer (.proto文件中定义message类型，.prototxt或.binaryproto文件中定义message的值) 中调用时包含属性name， type（data/conv/pool…）， connection structure (input blobs and output blobs)，layer-specific parameters（如conv层的kernel大小）。定义一个layer需要定义其setup, forward 和backward过程。


**blob.cpp:**  
net中的数据和求导结果通过4维的blob传递。一个layer有很多blobs， e.g,  

. 对data，weight blob大小为Number\*Channels\*Height\*Width, 如256\*3\*224\*224；

. 对conv层，weight blob大小为 Output 节点数 x Input 节点数 x Height x Width，如AlexNet第一个conv层的blob大小为96 x 3 x 11 x 11；  

. 对inner product(全连接) 层， weight blob大小为 1\*1 \* Output节点数\*Input节点数； bias blob大小为1\*1\*1\* Output节点数（ conv层和inner product层一样，也有weight和bias（偏移），所以在网络结构定义中我们会看到两个blobs\_lr，第一个是weights的，第二个是bias的。类似地，weight\_decay也有两个，一个是weight的，一个是bias的）；


blob中，mutable\_cpu/gpu\_data() 和cpu/gpu\_data()用来管理memory，cpu/gpu_diff()和 mutable\_cpu/gpu\_diff()用来计算求导结果。

**slover.cpp:**   
结合loss，用gradient更新weights。主要函数：
Init(),  
Solve(),   
ComputeUpdateValue(),  
Snapshot(), Restore(),//（拷贝）与恢复 网络state  
Test()；

在solver.cpp中有3中solver，即3个类：AdaGradSolver, SGDSolver和NesterovSolver可供选择。


关于loss，可以同时有多个loss，可以加regularization（L1/L2）；
***
#### Protocol buffer：

上面已经将过， protocol buffer在 .proto文件中定义message类型，.prototxt或.binaryproto文件中定义message的值；  

Caffe的所有message定义在$CAFFE/src/caffe/proto/caffe.proto中。  

**在实验中，主要用到两个protocol buffer: solver的和model的，分别定义solver参数（学习率啥的）和model结构(网络结构)。**

技巧：
冻结第一层不参与训练：设置其blobs_lr=0
对于图像，读取数据尽量别用HDF5Layer（因为只能存float32和float64，不能用uint8, 所以太费空间）

如果对protobuf不熟悉，可参考
[该文](http://www.cnblogs.com/dkblog/archive/2012/03/27/2419010.html)，讲了protobuf的语法和工作机制。

***
#### caffe训练基本流程：

**1 数据处理**  
法一，转换成caffe接受的格式：lmdb, leveldb, hdf5 / .mat, list of images, etc.；法二，自己写数据读取层(如https://github.com/tnarihi/tnarihi-caffe-helper/blob/master/python/caffe\_helper/layers/data\_layers.py)  

**2 定义网络结构**   
主要编写model的prototxt

**3 配置Solver参数**  
主要是编写slover的prototxt

**4 训练**：  
如 caffe train -solver solver.prototxt -gpu 0


***
#### 在python中训练:
Document & Examples: https://github.com/BVLC/caffe/pull/1733

核心code：

$CAFFE/python/caffe/\_caffe.cpp   
定义Blob, Layer, Net, Solver类  
$CAFFE/python/caffe/pycaffe.py   
Net类的增强功能
***
#### Debug：

在Make.config中设置DEBUG := 1  
在solver.prototxt中设置debug_info: true  
在python/Matlab中察看forward & backward一轮后weights的变化  
