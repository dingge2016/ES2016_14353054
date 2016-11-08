#lab5:ROS安装
##ROS在Ubuntu14.04上的安装和配置
-----
ROS是一个开放的标准平台，它提供了一系列的软件框架和工具以帮助软件开发者创建机器人应用软件。

##安装和配置流程如下：
###预准备
------
####设置软件源
设置软件源可以使操作系统就知道去哪里下载程序，并根据命令自动安装软件。
设置软件源的代码如下：
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list'
```


####设置密钥
```
wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
```
###安装ROS
------
首先确认Debian的软件包索引是最新的。

```sudo apt-get update```

在ROS中有许多不同的函数库和工具。完全安装时的工具包括ROS、rqt、可视化环境rviz、通用机器人库robot-generic libraries、2D（如stage）和3D（如Gazebo）仿真环境2D/3D simulators、导航功能包集navigation and 2D/3D（移动、定位、地图绘制、机械臂控制）、感知库perception（如视觉、激光雷达、RGB-D摄像头等）。此处使用完全安装

```udo apt-get install ros-indigo-desktop-full```
####初始化rosdep

rosdep不仅能够使你更方便的安装一些系统依赖程序包，而且ROS的一些主要部件的运行也需要rosdep。

```
sudo rosdep init 
rosdep update
```
####安装rosinstall

rosinstall命令是一个使用的非常频繁的命令，使用这个命令可以轻松的下载许多ROS软件包。

```sudo apt-get install python-rosinstall```

###设置环境
------
添加ROS的环境变量，使打开新的shell时，bash会话中会自动添加环境变量。

```
echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc 
source ~/.bashrc
```
 
  确定的你的ROS环境变量设置好了，检查是否有ROS_ROOT和ROS_PACKAGE_PATH这些环境变量。

```export | grep ROS``` 

配置完成截图：

 ![](http://ww3.sinaimg.cn/mw690/a44300b5gw1f9ku78iycxj20kx03mgnf.jpg)

环境设置文件是为你产生的，但是可以来自不同的地方：

> * 使用package管理器安装的ROS package提供setup.*sh文件；
* rosbuild workspace使用像rosws这样的工具提供setup.*sh文件；
* setup.*sh文件在编译和安装catkin package时作为副产品创建。

如果你在Ubuntu上使用apt工具安装ROS，那么你会在'/opt/ros/indigo/'目录中有setup.*sh文件，你可以这样source它们：

```source /opt/ros/indigo/setup.bash```

你每次打开新的shell都需要运行这个命令，如果你把source /opt/ros/indigo/setup.bash添加进.bashrc文件就不必要每次打开一个新的shell都运行这条命令才能使用ROS的命令了。

####创建ROS工作环境

对于ROS Groovy和之后的版本可以参考以下方式建立catkin工作环境。在shell中运行：

```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
```

可以看到在src文件夹中可以看到一个CMakeLists.txt的链接文件，即使这个工作空间是空的（在src中没有package），任然可以建立一个工作空间。

```
cd ~/catkin_ws/
catkin_make
```
输出结果

![](http://ww2.sinaimg.cn/mw690/a44300b5gw1f9ku6pjf0xj20kj0aw0xv.jpg)

catkin_make命令可以非常方便的建立一个catkin工作空间，在你的当前目录中可以看到有build和devel两个文件夹，在devel文件夹中可以看到许多个setup.\*sh文件。在继续下一步之前先启动你的新的setup.\*sh 文件。

```source devel/setup.bash```

为了确认你的环境变量是否被setup脚本覆盖了，可以运行一下命令确认你的当前目录是否在环境变量中：

```echo $ROS_PACKAGE_PATH```

最后输出：

![](http://ww4.sinaimg.cn/mw690/a44300b5gw1f9ku77pip4j20fr01574l.jpg)

 
