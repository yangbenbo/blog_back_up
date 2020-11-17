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
	whereis gcc   #查看gcc安装路径 只是寻找系统中某些特定目录 
	whereis -l    #查看寻找的特定目录
	
	which   gcc   #查看gcc运行路径
查找命令	

	locate -i frankaconfig  # 不区分大小写 匹配 *frankaconfig*
	find / -iname '*frankaconfig*'  #同上,区分大小写则参数是-name 不要i(case-insensitive)
	
	locate -l 5 passwd  #显示5行结果 通过数据库查找 使用updatedb 更新
	
	find / -mtime +4    # 5天前的文件 0表示当前到24小时之前 
	find / -mtime 4     # 前4-5天的文件 
	find / -mtime -4    # 4天内的文件 
	# 对find找到的内容再利用 {}为找到的内容 -exec 到\;是关键词 代表find的额外动作
	find / -mtime -4 -exec ls -l {} \;
	
	find /home -user yang   #属于yang的文件
	find / -nouser          #不属于任何人的文件 
    #那个 -a 是 and 的意思,为符合两者才算成功 ubuntu下 -exec 会有点不一样
	find /etc -size +50k -a -size -60k -exec ls -l {} \;
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
## 卸载软件
    dpkg -l    #列表显示安装的软件 配合grep 过滤
    dpkg -r packagename    
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
        
        # man page 是呼叫less来显示说明的 指令同less
        空格  向下翻页
        PgDn  向下
        PgUp  向上
        /string   向下查找string字符串
        ?string  向上查找
        n、N  继续下/上一个查找
        g G   前进到第一/最后一行  

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
        sudo apt install python3-d
        ev python3-pip python3-setuptools
        sudo pip3 install thefuck
        
        # .bashrc 添加如下内容
        eval $(thefuck --alias)
        # You can use whatever you want as an alias, like for Mondays:
        eval $(thefuck --alias FUCK)

# 时间
- modification time (mtime):
当该文件的『内容数据』变更时,就会更新这个时间!内容数据指的是文件的内容,而不是文件的属性或
权限
- status time (ctime):
当该文件的『状态 (status)』改变时,就会更新这个时间,举例来说,像是权限与属性被更改了,都会更新
这个时间啊。
- access time (atime):
当『该文件的内容被取用』时,就会更新这个读取时间 (access)。举例来说,我们使用 cat 去读取
/etc/man_db.conf , 就会更新该文件的 atime 了。 
    
        # 查看文件的各个时间  ls 默认mtime
        date; ls -l /etc/man_db.conf ; ls -l --time=atime /etc/man_db.conf ; \
        > ls -l --time=ctime /etc/man_db.conf
        
        touch -t 201406150202 bashrc    # 修改时间
touch可以方便修改时间,但是复制的时候即使复制所有属性,但是没办法复制ctime  


# 磁盘管理
    
    df -aT  # 查看所有特殊文件格式和名称
    du -sb  # 查看当前目录下有多少bytes
系统里面其实还有很多特殊的文件系统存在的。那些比较特殊的文件系统几乎都是在内存当中

1. /proc 的东西都是 Linux 系统所需要加载的系统数据,而且是挂载在『内存当中』
的, 没有占任何的磁盘空间 

2. /dev/shm/ 目录,其实是利用内存虚拟出来的磁盘空间,通常是总物理内存的一半     

     lsblk  #列出系统上的所有磁盘列表 
     sudo blkid #列出装置的 UUID 等参数  
     sudo parted /dev/sdb print #列出磁盘的分区表类型与分区信息

## 磁盘分区
磁盘分区: gdisk/fdisk/parted 不要处理正在活动文件系统
     
MBR 分区使用 fdisk 分区, GPT 分区使用 gdisk 分区 parted两个都适用

分区过程中 Last sector 后面只需要 : +1G 就可以 (增加的容量) 

**使用的『装置文件名』请不要加上数字,因为 partition 是针对『整个磁盘装置』而不是某个 partition** 
    
    cat /proc/partitions    #查看分区信息
    partprobe -s    #更新核心的分区表信息
    
    mkfs.ext4 /dev/sda6  #ext4文件系统 格式化
    
    mount -o remount,rw,auto /  #将/ 重新挂载 并加入rw与auto参数
    mount --bind /var /data/var #将某个目录挂载到其他目录
在配置 /etc/fstab 文件时中文件系统参数使用default就可以 包含rw, suid, dev, exec, auto, nouser, async

# 网络设置
查看网络信息
        
        nmcli connection show   # 后面可接特定网卡名称如eth0
        nmcli connection modify eth0 connection.autoconnect yes ipv4.method auto
        nmcli connection up eth0    #启动
        
        hostnamectl     #主机名
        hostnamectl set-hostname yangbenbo
        
        timedatectl # 时间
        
        localectl set-locale LANG=en_US.utf8    #设置语系
        
        dmidecode   #查看硬件设
        
        smartctl -a /dev/sda    #查看磁盘状态

# 守护进程
ps 与 top 来观察程序时,都会发现到很多的 {xxx}d 的程序, 通常那就是一些daemon(守护程序)

