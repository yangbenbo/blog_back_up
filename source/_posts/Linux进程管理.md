---
title: Linux进程管理
date: 2019-10-02 09:28:29
categories:
- Linux

tags:
---

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=3 orderedList=false} -->

<!-- code_chunk_output -->

- [Linux进程](#linux进程)
  - [进程管理常用命令](#进程管理常用命令)
  - [遇到的问题](#遇到的问题)
- [临时和周期任务安排](#临时和周期任务安排)

<!-- /code_chunk_output -->

## Linux进程
Linux是一个多用户多任务的操作系统。init进程是所有进程的发起者和控制者，每个进程
都有一个编号，PID(Process ID)，是在当前系统中的运行顺序，init进程在系统运行期间不会消亡或停止

### 进程管理常用命令
1. 查看进程状态

    ps aux      #静态查看系统中所有进程信息
    top         #动态查看  P M T 分别按照CPU Memory Time排序
    
2. 设置优先级 nice [-n][command][arguments]   #优先级-20 - 19 默认为0 -20最高
    
    nice  --12 processname   #-12
    renice          #修改优先级
3. 终止进程

    kill[-signal] PID   #不带参数默认15 即终止进程 
    kill -l             #查看可选signal
    kill -CONT  4385    #重新开始进程
    kill -9 4385        #强制终止
    kill -STOP 4385     #停止但不退出
    
    killall [-signal] [processname]  #终止所有名为processname的进程
4. 查看后台进程
    查看后台进程

        gnome-system-monitor    
    自启动

        gnome-session-properties

### 遇到的问题
1. win10可以联网,ubuntu不能

    关闭网络唤醒,在win10里面的电源计划,勾选快速启动

## 临时和周期任务安排

待补充    
        