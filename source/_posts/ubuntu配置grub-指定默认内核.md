---
title: 'ubuntu配置grub,指定默认内核'
date: 2020-01-09 15:30:29
categories:
- Linux
tags:
- grub
---

# Boot Loader: grub2

BIOS 读完信息后,接下来就是会到第一个开机装置
的 MBR 去读取 boot loader.这个 boot loader 可以具有选单功能、直接加载核心文件以及控制权
移交的功能等，,系统必须要有 loader 才有办法加载该操作系统的核心就是了.

由于MBR是整个硬盘的第一个 sector 内的一个区块,充其量整个大小也才 446 bytes 而已.即使
是 GPT 也没有很大的扇区来储存 loader 的数据.所以分两个阶段

- 执行 boot loader 主程序:在MBR或者boot sector,通常是最小主程序
- 主程序加载配置文件: 配置文件在/boot下,读取了文件系统定义数据之后就能够认识文件系统并读取该文件系统的核心

## 磁盘分区槽在grub2中的代号
/boot/grub/grub.cfg  里面有set root='hd1,gpt5'  指/boot目录挂载的区域

第一颗『搜寻到的硬盘』代号为：『(hd0)』，而该颗硬盘的第一号分区槽为『(hd0,1)』

- 第一颗(MBR):     (hd0) (hd0,msdos1) (hd0,msdos2) (hd0,msdos3)....
- 第二颗(GPT):     (hd1) (hd1,gpt1) (hd1,gpt2) (hd1,gpt3)....
- 第三颗 (hd2):    (hd2,1) (hd2,2) (hd2,3)....

## 配置文件
修改/etc/default/grub,然后使用updata-grub更新/boot/grub/grub.cfg
1. 数字
    
        GRUB_DEFAULT="1> 0"     #表示第二个选单子菜单的第一个  只使用"0" 表示第一个选单
2. ID
        
        GRUB_DEFAULT="Advanced options for Ubuntu>Ubuntu, with Linux 4.15.0-74-generic"
    
    可通过一下命令查看目前系统选单
        
        awk -F\' '/menuentry |submenu / {print $1 $2}' /boot/grub/grub.cfg  
3. saved   
        
         GRUB_DEFAULT=saved     
    代表使用 grub2-set-default 来设定哪一个 menuentry 为默认值的意思。通常预设为 0 

## 遇到的问题
1. 调用了update-grub之后/boot/grub/grub.cfg并没有改变,查看文件内部发现grub.cfg是需要使用grub-mkconfig

        sudo grub-mkconfig -o /boot/grub/grub.cfg   # 可以先备份一下配置文件
        
# 引用
1. [Setup GRUB to always boot the real-time kernel](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_robot_driver/doc/real_time.md)
2. 鸟哥的Linux私房菜-基础学习篇(第四版)         
        
             