将目前工作放在**bash后台** 程序后增加 & 或者 ctrl+z

        tar -zpcvf /tmp/etc.tar.gz /etc > /tmp/log.txt 2>&1 &   #错误和标准输出到文件 避免影响terminal
        jobs    #列出后台进程
        fg      #将后台工作拿到当前terminal
        fg jobnumber
vim 的工作无法被结束喔!因为他无法透过 kill 正常终止 

kill 后面直接加数字(pid)与加上 %number (后台工作号) 的情况是不同的
   
        kill -l     #查看常用signal  -1 启动被终止的进程 -9 强制删除一个不正常的工作  -15 默认值 正常结束一项工作 
        kill %2     # 结束后台2号进程
注销系统还能运行 at nohup
        
动态查看进程变化 load average 分别是1 5 15分钟cpu负载 0-1 /cpu个数
        
        top     #按下1 可查看不同cpu的负载  
        pstree  #查看进程依赖   
        
# 杀死僵尸程序
    top  # 查看是否有僵尸进程 zombie
    ps -A -ostat,ppid,pid,cmd | grep -e '^[zZ]'        
    sudo kill -9 ppid(父进程pid)
                            
    
# 其他
	uname    #查看当前系统信息 包括内核
	ctrl + L #在资源管理器中显示绝对路径
	su       #切换用户  sudo su
	who      #查看当前登录用户
	df -t ext4     #查看磁盘使用情况
	dd if=xxx of=xxx    #拷贝 少用
	startup  #搜索　软件开启自启动
	file filename   # 查看文件类型  二进制 可执行
	cd ~   # home 目录
	cd ~yang    # 杨的home目录
	cd -    # 上一个目录
	\rm -r /tmp/etc     # 加上反斜杠可以忽略alias的指定选项
	basename /etc/sysconfig/network # 取得文档名
	dirname /etc/sysconfig/network  # 取得目录名字
	nl /etc/issue   # 带行号显示文件内容 也可以使用cat -n
	echo password | od -t oCc   #找到 password 这几个字的 ASCII 对照
	file filename # 查看文件类型 ASCII data binary
	rmdir   #仅能删除空目录
	
	
- ubuntu 4个工作区 和win10的多桌面一样 ctrl+alt+方向箭头 切换 setting->appearance->behavior->enable workspaces

  可以在应用：键盘　设置快捷键　默认的是ctrl+alt+方向键
  
  - shift+1 切换到左边工作区　ctrl+shift+1 把当前窗口移动到左边工作区 
  - shift+2 切换到左边工作区　ctrl+shift+2 把当前窗口移动到右边工作区 
  
  切换程序到不同工作区　快捷键或者鼠标拖动就可以

- deb文件包含二进制文件、库文件、配置文件、帮助文档
ubuntu软件包：二进制包 (Binary Packages)   源码包(Source Packages)
Redhat Linux  -> RPM包    ubunut -> Deb包	
	
	
	
	
# 问题
1. 更新linux时候提示无法“由于没有公钥，无法验证下列签名 ***”的解决方案

      sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 6AF0E1940624A220 #此处6AF0E1940624A220需要是错误提示的key
2. ctrl+alt  会调出窗口最大最小化之类的窗口(其实就是　激活当前窗口菜单的快捷键)　使得这类快捷键无法使用 
快捷键窗口找相关快捷键删除 　激活当前窗口菜单冲突了　改成默认的alt+空格就行了

3. qt无法输入中文
    ubuntu16安装搜狗输入法,启动fcitx之后qtcreator无法输入中文,原因是缺少fcitx的支持库**libfcitxplatforminputcontextplugin.so**.
    - 查找库是否安装
        
            dpkg -L fcitx-frontend-qt5 | grep .so
            sudo apt-get install fcitx-frontend-qt5   #若没有则安装
    - 拷贝到qt插件库中
            
            # 我是用命令行安装qt的,安装目录可能和安装包安装不一样
            sudo cp libfcitxplatforminputcontextplugin.so /opt/qt59/plugins/platforminputcontexts 
        /opt 给第三方协力软件放置的目录.比如chrome就在这里.不确认安装目录可以查找
            
            sudo find / -name '*platforminputcontexts*'   
    - 重启qtcreator            
                           
	
# 引用
1. [linux下查看软件安装路径](https://blog.csdn.net/liufuchun111/article/details/80402109)
2. [更新linux时候提示无法“由于没有公钥，无法验证下列签名 ***”的解决方案](https://blog.csdn.net/loovejava/article/details/21837935)
3. [Ubuntu ctrl+alt会导致窗口还原的问题](https://www.cnblogs.com/stono/p/7105083.html)
4. [github the fuck](https://github.com/nvbn/thefuck)
5. [github tldr](https://github.com/lord63/tldr.py)
6. [理解Linux系统负荷](https://www.ruanyifeng.com/blog/2011/07/linux_load_average_explained.html)
7. [Ubuntu下Qtcreator无法输入中文的解决办法](https://blog.csdn.net/baidu_33850454/article/details/81212026?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2)
8. [Ubuntu查找和杀死僵尸进程](https://blog.csdn.net/wzy_1988/article/details/16944789)