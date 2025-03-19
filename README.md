# RTABMap-Slam-on-Jetson-Nano

# Setting Up ROS Noetic in a Docker Container

The Jetson Nano runs Ubuntu 18.04 with specialized JetPack versions. To set up a ROS1 Noetic environment on Jetson Nano, we use a Docker container with the following command:

```
docker run -it --privileged \
    --device=/dev/video0:/dev/video0 \
    --device=/dev/video1:/dev/video1 \
    --device=/dev/video2:/dev/video2 \
    --device-cgroup-rule='c 81:* rmw' \
    -v /dev/bus/usb:/dev/bus/usb \
    -v /dev:/dev \
    -v /home/nano/librealsense:/librealsense \
    ros:noetic
```

**Checking Video Devices and USB Access**

To ensure that the Docker container has access to the video devices and USB ports, run the following commands inside the container:

#Install v4l-utils

`sudo apt-get install v4l-utils`

#Check available video devices

`v4l2-ctl --list-devices`

#Check device permissions

`ls -l /dev/video*`

# Installing Intel RealSense SDK

After starting the Docker container, set up the Intel RealSense SDK by mounting the host's SDK directory inside the container.

**Install Dependencies**

`sudo apt-get update`

`sudo apt-get install -y cmake libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev`

**Build and Install the SDK**

`cd /librealsense`

`mkdir build && cd build`

#Configure with libuvc backend since we're on Jetson

`cmake ../ -DBUILD_EXAMPLES=true -DFORCE_LIBUVC=true`

#Build the SDK

`make -j4`

#Install

`sudo make install`

# Installing ROS Wrapper for RealSense

To integrate RealSense cameras with ROS, install the required packages:

`sudo apt-get install ros-$ROS_DISTRO-realsense2-camera`

Installing RealSense Description Package

The realsense2_description package includes 3D models of RealSense devices, which are necessary for running certain launch files (e.g., rs_d435_camera_with_model.launch). Install it with:

`sudo apt-get install ros-$ROS_DISTRO-realsense2-description`
