# RTAB_Map Setup
Setting up rtab_map and other files in a clean LattePanda  
The main link used is https://github.com/IntelRealSense/realsense-ros  

## system settings  
```system setting --> software updates --> enable main universe restricted and multiverse, then change server to singapore```  

## Installing the latest Intel RealSense SDK 2.0  
The link followed is https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md#installing-the-packages
  
1) Registering the server's public key:  
```sudo apt-key adv --keyserver keys.gnupg.net --recv-key C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key C8B3A55A6F3EFCDE```  
if the key is still not retrieved, run 
```export http_proxy="http://<proxy>:<port>"```
and rerun the command.

2) Add server to the list of repositories  
```sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial main" -u```

3) Install the libraries  
```sudo apt-get install librealsense2-dkms```  
```sudo apt-get install librealsense2-utils```

4) Install the developer packages  
```sudo apt-get install librealsense2-dev```  
with the dev package installed, we can compile an application with librealsense using ```g++ -std=c++11 filename.cpp -lrealsens2``` or and IDE of your choice.

5) To test, run ```realsense-viewer``` to verify the installation.

6) Verify that the kernel is updated:  
```modinfo uvcvideo | grep "version:"```

7) Upgrading the packages:  
```sudo apt-get update```  
```sudo apt-get upgrade```

## Ubuntu install of ROS Kinetic  (Installing the ROS distribution)
The link followed is http://wiki.ros.org/kinetic/Installation/Ubuntu  

1) Setup sources.list  
```sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'```

2) Set up keys  
```sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116```

3) Update  
```sudo apt-get update```

4) Install Desktop-Full Install  
```sudo apt-get install ros-kinetic-desktop-full```  

5) Init rosdep  
```sudo rosdep init```  
```rosdep update```  

6) Environment setup  
```echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc```  
```source ~/.bashrc```  

7) Install the tool and other dependencies for building ROS packages  
```sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential```  

## Install Intel RealSense ROS from Sources  
This is a continuation (step 3) of https://github.com/IntelRealSense/realsense-ros  

1) Create a catkin (can be named anything) workspace  
```mkdir -p ~/shadow_ws/src```  
```cd ~/shadow_ws/src/```  

2) Clone latest Intel RealSense ROS from [here](https://github.com/IntelRealSense/realsense-ros/releases) into 'shadow_ws/src/'  

3) Install ddynamic_reconfigure  
```sudo apt-get install ros-kinetic-ddynamic-reconfigure```  

4) Continuing with catkin
```catkin_init_workspace```  
```cd ..```  
```catkin_make clean```  
```catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release```  
```catkin_make install```  
```echo "source ~/shadow_ws/devel/setup.bash" >> ~/.bashrc```  
```source ~/.bashrc```  

5) starting camera node in ROS:  
```roslaunch realsense2_camera rs_camera.launch```  
aligned depth frames  
```roslaunch realsense2_camera rs_camera.launch align_depth:=true```  
point cloud  
```roslaunch realsense2_camera rs_camera.launch filters:=pointcloud```  

![point cloud2](https://github.com/frankienaik/rtab_map_setup/blob/master/pointcloud2.png)  
  
  
  

## Slamtec Rplidar_ros  
The link followed is https://github.com/Slamtec/rplidar_ros  

1) Clone the project into our catkin's workspace src folder  
```cd shadow_ws/src```  
```git clone https://github.com/Slamtec/rplidar_ros```  

2) Running catkin_make to build rplidarNode and rpilidarNodeClient 
```catkin_make``` in ws folder  

3) go into rplidar_ros/scripts and run  
```./create_udev_rules.sh```  
check ```ls -l /dev |grep ttyUSB``` to see rplidar -> ttyUSB0  

4) run ```roslaunch rplidar_ros view_rplidar_a3.launch```


## RTAB-Map  
The link followed is https://github.com/introlab/rtabmap_ros#rtabmap_ros-  

1) Install ROS distribution  
```sudo apt-get install ros-kinetic-rtabmao-ros```

Install RTAB-Map standalone libraries (not cloned in catkin workspace, but at root)
```cd ~```  
```git clone https://github.com/introlab/rtabmap.git rtabmap```  
```cd rtabmap/build```  
```cmake ..```  
```make```  
```sudo make install```  

Install RTAB-Map ro-pkg in src folder of Catkin workspace
```cd ~/catkin_ws```  
```git clone https://github.com/introlab/rtabmap_ros.git src/rtabmap_ros```  
```catkin_make -j1```  

