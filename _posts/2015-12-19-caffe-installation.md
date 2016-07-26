---
layout: post
title: caffe-installation
category: DL
comments: false
---


**Caffe是一个高效的深度学习框架。它既可以在CPU上执行也可以在GPU上执行。**

下面介绍在Ubuntu上的配置编译过程：

1 安装BLAS：  
$ sudo apt-get install libatlas-base-dev

2 安装依赖项：  
$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libboost-all-dev libhdf5-serial-dev protobuf-compiler liblmdb-dev libgflags-dev libgoogle-glog-dev

3 下载Caffe：  
$ git clone git://github.com/BVLC/caffe.git

4 编译Caffe：  
(1)、$ cp Makefile.config.example Makefile.config   
(2)、修改Makefile.config文件：例如，如果只用cpu模式，去掉注释，使CPU_ONLY:= 1；再如，如果Python是Anaconda，则要修改和Python路径相关的语句   
(3)、$ make all   
(4)、$ make test   
(5)、$ make runtest  
5 编译Python的caffe包：  
(1)、根据官网把Python的依赖安装好
(2)、$ make pycaffe
(3)、在~/.profile里面添加环境变量：export PYTHONPATH=$PYTHONPATH:/path_to_ca/caffe_master/python


**说明**  
编译带CUDA支持的Caffe与上面的步骤完全一致，只要把Makefile.config 的 CPU_ONLY:=1注释掉即可。
此外，在服务器上安装时，需要把Makefile.config里面的opencv3语句取消注释
