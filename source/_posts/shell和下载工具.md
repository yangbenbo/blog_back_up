---
title: shell和下载工具
date: 2019-09-29 12:27:00
categories:
- Linux
tags:
- shell
- 下载工具

---
# SHELL
shell 是一个命令解释器 而不是一门编程语言
**空格**用来分隔命令和传递给它的参数(或用来分隔命令的两个参数)

~/.bash_history 记录的是前一次登入以前所执行过的指令, 而至于这一次登入所执行的指令都被暂
存在内存中,当你成功的注销系统后,该指令记忆才会记录到 .bash_history 当中
# SHELL常用符号
	echo $SHELL      
	env |grep SHELL
	sh  #可进入shell    
1. \* 通用符号，可以表示任意一个字符（包括空字符）或多个字符组成的字符串  ls -l /bin/e*
2. ? 类似*，但是只能表示单个字符
3. [] 制定被显示内容的范围 ls [a-c] 显示a 、b 、c文件夹
4. ！排除符号 用来指定被屏蔽的部分，需要与[]一起使用  ls [!a-c]  不显示a、b、c文件夹
5. ; 分隔符号，多个命令分隔  ls;ls -l
6. ` 命令替代符，成对出现，包含的内容在shell中表示一条命令，并且会被执行，这不是单引号 echo `ls -l`  将命令的结果显示出来 
7. # 注释符号
# 进阶
1. 别名
		# 事实上 .bashrc 是一个shell脚本文件，在用户登录到系统自动运行，可以把想让系统启动时运行的任务写进去
		alias hs='hexo clean && hexo g && hexo s'  # 注意等号前后无空格  别名相当与快捷键
		

2. 重定向 > 输入（覆盖）  >> 输入追加  < 输出  << 立即文档(here document)

		ls -l > test.txt  # 把ls 的结果写入test.txt中
		cat << EOF      #告诉Shell从键盘接受输入 EOF作为结束，然后查看输入内容

3. 管道  "|"

		ls -l |grep test
4. 网络
    
        nmap -sn 10.1.1.1-255   #扫描局域网		

# shell编程
    
## 运行shell脚本
1. ./test.sh
    
        #! /bin/bash   # 告诉系统解释此脚本文件的shell程序
2. /bin/bash test.sh   #这种方式不需要在第一行指定解释器　写了也没用
## 变量
- 局部变量
- 环境变量
- shell变量 由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量

等号两边不能有空格

    your_name="runoob.com"   
    echo $your_name
    echo ${your_name}　　　# 最好加上括号
    echo $$         # $本身也是变量 代表当前程序的pid
    
    myUrl="http://www.google.com"
    readonly myUrl      # 只读变量
    
    unset myUrl         #　删除变量
## 字符串
### 引号规则
1. "" 阻止shell对大多数特殊字符(例如#)进行解释　但是$ ` "　仍然保持其特殊含义
2. '' 阻止shell对所有字符进行解释 所有字符都会原样输出

      单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。
3. `` 倒引号括起来一个shell　命令时，这个命令将会执行　结果作为这个表达式的值    

字符串变量最好加上"" 清晰度
### 常用命令 
1. 字符串拼接
    
        your_name="runoob"
        # 使用双引号拼接
        greeting="hello, "$your_name" !"
        greeting_1="hello, ${your_name} !"
        echo $greeting  $greeting_1
        # 使用单引号拼接
        greeting_2='hello, '$your_name' !'      #　注意　单引号拼接　
        greeting_3='hello, ${your_name} !'
        echo $greeting_2  $greeting_3    

