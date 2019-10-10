---
title: gdb调试
date: 2019-10-03 09:09:21
categories:
- Linux

tags:
- gdb

---

# 编译器
gcc(GNU Compiler Collection)

    -c  激活预处理、编译和汇编，生成.o文件
    -S  激活预处理和编译，生成.s的汇编代码wenjain
    -E  激活预处理，结果输出到标准输出(屏幕)，可结合重定向输出到文件 > pre_test
    -g  为调试程序生成相关信息
    
    默认生成a.out文件，可以使用-o 指定输出文件
    
编译c++：g++  gcc可以编译c++源文件，但是不会自动和c++程序使用的库连接，所以通常使用g++,会自动调用gcc

# gdb调试 
很多命令可以缩写为一个字母   
## 启动
    gcc -g summary.c
    gdb a.out           #二进制文件作为gdb参数就可以了
## 查看源代码
    l       #list
    search  #reverse-search
    回车      #重复上一指令
    
## 设置断点
    b 10  #第10行设置断点
    info break   #查看断点信息
    clear       #清除当前行断点
    
## 运行程序
    r
    n   #单步执行程序
    continue    #运行到下一个断点
    s   #step  遇到函数调用会进入函数内部 next不会
## 监视变量
    print variable  #缩写p  
    watch varialbe    #监视
## 临时改变变量
    set var i=1 #临时设置i=1
## 查看堆栈
    bt  #程序调用函数，函数的地址、参数、函数内局部变量都会压入
    
# ROS中使用
launch文件

    launch-prefix="xterm -e gdb --args"     //in a separate xterm window
    launch-prefix="gdb -ex run --args"　　　 //in the same xterm
    
使用memcheck启动valgrind来检测程序内存泄露　使用callgrind执行性能分析
    
    launch-prefix="valgrind"    //valgrind
设置ROS节点core文件转储　　coredump　进程突然崩溃的那一刻的内存快照

    ulimit -c unlimited
    echo 1 > /proc/sys/kernel/core_uses_pid     #将core文件名设置成默认使用进程的pid
    
    gdb  program_name  core_name  #查看core文件　program为编译好的　　/var/core_log
# rosrun启动gdb
    rosrun --prefix 'gdb -ex run --args' [package_name] [node_name]     
    
    
    

    

# 引用
1. [ros Tutorial Roslaunch Nodes in Valgrind or GDB](http://wiki.ros.org/roslaunch/Tutorials/Roslaunch%20Nodes%20in%20Valgrind%20or%20GDB)                  
    