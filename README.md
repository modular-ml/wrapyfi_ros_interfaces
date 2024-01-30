# Wrapyfi ROS interfaces

**WARNING**: These instructions are located in 
[https://github.com/modular-ml/wrapyfi_ros_interfaces](https://github.com/modular-ml/wrapyfi_ros_interfaces)

To transmit ROS audio messages with [Wrapyfi](https://github.com/fabawi/wrapyfi), you need to compile the ROS interfaces. 
ROS must already be installed on your system, with all its build dependencies. 
You can find the installation instructions [here](http://wiki.ros.org/noetic/Installation/Ubuntu) 
or install using [Robostack](https://robostack.github.io/GettingStarted.html).

## Prerequisites

- ROS Noetic
- Python 3.6

## Installation

We recommend [compiling](#compiling) the Wrapyfi ROS interfaces rather than installing them. However, if ROS was installed locally (**not** within mamba/micromamba), 
Then the Wrapyfi interfaces can be installed directly using APT [![ROS Package Index](https://img.shields.io/ros/v/noetic/wrapyfi_ros_interfaces)](https://index.ros.org/r/wrapyfi_ros_interfaces/#noetic)


**APT (local Noetic only)** 
```bash
source /opt/ros/noetic/setup.bash
sudo apt update
sudo apt install ros-noetic-wrapyfi-ros-interfaces
# test package: should return the audio message type for ROS
rosmsg show ROSAudioMessage

```

## Compiling

1. Copy the `wrapyfi_ros_interfaces` folder to your ROS workspace (assumed to be `~/ros_ws`).

    ```bash
    # from the current directory 
    cd ../
    cp -r wrapyfi_ros_interfaces ~/ros_ws/src
    
    ```

2. Compile the ROS interfaces:
    
    ```bash
    cd ~/ros_ws
    catkin_make
    
    ```
    
    **Note**: If the wrong version of Python is used, the compilation will fail. Make sure that the correct version of cmake 
    is used by modifying the `cmake_minimum_required` version in the `~/ros_ws/src/wrapyfi_ros_interfaces/CMakeLists.txt` file:
    
    ```cmake
    # CMakeLists.txt
    cmake_minimum_required(VERSION 3.0.2)
    # ...
    ```
    
    Replacing VERSION 3.0.2 with the correct version of cmake.

3. Source the ROS workspace:

    ```bash
    source ~/ros_ws/devel/setup.bash
    ```

4. Verify that the ROS Audio message interface is compiled:
        
    ```bash
    rosmsg show ROSAudioMessage
    ```
    
    Which should output:
    
    ```bash
   [wrapyfi_ros_interfaces/ROSAudioMessage]:
   std_msgs/Header header
     uint32 seq
     time stamp
     string frame_id
   uint32 chunk_size
   uint8 channels
   uint32 sample_rate
   string encoding
   uint8 is_bigendian
   uint32 bitrate
   string coding_format
   uint32 step
   uint8[] data
    
    ```

5. Verify that the ROS Audio service interface is compiled:
        
    ```bash
    rossrv show ROSAudioService
    ```
    
    Which should output:
    
    ```bash
   [wrapyfi_ros_interfaces/ROSAudioService]:
   string request
   ---
   wrapyfi_ros_interfaces/ROSAudioMessage response
     std_msgs/Header header
       uint32 seq
       time stamp
       string frame_id
     uint32 chunk_size
     uint8 channels
     uint32 sample_rate
     string encoding
     uint8 is_bigendian
     uint32 bitrate
     string coding_format
     uint32 step
     uint8[] data

    ```
   
     Run your Wrapyfi enabled script from the same terminal. Now you can transmit ROS audio messages in PUB/SUB [\[example\]](https://wrapyfi.readthedocs.io/en/latest/examples/examples.sensors.html#module-examples.sensors.cam_mic) and REQ/REP [\[example\]](https://wrapyfi.readthedocs.io/en/latest/examples/examples.communication_patterns.html#module-examples.communication_patterns.request_reply_example).
     
