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
	ssh 服务器用户名@服务器ip地址  #客户端登录  特别注意用户名要正确
	ssh-keygen -t rsa      #生成公钥私钥
	ssh-copy-id server@10.1.1.20 #把公钥上传服务器 免密登录
# 问题
ssh无秘钥登录报错sign_and_send_pubkey: signing failed: agent refused operation。
表示ssh-agent 已经在运行了，但是找不到附加的任何keys，就是说你生成的key，没有附加到ssh-agent上，需要附加一下，执行    

	ssh-add   #可能需要先执行eval "$(ssh-agent -s)"

# 参考

[SSH简介及两种远程登录的方法](https://blog.csdn.net/li528405176/article/details/82810342)