# 智能车仿真项目复现
---
## 一、创建一个ros的<mark>工作空间</mark>(即一个文件夹)
    mkdir -p ~/racecar_ws/src
 这个工作空间可以用来存放我们的所有代码和资源,mkdir命令用于创建文件夹，-p命令代表同时创建子目录，racecar_ws则是我们工作空间的名字
## 二、初始化工作空间
    cd ~/racecar_ws/src

    catkin_init_workspace
## 三、克隆程序并且编译
    git clone https://github.com/xmy0916/racecar.git
    
    cd ..

    catkin_make

 ### <mark>若装有多个python版本可能会出现如下报错

 ![问题](/home/tianbot/Pictures/il.png)
原因是系统安装了多个版本的python解释器，指定python版本即可，例如：

    catkin_make -DPYTHON_EXECUTABLE=/path/to/python
    
/path/to/python替换为你的python解释器路径

### <mark>若出现以下报错
![1](/home/tianbot/Downloads/2.png)
原因可能是没有下载控件，解决办法：
    
    sudo apt-get install ros-<ros版本>-driver-base
缺少什么安装什么即可

若是控件安装完后依然存在显示无法找到库文件，可在对应的cmakelists文件中设定库文件的位置（可用以下语法找库文件的位置）

    locate driver_baseConfig.cmake

获得的是你的实际库文件安装路径

    export driver_base_DIR=实际安装路径

最后再次编译，出现以下图就算编译成功

![3](/home/tianbot/Downloads/3.PNG)
---
## 四、开始仿真
### 1.建立仿真地图
具体可参考以下[点击此处学习gazebo建图](https://blog.csdn.net/ZhangRelay/article/details/92799977)
建好的图如下

![4](/home/tianbot/Downloads/4.PNG)

### 2.启动仿真
<mark>首先修改环境变量
    
    echo "source ~/racecar_ws/devel/setup.bash" >> ~/.bashrc

    source ~/.bashrc
下面是这个命令的作用：

1.source命令用于在当前Shell会话中执行指定的脚本或命令。

2.~/racecar_ws/devel/setup.bash是一个脚本文件的路径，它位于您的ROS工作区中的devel文件夹中。这个脚本包含了设置ROS环境所需的环境变量和函数。

3.~/.bashrc是一个Shell配置文件，用于在每次启动新的Shell会话时自动执行。通过将source命令添加到.bashrc文件中，您确保了每次打开终端时都会加载ROS环境。

<mark>然后就启动地图

    roslaunch racecar_gazebo racecar_runway.launch
启动后将会看到地图：
![5](/home/tianbot/Pictures/5.png)

新开一个终端，启动slam和rviz
    
    roslaunch racecar_gazebo slam_gmapping.launch

如下：
![6](/home/tianbot/Downloads/6.PNG)
将鼠标保持点击在tk框（使键盘控制节点保持激活状态）。使用wasd对小车进行移动，小车跑完整个地图后，保存地图

保存地图到以下的位置，因为移动小车时有撞墙的情况，所以地图不是很完整
![7](/home/tianbot/Downloads/7.PNG)

## 五、进行导航

### 1.启动导航和环境地图
首先启动自己的launch文件，把原文件的这三个地方改为自己的文件名字即可
![8](/home/tianbot/Downloads/8.PNG)
然后运行launch文件

    roslaunch racecar_gazebo racecar_runway_navigation.launch

### 2.启动rviz
    roslaunch racecar_gazebo racecar_rviz.launch

### 3.选择2D Nav Goal发布目标

在这里选择目标位置，通过2D Nav Goal选择地图上的一个位置进行导航
![9](/home/tianbot/Downloads/9.PNG)

## 4.启动python文件进行导航

    rosrun racecar_gazebo path_pursuit.py
小车会缓慢移动至终点，终端会出现以下反馈,最后出现Goal Reached!表示小车成功到达目标点
![10](/home/tianbot/Downdloads/10.PNG)




    






