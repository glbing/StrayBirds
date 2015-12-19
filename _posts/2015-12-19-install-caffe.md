---
layout: post
title: Install-Caffe
category: DL
comments: false
---


**Caffe是一个高效的深度学习框架。它既可以在CPU上执行也可以在GPU上执行。**

下面介绍在Ubuntu上不带CUDA的Caffe配置编译过程：

1 安装BLAS：$ sudo apt-get install libatlas-base-dev

2 安装依赖项：$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libboost-all-dev libhdf5-serial-dev protobuf-compiler liblmdb-dev libgflags-dev libgoogle-glog-dev

3 下载Caffe：$ git clone git://github.com/BVLC/caffe.git

4 安装Caffe：(1)、$ cp Makefile.config.example Makefile.config  (2)、修改Makefile.config文件：去掉注释, CPU_ONLY:= 1 (3)、$ make all (4)、$ make test (5)、$ make runtest



**说明** 编译带CUDA支持的Caffe与上面的步骤完全一致，只要把CPU_ONLY:=1注释掉即可。
