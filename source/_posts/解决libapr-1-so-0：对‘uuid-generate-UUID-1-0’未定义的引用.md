---
title: 解决conda和ros库不兼容问题,libapr-1.so.0：对‘uuid_generate@UUID_1.0’未定义的引用
date: 2020-11-10 11:05:40
categories:
- Linux
tags:
- anaconda
---

## 主要是因为conda和ros兼容的问题
1. 问题关键词： **libapr-1.so.0**和**uuid**
2. locate第一个关键词: 
        
        $ locate libapr-1.so.0 
        /usr/lib/x86_64-linux-gnu/libapr-1.so.0
        /usr/lib/x86_64-linux-gnu/libapr-1.so.0.5.2
3. 查看该库对第二个关键字的依赖包位置
        
        $ ldd /usr/lib/x86_64-linux-gnu/libapr-1.so.0 |grep uuid
        libuuid.so.1 => /lib/x86_64-linux-gnu/libuuid.so.1 (0x00007f3588aab000)
    这是ros系统所使用的库依赖，报错是因为anaconda中也有libuuid.so.1,但是版本不同
4. locate第二个关键词
        
       $ locate libuuid.so.1  
       /home/yang/anaconda3/envs/franka/lib/libuuid.so.1
       /home/yang/anaconda3/envs/franka/lib/libuuid.so.1.0.0
       /home/yang/anaconda3/lib/libuuid.so.1
       /home/yang/anaconda3/lib/libuuid.so.1.0.0
       /home/yang/anaconda3/pkgs/libuuid-1.0.3-h1bed415_2/lib/libuuid.so.1
       /home/yang/anaconda3/pkgs/libuuid-1.0.3-h1bed415_2/lib/libuuid.so.1.0.0
       /lib/i386-linux-gnu/libuuid.so.1
       /lib/i386-linux-gnu/libuuid.so.1.3.0
       /lib/x86_64-linux-gnu/libuuid.so.1
       /lib/x86_64-linux-gnu/libuuid.so.1.3.0
       /snap/core/10126/lib/x86_64-linux-gnu/libuuid.so.1
       /snap/core/10126/lib/x86_64-linux-gnu/libuuid.so.1.3.0
       /snap/core/10185/lib/x86_64-linux-gnu/libuuid.so.1
       /snap/core/10185/lib/x86_64-linux-gnu/libuuid.so.1.3.0
   可以看出,ros使用的是/lib/x86_64-linux-gnu/libuuid.so.1,由于anaconda的存在,我使用的库是/home/yang/anaconda3/envs/franka/lib/libuuid.so.1
   对比两个版本
   
       $ ll /lib/x86_64-linux-gnu/ |grep uuid   
       lrwxrwxrwx  1 root root      16 Jan 27  2020 libuuid.so.1 -> libuuid.so.1.3.0
       -rw-r--r--  1 root root   18976 Jan 27  2020 libuuid.so.1.3.0
        
       $ ll ~/anaconda3/envs/franka/lib |grep uuid
       -rw-rw-r--  3 yang yang    26398 Jan 12  2018 libuuid.a
       -rwxrwxr-x  1 yang yang      978 Nov  9 16:05 libuuid.la*
       lrwxrwxrwx  1 yang yang       16 Nov  9 16:05 libuuid.so -> libuuid.so.1.0.0*
       lrwxrwxrwx  1 root root       34 Nov 10 09:35 libuuid.so.1 -> libuuid.so.1.0.0*
       -rwxrwxr-x  3 yang yang    18472 Jan 12  2018 libuuid.so.1.0.0*
       
       可以看出系统库是1.3.0版本,anaconda使用的是1.0.0版本
5. 把anaconda库链接到系统的库(比较简单,可以先把anaconda对应库备份一下,也可以考虑升级anaconda对应库,但是我没找到)
        
        mv ~/anaconda3/envs/franka/lib/libuuid.so.1 ~/anaconda3/envs/franka/lib/libuuid.so.1.back       
        sudo ln -s /lib/x86_64-linux-gnu/libuuid.so.1 ~/anaconda3/envs/franka/lib/libuuid.so.1

## 引用
1. [解决libapr-1.so.0：对‘uuid_generate@UUID_1.0’未定义的引用](https://blog.csdn.net/qq_36013249/article/details/103311001)