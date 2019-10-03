---
title: FTP配置和使用
date: 2019-09-30 20:45:12
categories:
- Linux
tags:
- 数据传输

---

# FTP简介
文件传输协议(File Transfer Protocol)：传统网络传输协议，主要实现服务器和客户端之间的文件传输

# FTP配置
匿名用户无法使用ls dir等命令查看文件

---
    sudo apt-get install vsftpd   #服务器端
    sudo vim /etc/vsftpd.conf     #可以默认 anonymous_enable=YES 可用anonymous作为用户名和密码登录
    sudo /etc/init.d/vsftpd restart  #重启服务
    
    ftp 10.1.1.20   #连接服务器
    
    
配置文件:vsftpd.conf   # 测试了一下 感觉默认设置就可以本地用户登录 
可能需要改/etc/ftpusers  并重启  


    local_root=/home/ftp  # 锁定共享目录 手动添加  可以不用
    local_enable=YES    #本机可以访问
    write_enable=YES    #允许写操作
    
    #chroot_local_user=YES
    chroot_list_enable=YES    # 限制实体用户在自己的主目录设定文件
    # (default follows)
    chroot_list_file=/etc/vsftpd.chroot_list
    sudo vim /etc/vsftpd.chroot_list     # 添加用户名
    sudo vim /etc/ftpusers  #这里面是不能访问ftp的用户  删除对应的
    
    
# 常用命令
## FTP命令

    ftp> ascii # 设定以ASCII方式传送文件(缺省值) 
    ftp> bell  # 每完成一次文件传送,报警提示. 
    ftp> binary # 设定以二进制方式传送文件. 
    ftp> bye  # 终止主机FTP进程,并退出FTP管理方式. 
    ftp> case # 当为ON时,用MGET命令拷贝的文件名到本地机器中,全部转换为小写字母. 
    ftp> cd   # 同UNIX的CD命令. 
    ftp> cdup  # 返回上一级目录. 
    ftp> chmod # 改变远端主机的文件权限. 
    ftp> close # 终止远端的FTP进程,返回到FTP命令状态, 所有的宏定义都被删除. 
    ftp> delete # 删除远端主机中的文件. 
    ftp> dir [remote-directory] [local-file] # 列出当前远端主机目录中的文件.如果有本地文件,就将结果写至本地文件. 
    ftp> get [remote-file] [local-file] # 从远端主机中传送至本地主机中. 
    ftp> help [command] # 输出命令的解释. 
    ftp> lcd # 改变当前本地主机的工作目录,如果缺省,就转到当前用户的HOME目录. 
    ftp> ls [remote-directory] [local-file] # 同DIR. 
    ftp> macdef         # 定义宏命令. 
    ftp> mdelete [remote-files] # 删除一批文件. 
    ftp> mget [remote-files]  # 从远端主机接收一批文件至本地主机. 
    ftp> mkdir directory-name  # 在远端主机中建立目录. 
    ftp> mput local-files # 将本地主机中一批文件传送至远端主机. 
    ftp> open host [port] # 重新建立一个新的连接. 
    ftp> prompt      # 交互提示模式. 
    ftp> put local-file [remote-file] # 将本地一个文件传送至远端主机中. 
    ftp> pwd # 列出当前远端主机目录. 
    ftp> quit # 同BYE. 
    ftp> recv remote-file [local-file] # 同GET. 
    ftp> rename [from] [to]   # 改变远端主机中的文件名. 
    ftp> rmdir directory-name  # 删除远端主机中的目录. 
    ftp> send local-file [remote-file] # 同PUT. 
    ftp> status  # 显示当前FTP的状态. 
    ftp> system  # 显示远端主机系统类型. 
    ftp> user user-name [password] [account] # 重新以别的用户名登录远端主机. 
    ftp> ? [command] # 同HELP. [command]指定需要帮助的命令名称。如果没有指定 command，ftp 将显示全部命令的列表。
    ftp> ! # 从 ftp 子系统退出到外壳。
   
   
## 关闭FTP
    bye
    
    exit
    
    quit
## 上传下载
    ftp> put /path/readme.txt # 上传 readme.txt 文件
    ftp> mput *.txt      # 可以上传多个文件
    
    ftp> get readme.txt # 下载 readme.txt 文件
    ftp> mget *.txt   # 下载
    
win10上传下载 命令同Linux
    
    cmd
    ftp 10.1.1.20
    
    
## 状态码
    •230 - 登录成功
    •200 - 命令执行成功
    •150 - 文件状态正常，开启数据连接端口
    •250 - 目录切换操作完成
    •226 - 关闭数据连接端口，请求的文件操作成功 
# 参考
1. [Linux中ftp的常用命令](https://www.cnblogs.com/feiquan/p/9236768.html)
2. 1. [Ubuntu安装ftp服务](https://blog.csdn.net/Klein_yang/article/details/84954958)