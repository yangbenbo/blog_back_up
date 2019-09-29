---
title: ubuntu小技巧
date: 2019-09-29 21:11:37
categories:
- Linux
tags:
- 

---

# 后台运行
	gedit test.txt &   #  &告诉终端后台运行,
	gedit test.txt  
	ctrl+Z  #移到后台
	bg    #返回gedit
# 进程 
	ps ax | grep sample
	kill <id>     #终止进程

	top   #show process and memory usage
	shift+P   #sort by cpu
	shift+M  #sort by memory
# 其他
	ctrl + L  #在资源管理器中显示绝对路径
- ubuntu 4个工作区 和win10的多桌面一样 ctrl+alt+方向箭头 切换 setting->appearance->behavior->enable workspaces

- deb文件包含二进制文件、库文件、配置文件、帮助文档
ubuntu软件包：二进制包 (Binary Packages)   源码包(Source Packages)
Redhat Linux  -> RPM包    ubunut -> Deb包