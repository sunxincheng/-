-----------------------------------------------------------------------------------
                      pangolin 安装
安装前准备；
安装GCC：
    sudo apt-get install build-essential
安装CMake
     sudo apt-get install cmake
安装Git
     sudo apt-get install git
安装GTK开发版
     sudo apt-get install libgtk2.0-dev
安装pkg-config
    sudo apt-get install pkg-config
安装媒体包：
    sudo apt-get install ffmpeg  



sudo apt-get install libglew-dev

sudo apt-get install libpython2.7-dev

 sudo apt-get install python-pip

sudo python -mpip install numpy pyopengl Pillow pybind11   

sudo apt-get install ffmpeg libavcodec-dev libavutil-dev libavformat-dev libswscale-dev

sudo apt-get install libdc1394-22-dev libraw1394-dev

sudo apt-get install libjpeg-dev libpng12-dev libtiff5-dev libopenexr-dev


git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
mkdir build
cd build
cmake ..
cmake --build .
sudo make install
----------------------------------------------------------------------------------------
                                      OpenCV 安装
下载opencv   https://github.com/Itseez/opencv/archive/2.4.13.zip
进入下载目录解压
编译安装
    打开文件夹"opencv-2.4.13"：
    cd opencv-2.4.13
    新建一个文件夹用于存放临时文件：
    mkdir release
    切换到该临时文件夹：
    cd release
    开始编译：
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
    make -j4    //开启线程 按照自己的配置
    sudo make install
相关配置
  配置环境
     将opencv的库加入到路径，从而让系统可以找到
     sudo gedit /etc/ld.so.conf.d/opencv.conf
     末尾加入/usr/local/lib，保存退出
     sudo ldconfig    使配置生效
     sudo gedit /etc/bash.bashrc 
     末尾加入
     PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig
     export PKG_CONFIG_PATH
     保存退出
   
     su（进入root权限）
     输入密码
     source /etc/bash.bashrc   #使配置生效
     Ctrl+d（退出root）
     sudo updatedb #更新database
-------------------------------------------------------------------------------------
                  Google Ceres-Solver 安装    http://www.ceres-solver.org/installation.html
# CMake
sudo apt-get install cmake
# google-glog + gflags
sudo apt-get install libgoogle-glog-dev
# BLAS & LAPACK
sudo apt-get install libatlas-base-dev
# Eigen3
sudo apt-get install libeigen3-dev
# SuiteSparse and CXSparse (optional)
# - If you want to build Ceres as a *static* library (the default)
#   you can use the SuiteSparse package in the main Ubuntu package
#   repository:
sudo apt-get install libsuitesparse-dev

下载Ceres-Solver源码  http://www.ceres-solver.org/installation.html
到下载目录下解压下载好的源程序
tar zxf ceres-solver-1.13.0.tar.gz
mkdir ceres-bin
cd ceres-bin
cmake ../ceres-solver-1.13.0
make -j3
make test
# Optionally install Ceres, it can also be exported using CMake which
# allows Ceres to be used without requiring installation, see the documentation
# for the EXPORT_BUILD_DIR option for more information.
sudo make install
-------------------------------------------------------------------------------------
             ROS(Robot Operate System) 安装   http://wiki.ros.org/kinetic/Installation/Ubuntu
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

sudo apt-get update

sudo apt-get install ros-kinetic-desktop-full

sudo rosdep init

rosdep update

echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc

source ~/.bashrc

sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential

-------------------------------------------------------------------------------------
                         小觅相机的开发包安装
下载小觅相机开发包 https://github.com/slightech/MYNT-EYE-SDK  https://pan.baidu.com/s/1i5REqVz#list/path=%2F
选择gcc5和opencv2.4.13版本的开发包。
# MYNT EYE SDK on Linux

## Getting Started

### Prerequisites

```
# Install build tools
sudo apt-get install build-essential cmake git

# Install OpenCV dependencies
sudo apt-get install pkg-config libgtk2.0-dev

# Install ssl for https, v4l for video
sudo apt-get install libssl-dev libv4l-dev v4l-utils

### Install SDK

Go to the SDK directory and edit `sdk.cfg` to set your OpenCV install directory etc., then install:

```
cd <sdk directory>
./install.sh
source ~/.bashrc
```

### Check Installation

After `./install.sh`, you could check the environment variables:

echo $MYNTEYE_SDK_ROOT
<Should be the sdk path>

-------------------------------------------------------------------------------------
             源程序的编译方式（默认该系统已经配置好ROS环境，OpenCV，MYNTEYE等环境）
递归创建一个文件夹。
mkdir -p ROS_VIN/src

将源程序拷贝到src文件夹目录下
cp MYNT-EYE-VINS-Sample MYNT-EYE-ROS-Wrapper ~/ROS_VIN/src

cd 到ROS_VIN目录下
cd ~/ROS_VIN

采用ROS下catkin_make 进行编译
catkin_make -j2   //-jn 是指定编译的核心数，一般双核四线程，n最大为4，由于电脑棒本身性能的问题，采用，四线程容易卡死，所以我们采用n=2

编译完成时，在当前终端或新打开一个终端启动ROS系统
roscore

再新打开一个终端，启动执行相机数据的采集
cd ~/ROS_VIN
source ./devel/setup.bash //将当前的编译好程序的环境添加到ROS环境下
roslaunch mynteye_ros_wrapper mynt_camera.launch  //启动执行相机数据的采集

再新打开一个终端，启动ROS的图像显示界面】
cd ~/ROS_VIN
source ./devel/setup.bash //将当前的编译好程序的环境添加到ROS环境下
roslaunch vins_estimator vins_rviz.launch

再新打开一个终端，执行程序进行无人机的位置估计
cd ~/ROS_VIN
source ./devel/setup.bash //将当前的编译好程序的环境添加到ROS环境下
roslaunch vins_estimator mynteye.launch

----------------------------------------------------------------------------------
                       UDP数据发送与采集
进行udp数据的发送和采集需要首先修改源程序里的IP地址进行重新编译
cd ~/ROS_VINS/src/MYNT-EYE-VINS-Sample/vins_estimator/src
gedit UdpServer.cc
将ser_addr.sin_addr.s_addr = inet_addr("IP");注释去掉IP改成你需要发送的IP地址，再将
ser_addr.sin_addr.s_addr = htonl(INADDR_ANY);添加注释，保存后重新编译MYNT-EYE-VINS-Sample。

---------------------------------------------------------------------------------
                        相机采集到的数据保存
cd 到 .ros目录下创建一个目录
cd ~/.ros
mkdir cam0  //用于保存采集到图片信息 notice：每次执行程序前注意是否存在cam0文件夹，不存在需创建一个，存在将该目录下的图片清空

数据采集完成后将采集好的IMU数据以及图片信息copy出来。
cd ~/.ros
mv cam0 ~/  //移动到主目录下
mv imu.csv imu0.csv ~/    //notice: imu.csv 是相机采集过程中的IMU数据， imu0.csv是vins_estimator程序执行过程中实际运行IMU的数据。
-------------------------------------------------------------------------------------

                 notice :   
        MYNT-EYE-ROS-Wrapper/launch目录下的mynt_camera.launch是指定相机采集的数据
                             
        MYNT-EYE-VINS-Sample/config/mynteye 目录下的mynteye_config.yaml是标定好的相机以及IMU的内参以及外参，以及程序执行的相关参数
