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
# 网络
	nmap -sn 10.1.1.1-255  # 扫描局域网ip
# 软件安装路径
	whereis gcc   #查看gcc安装路径
	which   gcc  #查看gcc运行路径
rpm

	rpm -ql gcc        #查看gcc相关文件的安装路径
	rpm -qa | grep gcc #查看有没有安装gcc
	rpm -qa            #查看全部使用rpm安装的软件
deb

    dpkg -L gcc         #查看gcc相关文件的安装路径
    dpkg -l | grep gcc  #查看有没有安装gcc
    dpkg -l             #查看全部安装包
    
# 获取命令
    whatis uname    #一行介绍性文字
    apropos search  #想搜索一个文件又忘了命令    
# 其他
	uname    #查看当前系统信息 包括内核
	ctrl + L #在资源管理器中显示绝对路径
	su       #切换用户  sudo su
	who      #查看当前登录用户
	
# 问题
- 更新linux时候提示无法“由于没有公钥，无法验证下列签名 ***”的解决方案

      sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 6AF0E1940624A220 #此处6AF0E1940624A220需要是错误提示的key  
	
- ubuntu 4个工作区 和win10的多桌面一样 ctrl+alt+方向箭头 切换 setting->appearance->behavior->enable workspaces

- deb文件包含二进制文件、库文件、配置文件、帮助文档
ubuntu软件包：二进制包 (Binary Packages)   源码包(Source Packages)
Redhat Linux  -> RPM包    ubunut -> Deb包

# 引用
1. [linux下查看软件安装路径](https://blog.csdn.net/liufuchun111/article/details/80402109)
2. [更新linux时候提示无法“由于没有公钥，无法验证下列签名 ***”的解决方案](https://blog.csdn.net/loovejava/article/details/21837935)