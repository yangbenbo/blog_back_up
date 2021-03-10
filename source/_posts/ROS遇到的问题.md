---
title: ROS遇到的问题
date: 2019-12-21 11:54:38
categories:
- ROS
tags:
- ROS
---

# 终端结束所有节点之后显示仍有节点
清理node,否则影响查看节点通信
    
    rosnode list
    rosnode cleanup

# 程序排错

首先看**循环**,是否调用ros::spinOnce(),对应的数据是否更新

好几次是因为循环没有更新数据,排错废了不少时间

- 没有调用ros::spinOnce(),导致没有处理回调函数,也就没有更新数据
- 回调函数处理后的数据是否放入对应变量,有时候用的是中间变量,导致更新的数据没有放进去

ROS也有异步更新的机制,之前用过,出过一次问题,可以根据需要选择更新的机制

# KDL和trac-ik库使用
在ubuntu16+ros kinetic 和ubuntu18+ros melodic中不一样
后一版本中需要用于初始化正逆解对象的KDL::Chain一直存在，可作为类成员变量，可能是后一版本中进行了优化，减少了拷贝的次数

# 远程通信
When a ROS node advertises a topic, it provides a hostname:port combination (a URI) that other nodes will contact when they want to subscribe to that topic. 
**It is important that the hostname that a node provides can be used by all other nodes to contact it.**
注意需要知道相互的主机名到ip的映射,否则可能会有问题,有一次没配置主机名的映射,从机可以获取主机的会话题信息,但是无法连接上主机的server

例子:remote为主机名master,本地主机名为slave

    hostname    # 查看本机主机名 hostname
    # 修改ip映射 主机加上从机的hostname 和 ip
    # 从机加上主机的hostname 和 ip    
    sudo vim /etc/hosts 
    
    # 修改ros 环境变量
    vim ~/.bashrc
    # 主机 增加 如下  localhost可以直接使用ip或者主机名(master,slave)
    export ROS_HOSTNAME=localhost
    export ROS_MASTER_URI=http://localhost:11311
    # 从机增加如下
    export ROS_HOSTNAME=localhost
    export ROS_MASTER_URI=http://master:11311
    
## rosrun rqt_tui rqt_gui 报错 segfaults
问题描述:
可以使用`rqt`打开,但是无法使用`rosru rqt_gui rqt_gui`打开,导致无法使用easy-hand-eye标定包

通过查阅[rqt segfaults on kinetic #114](https://github.com/ros-visualization/rqt/issues/114)
[rqt segmentation fault following tutorials](https://answers.ros.org/question/253655/rqt-segmentation-fault-following-tutorials/)
得知问题主要是PyQt version的原因,所以试过了链接中的方法,都不太行,最后是卸载了qt5*,然后重新安装rqt_gui,发现只要安装rqt之后就无法通过`rosrun`的方式打开`
    
    sudo apt remove qt5*
    sudo apt install ros-kinetic-rqt-gui

## "Failed to fetch current robot state" when running getCurrentPose #1187
描述:使用moveit的时候出现无法获取机械臂当前状态信息k

解决方案:

    // 异步处理回调函数,处理更新
    ros::AsyncSpinner spinner(1); 
    spinner.start();

## 多个工作空间`activate`,即`Sourcing from multiple workspaces`
描述,想要多个ROS空间的环境变量同时工作
- 如果使用clion编译则可以不用管,直接source cmake-build-debug/devel/setup.bash
- 使用`catkin`编译则需要注意空间[Sourcing from multiple workspaces](https://answers.ros.org/question/205976/sourcing-from-multiple-workspaces/)
`catkin_make` 第一次运行会生成`devel`文件夹,并创建`setup.bash`,这个文件会覆盖所有其他已经被`source`的空间(除了ROS本身的安装空间),如果需要多个空间同时`active`
**注意只需要在第一次编译的时候移除文件夹devel,然后重编译.之后改动就正常编译就行**

        # 需要新加空间的地方
		catkin_make
		rm -rf devel/
		catkin_make
		source devel/setup.bash

## 实时性
可以调整CPU频率，intel的cpu默认省电模式，频率较低，可能会导致延时大
使用cpufreq，参考[franka操作](https://frankaemika.github.io/docs/troubleshooting.html)

##　Gazebo第一次加载太慢
把[模型](https://bitbucket.org/osrf/gazebo_models/downloads/  )放在指定目录下 `~/.gazebo/models` 下保证模型顺利加载


## 引用
1. [ROSNetworkSetup](http://wiki.ros.org/ROS/NetworkSetup)             
