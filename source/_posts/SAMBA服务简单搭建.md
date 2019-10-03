---
title: SAMBA服务简单搭建
date: 2019-10-01 21:00:55
categories:
- Linux

tags:
- 文件共享

---

# SAMBA简介
Samba是在Linux和UNIX系统上实现SMB协议的一个免费软件，由服务器及客户端程序构成。
SMB（Server Messages Block，信息服务块）是一种在局域网上共享文件和打印机的一种通信协议，
它为局域网内的不同计算机之间提供文件及打印机等资源的共享服务。

可以实现Windows和Linux主机之间共享资源互访功能

实验室小范围使用 直接采用匿名共享资源 简单快捷

#  配置

server 服务器
    
    sudo apt install samba    # samba-common 会自动安装
    sudo vi /etc/samba/smb.conf
    
    [share]             #共享目录
    comment=Linux share     #共享目录描述，给自己看的
    path=/home/share    #共享目录路径
    public=yes
    writeable=yes   #可写
    browseable=yes  #共享在Windows中网上邻居可见 结合public
    guest ok=yes    #允许匿名访问
    
        
    sudo /etc/init.d/samba start  # 开启服务器
    
client ubuntu
    
    sudo apt install smbclient
    smbclient -L 10.1.1.20   #查看共享目录
    smbclient //10.1.1.20/share  # IP地址后接共享目录  访问
    
    sudo apt-get install cifs-utils   #下载相应组件
    sudo mount -t cifs //10.1.1.20/share  /home/yang/share
    
    # vi /etc/fstab 开机自动挂载 具有写权限 注意逗号 之前就是用的空格导致不行
    # cifs 文件系统  0: 备份频度0执行备份(完全备份) 0：fsck检查次序为0(序号为0的最先检查)
    //10.1.1.20/share	/home/yang/share cifs passward=123,dir_mode=0777,file_mode=0777 0 0  
              
client Windows

    "运行"对话框输入\\10.1.1.20 回车
    选中共享文件夹 右键映射网络驱动器 即可像本地磁盘使用
# 服务器开机自启动

    sudo vi /etc/rc.loca
    
    /etc/init.d/ssh start # 末尾添加 exit 0 之前
    /etc/init.d/smbd start    
    
# 引用
1. [Ubuntu终端访问samba服务器](https://blog.csdn.net/qq_43682605/article/details/86562212)    
2. [如何配置使Ubuntu启动后samba服务与ssh服务开机自动启动](https://www.cnblogs.com/torres-9/p/5880188.html?utm_source=itdadao&utm_medium=referral)