---
title: shell和下载工具
date: 2019-09-29 12:27:00
categories:
- Linux
tags:
- shell
- 下载工具

---

# SHELL常用符号
	echo $SHELL      
	env |grep SHELL
	sh  #可进入shell    
1. *  通用符号，可以表示任意一个字符（包括空字符）或多个字符组成的字符串  ls -l /bin/e*
2. ? 类似×，但是只能表示单个字符
3. [] 制定被显示内容的范围 ls [a-c] 显示a 、b 、c文件夹
4. ！排除符号 用来指定被屏蔽的部分，需要与[]一起使用  ls [!a-c]  不显示a、b、c文件夹
5. ; 分隔符号，多个命令分隔  ls;ls -l
6. ` 命令替代符，成对出现，包含的内容在shell中表示一条命令，并且会被执行，这不是单引号 echo `ls -l`  将命令的结果显示出来 
7. # 注释符号
# 进阶
1. 别名
		
		alias hs='hexo clean && hexo g && hexo s'  # 注意等号前后无空格  别名相当与快捷键

2. 重定向 > 输入（覆盖）  >> 输入追加  < 输出

		ls -l > test.txt  # 把ls 的结果写入test.txt中


3. 管道  "|"

		ls -l |grep test

# 下载工具
- apt
- wget
- Transmission   #种子下载

		wget  -r -np -nd http://10.143.2.9/packages/      递归下载 不遍历父目录  不在本地重新创建目录
		wget  -r -np -nd --accept=cpp http://10.143.2.9/packages/      递归下载 不遍历父目录  不在本地重新创建目录 扩展名为cpp的文件
		wget -i filename.txt      URL地址放在filename.txt文件，自动下载所有文件
		wget -c  ...  断点续传



# 注释符号