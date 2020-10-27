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