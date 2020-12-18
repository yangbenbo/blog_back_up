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
    
        
        
## 引用
1. [ROSNetworkSetup](http://wiki.ros.org/ROS/NetworkSetup)             
