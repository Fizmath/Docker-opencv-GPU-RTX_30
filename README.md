

#  GPU-accelerated Docker container with OpenCV 4.6, Python 3.9, GStreamer and CUDA 11.7

- [opencv](https://github.com/opencv/opencv) + [opencv_contrib](https://github.com/opencv/opencv_contrib)
- Python 3.9.5
- Ubuntu  20.04 
- GStreamer  1.16.3
- FFMPEG
- CUDA  11.7.0
- NVIDIA GPU  Compute Capability:  sm_86 (  single arch  build ; the container wokrs with **NVIDIA Ampere GPUs** RTX 30 series.  For older GPUs see this [repo](https://github.com/Fizmath/Docker-opencv-GPU))
- cuDNN:  8.5.0
- OpenCL
- Qt5::OpenGL  5.12.8
- Intel IPP and TBB
- UNCOMPRESSED SIZE  8.32 GB


## How to build :

Unlike the [older repo](https://github.com/Fizmath/Docker-opencv-GPU) there is no pre-built container in the Docker Hub so you should build it in your local mcachine. Since it is a single arch build i.e sm_86, it builds faster about 15 minutes with 16 core CPU. Open your CMD in  the folder where you have the [Dockerfile](Dockerfile) then run this command :


```bash
$ docker build -f Dockerfile -t <name>:<tag> .
```
You will also see some ignorable deprecation warnings.

## Test the container with the examples in the [older repo](https://github.com/Fizmath/Docker-opencv-GPU) :

The work through how to run and test some examples is in that repo. The only thing you need to do is to replace the older image name ``fizmath/gpu-opencv:latest``  with yours ``` <name>:<tag> ``` .

## CUDA backend crashing issue  :
For the tests 5 and 6 : Real-time face detection with OpenCV DNN and  object detection with YOLO v4 you will get this error :

```
global /opt/opencv-4.6.0/modules/dnn/src/op_cuda.cpp (80)

 initCUDABackend CUDA backend will fallback to the CPU implementation for the layer "_input" of type __NetInputLayer__

```
Check out the [source code ](https://github.com/opencv/opencv/blob/ 3eeec4faae0c8b020d0efd30a51c1e97a0f444a5/modules/dnn/src/dnn.cpp#L2854) for this error

You can see this erro by typing `` export OPENCV_LOG_LEVEL=INFO `` in the container's shell .

I also tried some of the newest models from [OpenCV Zoo](https://github.com/opencv/opencv_zoo). BUT, again got the same CUDA backend crashings.


That error crashes the inference with the GPU.  The reason behind this might be that  OpenCV DNN models are not compatile with the new **NVIDIA Ampere GPUs** architecture or some other issues  ...


If you run the container with CPU, you see that the DNN models work fine with the INTEL TBB and IPP with full CPU cores usage.


If you solve this problem please open up an issue to discuss . 

Thanks .


