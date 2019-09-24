# Steps to build TensorFlow r1.14.0 C-library on Jetson Nano for GPU

## Jetson Nano Developer Kit configuration
* ARM 64 (aarch64)
* gcc 7.4
* cuda 10
* cudnn 7
* ubuntu 18.04.1

## TensorFlow configuration
* v1.14.0


## Steps

### Build Bazel 0.24.1
* Install Requirements

```
sudo apt-get install -y pkg-config zip g++ zlib1g-dev unzip
sudo apt-get install -y openjdk-8-jdk
```

* wget https://github.com/bazelbuild/bazel/releases/download/0.24.1/bazel-0.24.1-dist.zip
* unzip bazel-0.24.1-dist.zip -d bazel-0.24.1-dist
* cd bazel-0.24.1-dist
* ./compile.sh
* export PATH=$PATH:$PWD/output


### Build Tensorflow 1.14.0

* Enable at least 8GB swap space refer to: https://devtalk.nvidia.com/default/topic/1041894/jetson-agx-xavier/creating-a-swap-file/
* sudo apt-get install python-pip
* pip install future
* git clone https://github.com/tensorflow/tensorflow.git
* cd tensorflow
* git checkout r1.14
* configure only enable cuda and choose config monolithic
* bazel build --config=monolithic --local_resources 2048,3.0,1.0 --config=cuda --config=noaws --config=nogcp --config=nonccl --jobs 4 //tensorflow:libtensorflow.so //tensorflow:libtensorflow_framework.so
* wait for more than 12 hours...
* After several hours you should see `libtensorflow.so` and `libtensorflow_framework.so` files in `bazel-bin` folder


## Reference
* https://gist.github.com/sdeoras/533708a95a3de5ff6de1c6b018cabf6d
* https://devtalk.nvidia.com/default/topic/1058253/jetson-nano/request-for-prebuilt-tensorflow-c-c-api-libs-for-jetson-nano/
