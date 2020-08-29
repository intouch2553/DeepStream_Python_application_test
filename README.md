# DeepStream_Python_application_test

This project is for testing the demo deepstream with a python api using docker. 

You need to prepare:
  -linux computer with nvidia gpu hardware
  
----------------------------------------------------------------------------------------------------------------
Running DeepStream

Prerequisites

Ensure these prerequisites are available on your system:

1.nvidia-docker We recommend using Docker 19.03 along with the latest nvidia-container-toolkit as described in the installation steps. Usage of nvidia-docker2 packages in conjunction with prior docker versions is now deprecated.

2.NVIDIA display driver version 440+

Pull the container

Before running the container, use docker pull to ensure an up-to-date image is installed. Once the pull is complete, you can run the container image.

Procedure

  1.In the Pull column, click the icon to copy the docker pull command for the deepstream container of your choice

  2.Open a command prompt and paste the pull command. The pulling of the container image begins. Ensure the pull completes successfully before proceeding to the next step.
  
 
To run the container:

1.Allow external applications to connect to the host's X display:
 $ xhost +
2.Run the docker container (use the desired container tag in the command line below):
 $ docker run --gpus all -it --rm -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -w /opt/nvidia/deepstream/deepstream-5.0  nvcr.io/nvidia/deepstream:5.0-20.07-triton
  
  
After this, you will be placed in a deepstream container.

We will use deepstream_python_apps to execute the pipeline.

First we need to install Gst Python v1.14.5 for Initializing GStreamer in order to create a pipeline using the below command.
$ sudo apt-get install python-gi-dev

Running Sample Applications

Clone the deepstream_python_apps repo under <DeepStream 5.0 ROOT>/sources: git clone https://github.com/NVIDIA-AI-IOT/deepstream_python_apps

This will create the following directory:
<DeepStream 5.0 ROOT>/sources/deepstream_python_apps

The Python apps are under the "apps" directory.
Go into each app directory and follow instructions in the README.

NOTE: The app configuration files contain relative paths for models.


For me now, I haven't changed or played any gimmicks about the element of the pipeline, just try the demo by changing the input.

The ones that I have tested are deepstream_test_1.py , dstest1_pgie_config.txt

Prequisites:
- DeepStreamSDK 5.0
- Python 3.6
- Gst-python

To run the test app:
  $ python3 deepstream_test_1.py <h264_elementary_stream>

This document shall describe the sample deepstream-test1 application.

It is meant for simple demonstration of how to use the various DeepStream SDK
elements in the pipeline and extract meaningful insights from a video stream.

This sample creates instance of "nvinfer" element. Instance of
the "nvinfer" uses TensorRT API to execute inferencing on a model. Using a
correct configuration for a nvinfer element instance is therefore very
important as considerable behaviors of the instance are parameterized
through these configs.

For reference, here are the config files used for this sample :
1. The 4-class detector (referred to as pgie in this sample) uses
    dstest1_pgie_config.txt

In this sample, we first create one instance of "nvinfer", referred as the pgie.
This is our 4 class detector and it detects for "Vehicle , RoadSign, TwoWheeler,
Person".
nvinfer element attach some MetaData to the buffer. By attaching
the probe function at the end of the pipeline, one can extract meaningful
information from this inference. Please refer the "osd_sink_pad_buffer_probe"
function in the sample code. For details on the Metadata format, refer to the
file "gstnvdsmeta.h"

  
----------------------------------------------------------------------------------------------------------------
  
credit: https://ngc.nvidia.com/catalog/containers/nvidia:deepstream;https://github.com/NVIDIA-AI-IOT/deepstream_python_apps/blob/master

  

