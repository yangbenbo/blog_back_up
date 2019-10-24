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
	P   #sort by cpu
	M  #sort by memory
	
	cron  at #周期任务和临时任务安排
# 网络
	nmap -sn 10.1.1.1-255  # 扫描局域网ip
	nmap -sT -O 10.1.1.20     #扫描主机网络端口 和操作系统  -sT是默认 可以不用
	namp -p1-5000 10.1.1.20 #扫描1-5000端口
	
# 软件安装路径
	whereis gcc   #查看gcc安装路径
	which   gcc  #查看gcc运行路径
rpm

	rpm -ql gcc        #查看gcc相关文件的安装路径
	rpm -qa | grep gcc #查看有没有安装gcc
	rpm -qa            #查看全部使用rpm安装的软件
	
	sudo rpm -ivh xxx.rpm   # install  -e  (to remove)
deb

    dpkg -L gcc         #查看gcc相关文件的安装路径
    dpkg -l | grep gcc  #查看有没有安装gcc
    dpkg -l             #查看全部安装包
    
    dpkg -i xxx.deb     #install   --remove
    
# 获取命令
    whatis uname    #一行介绍性文字
    apropos search  #想搜索一个文件又忘了命令  
    
# 文件压缩与归档

    gzip xxx    #压缩工具
    bzip xxx    #更高效的压缩工具
    #c:create f:指定归档文件名 v:显示执行过程  提取c -> x
    # czvf 会在归档之后调用gzip  cjf 调用bzip  短划线- 都是可以省略的
    tar -cvf shell.tar shell/   
    
    dump restore    # 增量备份和回复 但是备份不会注意删除的文件，有可能恢复了删除的
    cron            # 计划任务配合，自动备份          
# 查看帮助
1. --help
2. man page       // man ls    1代表一般账号可用命令，8代表管理员常用命令，5代表系统配置文件
        
        空格  向下翻页
        PgDn  向下
        PgUp  向上
        /string   向下查找string字符串
        ?string  向上查找
        n、N  继续下/上一个查找

3. info page   可读性高
    
        空格  向下翻页
        PgDn  向下
        PgUp  向上
        Tab    在节点之间移动，节点通常以*表示
        Enter    进入节点
        b    光标移动到info界面第一处
        e    光标移动到info界面最后一个节点
        n    上一个节点
        p     下一个节点
        u    向上移动一层
        s(/)    查找
        h,?    显示帮助
4. 数据同步写入磁盘 sync
虽然目前的shutdown reboot halt在关机前执行了sync，但是多执行几次确保

        
# 高效工具
1. **tldr** **cheat**

    这两个是很好的命令备忘录　相比man 会简洁很多
    安装tldr   (Too long, Don't read)
    
        sudo npm install -g tldr
        tldr ls   #　查看ls 常用命令使用
    安装cheat

        # [对于 Python2]
        sudo apt install python-pip python-setuptools
        # [对于 Python3]
        sudo apt install python3-pip
        
        sudo pip install cheat
        cheat ls  #　查看ls 常用命令使用
2. the fuck
    
    命令行自动纠错工具

        sudo apt update
        sudo apt install python3-dev python3-pip python3-setuptools
        sudo pip3 install thefuck
        
        # .bashrc 添加如下内容
        eval $(thefuck --alias)
        # You can use whatever you want as an alias, like for Mondays:
        eval $(thefuck --alias FUCK)
            
# 其他
	uname    #查看当前系统信息 包括内核
	ctrl + L #在资源管理器中显示绝对路径
	su       #切换用户  sudo su
	who      #查看当前登录用户
	df -t ext4     #查看磁盘使用情况
	dd if=xxx of=xxx    #拷贝 少用
	startup  //搜索　软件开启自启动
	
	
- ubuntu 4个工作区 和win10的多桌面一样 ctrl+alt+方向箭头 切换 setting->appearance->behavior->enable workspaces

  可以在应用：键盘　设置快捷键　默认的是ctrl+alt+方向键
  
  - shift+1 切换到左边工作区　ctrl+shift+1 把当前窗口移动到左边工作区 
  - shift+2 切换到左边工作区　ctrl+shift+2 把当前窗口移动到右边工作区 
  
  切换程序到不同工作区　快捷键或者鼠标拖动就可以

- deb文件包含二进制文件、库文件、配置文件、帮助文档
ubuntu软件包：二进制包 (Binary Packages)   源码包(Source Packages)
Redhat Linux  -> RPM包    ubunut -> Deb包	
	
	
	
	
# 问题
- 更新linux时候提示无法“由于没有公钥，无法验证下列签名 ***”的解决方案

      sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 6AF0E1940624A220 #此处6AF0E1940624A220需要是错误提示的key
- ctrl+alt  会调出窗口最大最小化之类的窗口(其实就是　激活当前窗口菜单的快捷键)　使得这类快捷键无法使用 
快捷键窗口找相关快捷键删除 　激活当前窗口菜单冲突了　改成默认的alt+空格就行了       
	
# 引用
1. [linux下查看软件安装路径](https://blog.csdn.net/liufuchun111/article/details/80402109)
2. [更新linux时候提示无法“由于没有公钥，无法验证下列签名 ***”的解决方案](https://blog.csdn.net/loovejava/article/details/21837935)
3. [Ubuntu ctrl+alt会导致窗口还原的问题](https://www.cnblogs.com/stono/p/7105083.html)
4. [github the fuck](https://github.com/nvbn/thefuck)
4. [github tldr](https://github.com/lord63/tldr.py)