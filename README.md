# Tensorrt demos

The examples in this repo are from ([JK Jung blog](https://jkjung-avt.github.io/) and demonstrate how to optimize caffe/tensorflow/darknet models with TensorRT and run inferencing on NVIDIA Jetson nano.   

* Run an optimized "GoogLeNet" image classifier at ~60 FPS on Jetson Nano.
* Run a very accurate optimized "MTCNN" face detector at 6~11 FPS on Jetson Nano.
* Run an optimized "ssd_mobilenet_v1_coco" object detector ("trt_ssd_async.py") at 27~28 FPS on Jetson Nano.
* Run an optimized "yolov3-416" object detector at ~3 FPS on Jetson Nano.


Table of contents
-----------------

* [Prerequisite](#prerequisite)
* [Demo #1: GoogLeNet](#googlenet)
* [Demo #2: MTCNN](#mtcnn)
* [Demo #3: SSD](#ssd)
* [Demo #4: YOLOv3](#yolov3)

<a name="prerequisite"></a>
Prerequisite
------------

The code in this repository is tested on  Jetson Nano.  In order to run the demos below, first make sure you have the JetPack 4.4 installed on the Jetson nano system.  

## Hardware
- Jetson nano
- HDMA screen
- USB keyboard and mouse
- Wifi, complete kit
- Makeronics Developer Kit for Jetson Nano (Wifi included)
- SD card reader
- Logitech USB camera

## Step 1: Flash NVIDIA’s Jetson Nano Developer Kit .img to a microSD
NVIDIA JetPack 4.4  bundles most of the developer tools required on the Jetson platform, including system profiler, graphics debugger, and the CUDA Toolkit
- L4T R32.4.2
- CUDA 10.2
- cuDNN 8.0.0 (Developer Preview)
- TensorRT 7.1.0 (Developer Preview)
- VisionWorks 1.6
- OpenCV 4.1
- Vulkan 1.2
- VPI 0.2.0 (Developer Preview)
- Nsight Systems 2020.2
- Nsight Graphics 2020.1
- Nsight Compute 2019.3

Download Jetpack 4.4 from ([here]https://developer.nvidia.com/embedded/jetpack). I am using my mac and a microSD card reader. You will need to download and install BalenaEtcher a disk image flashing tool. Insert the microSD into the card reader, and then plug the card reader into a USB port on your computer. Start balenaEtcher and proceed to flash. Once flashing is completed, eject and you are ready to move to step 2.

## Step 2: Boot Jetson Nano with the microSD
Insert the microSD into the Jetson Nano, connect thescreen, keyboard, mouse. Apply power. Insert the power plug of your power adapter into your Jetson Nano (use the J48 jumper if using a 20W barrel plug supply). You will see the NVIDIA + Ubuntu 18.04 desktop, you should configure your wired or wireless network settings, langage and basic linux configuration including setting a password.

```shell
$ ls /usr/lib/aarch64-linux-gnu/libnvinfer.so*
/usr/lib/aarch64-linux-gnu/libnvinfer.so
/usr/lib/aarch64-linux-gnu/libnvinfer.so.5
/usr/lib/aarch64-linux-gnu/libnvinfer.so.5.1.6
```

Furthermore, the demo programs require "cv2" (OpenCV) module for python3.  You could use the "cv2" module which came in the JetPack.  Or, if you'd prefer building your own, refer to [Installing OpenCV 3.4.6 on Jetson Nano](https://jkjung-avt.github.io/opencv-on-nano/) for how to build from source and install opencv-3.4.6 on your Jetson system.

Lastly, if you plan to run Demo #3 (SSD), you'd also need to have "tensorflowi-1.x" installed.  You could probably use the [official tensorflow wheels provided by NVIDIA](https://docs.nvidia.com/deeplearning/frameworks/pdf/Install-TensorFlow-Jetson-Platform.pdf), or refer to [Building TensorFlow 1.12.2 on Jetson Nano](https://jkjung-avt.github.io/build-tensorflow-1.12.2/) for how to install tensorflow-1.12.2 on the Jetson system.



<a name="googlenet"></a>
Demo #1: GoogLeNet
------------------


Licenses
--------

I referenced source code of [NVIDIA/TensorRT](https://github.com/NVIDIA/TensorRT) samples to develop most of the demos in this repository.  Those NVIDIA samples are under [Apache License 2.0](https://github.com/NVIDIA/TensorRT/blob/master/LICENSE).
