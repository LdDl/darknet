![Darknet Logo](http://pjreddie.com/media/files/darknet-black-small.png)

# Darknet #
Darknet is an open source neural network framework written in C and CUDA. It is fast, easy to install, and supports CPU and GPU computation.

For more information see the [Darknet project website](http://pjreddie.com/darknet).

For questions or issues please use the [Google Group](https://groups.google.com/forum/#!forum/darknet).

# Table of Contents

- [About this fork](#about)
- [Installation](#installation)
- [Usage](#usage)
- [How to train custom detector](#train)

# About

### Features of this fork:

1. Makefile contains rules for install and uninstall.
2. image.c contains functions for loading image from memory (usefull when you want to feed images to YOLO Darknet from existing project or make bindings for other programming language): see https://github.com/LdDl/darknet/blob/master/src/image.c#L1358
3. Added tutorial and annotation tool for traning.

# Installation

### Disclaimer:
This tutorial targeted for Linux distributions. I haven't tested it on Windows or MacOS.

### Getting source code

```cmd
git clone https://github.com/LdDl/darknet.git
```
### Prerequisites (OPTIONAL)

If you want to use your GPU, you have to install CUDA on your system first:
https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html

For cuDNN:
https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html

For OpenCV:
I prefer to not use OpenCV with this library (due some runtime errors for different version of OpenCV). But if you want to (for latest version):
https://docs.opencv.org/trunk/d7/d9f/tutorial_linux_install.html

For OpenMP:
Check if your compilier supports OpenMP:
```
echo |cpp -fopenmp -dM |grep -i open
```
If output is something like this
```c
#define _OPENMP 201511
```
then you good to go with OpenMP. Otherwise you need proper compilier.


### Edit Makefile

Actually you need to edit Makefile (for building) and /include/darknet.h (for using as external library) files.

In Makefile (my settings):
```
GPU=1 # For using GPU (nVidia) https://developer.nvidia.com/cuda-toolkit
CUDNN=1 # For using cuDNN (nVidia) https://developer.nvidia.com/cudnn
OPENCV=0 # For using with OpenCV https://github.com/opencv/opencv
OPENMP=0 # For using with OpenMP https://www.openmp.org/
DEBUG=0 # For debuging mode
```
In /include/darknet.h (my defenition). If you have '1' for some parameter in Makefile then you need the same definition in darknet.h file. '0' value can be ignored.
```c
#define GPU = 1
#define CUDNN = 1
```
### Building

From source folder (where Makefile is located)
```
make -j20
```
where '-j20' says how many cores for building you want to use. You can just ignore this flag.
After this libdarknet.a (static library), libdarknet.so (dynamic library) and ./darknet (executable) files will be generated.
libdarknet.so can be used in your projects.

### Installing
To install library:
```
sudo make install
```
This command copies /include/darknet.h file in /usr/local/include and /usr/include folders of your filesystem. Also it copies generated libdarknet.a file in /usr/local/lib and /usr/lib.

### Uninstall
If you want remove library from your system:
```
sudo make uninstall
```
# Usage

1. Go to /trained_nets/yolov3 or /trained_nets/yolov3-tiny folder.
2. Download weights file and put it inside of picked folder

  For yolov3:

  https://pjreddie.com/media/files/yolov3.weights

  For yolov3-tiny:

  https://pjreddie.com/media/files/yolov3-tiny.weights
  
3. Go back to root repository folder and run from terminal:

  For yolov3:
  ```
  ./darknet detect trained_nets/yolov3/yolov3.cfg trained_nets/yolov3/yolov3.weights images/dog.jpg
  ```
  Output for yolov3:
  ```
  Loading weights from trained_nets/yolov3/yolov3.weights...Done!
  images/dog.jpg: Predicted in 0.016514 seconds.
  dog: 100%
  truck: 81%
  bicycle: 100%
  ```
  For yolov3-tiny:
  ```
  ./darknet detect trained_nets/yolov3_tiny/yolov3-tiny.cfg trained_nets/yolov3_tiny/yolov3-tiny.weights images/dog.jpg
  ```
  Output for yolov3-tiny:
  ```
  Loading weights from trained_nets/yolov3_tiny/yolov3-tiny.weights...Done!
  images/dog.jpg: Predicted in 0.004073 seconds.
  dog: 57%
  car: 52%
  truck: 56%
  car: 62%
  bicycle: 59%
  ```
  
  Besides terminal's output there will be generated predicitons.jpg file with highlighted objects:
  ![-](https://raw.githubusercontent.com/LdDl/darknet/master/predictions_github.jpg)
  
  
# Train

In process
