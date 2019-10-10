---
title: ssh远程登录
date: 2019-09-29 12:13:36
categories:
- Linux
tags:
- ssh

---

# ssh简介
**S**ecure **Sh**ell(SSH) 可以把所有传输的数据进行加密，也能够防止DNS欺骗和IP欺骗。还有一个额外的好处就是传输的数据是经过压缩的，所以可以加快传输的速度。目前已经成为Linux系统的标准配置。

SSH之所以能够保证安全，原因在于它采用了非对称加密技术(**RSA**)加密了所有传输的数据。

# 安装

	sudo apt-get install openssh-client #本地主机运行此条，实际上通常是默认安装client端程序的
	sudo apt-get install openssh-server #服务器运行此条命令安装
	dpkg -l | grep ssh    #查看是否安装对应包
# 启动
	sudo /etc/init.d/ssh start    #打开服务器服务  关闭 stop
	ps -e | grep ssh    #可以查看服务是否启动
# 登录
	ssh 服务器端登录用户名@服务器ip地址  #客户端登录  特别注意用户名要正确
	ssh-keygen -t rsa      #生成公钥私钥
	ssh-copy-id server@10.1.1.20 #把公钥上传服务器 免密登录  
	ssh-copy-id server@10.1.1.20 -i copyed_file_name  #指定复制的文件名  
	
# 同一台电脑配置github gitlab的SSH
1. 生成密钥文件
        
        ssh-keygen -t rsa -C '1253156159@qq.com' // GitLab
        // Enter file in which to save the key (/Users/tomatoro/.ssh/id_rsa): gitlab_ssh
        
        ssh-keygen -t rsa -C 'a1253156159@gmail.com' // GitHub
        // Enter file in which to save the key (/Users/tomatoro/.ssh/id_rsa): github_ssh
2. 进入到.ssh文件下,找到gitlab_ssh.pub 和 github_ssh.pub 将里面的内容全部复制粘贴到github 和 gitlab 的SSHKEY上
3. 存储本地key
        ssh-agent -s
        ssh-add ~/.ssh/id_rsa_github    // 输入生成秘钥时设置的密码
        ssh-add ~/.ssh/id_rsa_gitlab    // 输入生成秘钥时设置的密码
4. .ssh下创建config 文件
        
        Host github.com
          HostName github.com
          User a1253156159@gmail.com    # 登录的邮箱　用户名
          PreferredAuthentications publickey
          IdentityFile ~/.ssh/github_ssh
        
        Host 10.1.1.20
          HostName 10.1.1.20
          User 1253156159@qq.com
          PreferredAuthentications publickey
          IdentityFile ~/.ssh/gitlab_ssh
          Port 80
        
        Host 10.1.1.20
          HostName 10.1.1.20
          User yang@yang-cp
          PreferredAuthentications publickey
          IdentityFile ~/.ssh/gitlab_ssh
          Port 22
5. 因为在自己的服务器上搭建的局域网gitlab　所以服务器登录和gitlab登录的ip一样，导致不能登录服务器
    nmap -sT -O 10.1.1.20           //查看ssh端口　默认是22
    ssh server@10.1.1.20 -p 22      //指定端口就可以了
            
        
    

# 问题
ssh无秘钥登录报错sign_and_send_pubkey: signing failed: agent refused operation。
表示ssh-agent 已经在运行了，但是找不到附加的任何keys，就是说你生成的key，没有附加到ssh-agent上，需要附加一下，执行    

    eval "$(ssh-agent -s)"
	ssh-add  


# 参考

1. [SSH简介及两种远程登录的方法](https://blog.csdn.net/li528405176/article/details/82810342)
2. [如何在一台设备上同时配置github和gitlab的SSH](https://segmentfault.com/a/1190000020010343)