2. 获取字符串长度

        string="abcd"
        echo $(#string)  # 输出４
3. 提取子字符串
    
        string="runoob is a great site"
        echo ${string:1:4} # 输出 unoo
4. 查找子字符串

        string="runoob is a great site"
        echo `expr index "$string" io`  # 输出 4  查找字符　i || o 位置
        
## 数组
    array_name=(value0 value1 value2 value3)   
    echo ${array_name[@]}   # 获取数组中所有元素　@  
    
    length=$(#arrar_name[@])    # 数组元素个数
    length=$(#arrar_name[*])    # 数组元素个数
    length=$(#arrar_name[n])    #　单个元素长度
    
## 注释
    
    # 单行注释　　可以把一块代码定义成函数　达到多行注释
    
    # 多行注释
    :<<EOF
    注释内容...
    注释内容...
    注释内容...
    EOF 
    
    # EOF 可以使用其他符号
    :<<!
    注释内容...
    注释内容...
    注释内容...
    !  
## 参数传递
     
     $0　脚本自己的名字
     $1　第一个参数
     $2　第二个参数
     ...
     ${13} 第13个参数
     
     $# 参数的个数
     $* 以一个单字符串显示所有向脚本传递的参数。
        如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
     $@ 与$*相同，但是使用时加引号，并在引号中返回每个参数。
        如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
$* 和 $@区别:
- 相同点：都是引用所有参数。
- 不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。
        
## 基本运算
计算表达式的值

    num=$[ $num + 1 ]
    expr 1 + 2
    let num=$num+1  # 用于计算整数表达式的值

    val=`expr $a \* $b`   #等号两边没有空格 乘法
    [ $a == $b ]  # 注意有空格
字符串运算符
    
    a="abc"
    [ -z $a ]   #检测字符串长度是否为0 是 true
    [ -n "$a" ] #字符串长度是否为0 不为0 true
    [ $a ]      #检测字符串是否非空 不为孔 true  
注意:**判断非空是用了双引号**    
**字符串比较时 == 可以用 = 替代**    
文件测试运算符
![文件测试](文件测试.png)   


### test命令和[
if语句不执行任何判断 实际接收一个程序名作为参数,依据程序返回值判断执行相应的语句
    
    if ./testscript 0   ## 如果返回值为0
if通常两种方式

    if test "$password" = "join"      
    if [ "$password" = "join" ]
while有时可以不用test
        
    while test $number -le 100
    
    while read n    # 每条指令都有返回值 可以利用 read 读取到键盘输入返回0 ctrl+D出入文件结束符则返回非0 通常是1
for语句 

    for i in 1 2 3
    for i in `seq 3`    # 1 2 3 注意是反引号                 
1. 字符串比较
2. 文件测试
3. 数字比较 只能比较整数
## 函数
 	
$? 获得上一条语句返回值 

- 如果函数没有明确指定返回值 则是最后一条语句运算结果 0 为true 1 为false 

- 仅对其上一条指令负责，一旦函数返回后其返回值没有立即保存入参数，那么其返回值将不再能通过 $? 获得。
 
## 重定向
一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：

- 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
- 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
- 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

        command 2 > file    # 错误文件重定向到file
        command > file 2>&1 # stdout和stderr合并到file 放在>后面的&，表示重定向的目标不是一个文件，而是一个文件描述符
### 即时脚本 here document
基本形式
        
        command << delimiter
            document
        delimiter
        
例子

        $ wc -l << EOF
            欢迎来到
            菜鸟教程
            www.runoob.com
        EOF 
### 禁止输出
如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：
    
    command > /dev/null
    command > /dev/null 2>&1
    
    
## 文件包含
两种方式 都不需要执行权限就可以

    . filename   # 注意点号(.)和文件名中间有一空格
    
    source filename   # 这种方式也可以
source命令可以让一个脚本强行影响其父shell环境
export命令可以让脚本影响其子shell 
    export EDITOR='gedit'      

运行一个shell脚本时会启动另一个命令解释器。
每个shell脚本有效地运行在父shell(parent shell)的一个子进程里。 
这个父shell是指在一个控制终端或在一个xterm窗口中给你命令指示符的进程。
    
                   
## 读取用户输入
1. read接受变量名为参数 从标准输入接收到的信息储存 如果没有提供变量 则储存在**REPLY**
2. 用作输出一段内容后暂停
        
        echo "press enter to continue"
        read
## exit trap
exit 返回信号
trap 捕捉信号
    trap 'echo "goodbye"; exit' EXIT    # 捕捉退出        
## 常用命令
1. cut 指定的方式分割行
2. diff 比较文件差异
3. sort 按照字母排序(不改变源文件)
4. uniq 对**已经排好序**的输入删除重复的行
5. tr   按照制定方式对字符进行替换
6. wc   (word counts) 统计文中字节 单词 行的数量
7. substr 从字符中提取一部分 必须搭配expr进行求值
        
        expr substr "hello world" 1 5   # hello  起始位置从1开始算
8. seq 产生一个整数序列 

# bash
1. type cd 查询是否为内建命令 
2. 常用组合键

        [ctrl]+u/[ctrl]+k 从光标处向前删除指令串  及向后删除指令串 
        [ctrl]+a/[ctrl]+e 让光标移动到整个指令串的最前面  或最后面  home end也可以
          

# 下载工具
- apt
- wget
- Transmission   #种子下载

		wget  -r -np -nd http://10.143.2.9/packages/      递归下载 不遍历父目录  不在本地重新创建目录
		wget  -r -np -nd --accept=cpp http://10.143.2.9/packages/      递归下载 不遍历父目录  不在本地重新创建目录 扩展名为cpp的文件
		wget -i filename.txt      URL地址放在filename.txt文件，自动下载所有文件
		wget -c  ...  断点续传



# 引用
1. [Shell 教程](https://www.runoob.com/linux/linux-shell.html)