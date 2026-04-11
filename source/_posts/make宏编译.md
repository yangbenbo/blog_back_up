---
title: make宏编译
date: 2019-11-02 22:32:22
categories:
- Linux
tags:
- make
---

- 简化编译时所需要下达的指令;
- 若在编译完成之后,修改了某个原始码文件,
则 make 仅会针对被修改了的文件进行编译,其他的 object file不会被更动;
- 最后可以依照相依性来更新 (update) 执行档。

# 语法变量
    目标(target): 目标文件 1 目标文件 2
    <tab>
    gcc -o 欲建立的执行文件 目标文件 1 目标文件 2
- 在 makefile 当中的 # 代表批注;
- <tab> 需要在命令行 (例如 gcc 这个编译程序指令) 的第一个字符;
- 目标 (target) 与相依文件(就是目标文件)之间需以『:』隔开。 

makefile 示例 两个target 
**$@:代表目前的目标(target)**

        main: main.o haha.o sin_value.o cos_value.o
            gcc -o main main.o haha.o sin_value.o cos_value.o -lm
        clean:
            rm -f main main.o haha.o sin_value.o cos_value.o 
        
        # 简化重复内容之后如下
        # $@:代表目前的目标(target)
        LIBS = -lm
        OBJS = main.o haha.o sin_value.o cos_value.o
        main: ${OBJS}
            gcc -o $@ ${OBJS} ${LIBS}
        clean:
            rm -f main ${OBJS}  
调用

        make  (make main)   #调用main对象
        make clean          #调用clean
        make clean main     #先clean 再main
        
## 基本语法
1. 变量与变量内容以『=』隔开,同时两边可以具有空格;
2. 变量左边不可以有 <tab> ,例如上面范例的第一行 LIBS 左边不可以是 <tab>;
3. 变量与变量内容在『=』两边不能具有『:』
;
4. 在习惯上,变数最好是以『大写字母』为主;
5. 运用变量时,以 ${变量} 或 $(变量) 使用;
6. 在该 shell 的环境变量是可以被套用的,例如提到的 CFLAGS 这个变数!
7. 在指令列模式也可以给予变量。 
## 变量
gcc 在进行编译的行为时,会主动的去读取 CFLAGS 这个环境变量,所以,你可以直接在 shell
定义出这个环境变量,也可以在 makefile 文件里面去定义,更可以在指令列当中给予

    CFLAGS="-Wall" make clean main      #指令中指定 或者在makefile中定义变量
环境变量取用规则
1. make 指令列后面加上的环境变量为优先;
2. makefile 里面指定的环境变量第二;
3. shell 原本具有的环境变量第三。    
           
# 源码安装
1. diff  行对比
        
        diff -Naur passwd.old passwd.new > passwd.patch    # 生成补丁文件
        diff /etc/rc0.d/ /etc/rc5.d/                       # 比较布姆下相同文件名
                    
2. patch
    
    用来更新源码安装的源文件,使用规则
    
        patch -p 数字 < patch_file
        patch -R  -p 数字 < patch_file    #回复旧文件
    patch_file 第一行写的是这样:

        *** /home/guest/example/expatch.old
    
    『patch -p0 < patch_file 』时,则更新的文件是『 /home/guest/example/expatch.old 』,
    
    『patch -p1 < patch_file』,则更新的文件为『home/guest/example/expatch.old』,
    
    『patch -p4 < patch_file』则更新『expatch.old』
    
    **-pxx 那个 xx 代表『拿掉几个斜线(/)』的意思!**  
3. cmp 字节对比 可以用来对比binary             
# 动态函数库.so  静态函数库.a
将常用到的动态函式库先加载内存当中 (快取, cache),需要 ldconfig 与 /etc/ld.so.conf协助
1. 首先,我们必须要在 /etc/ld.so.conf 里面写下『 想要读入高速缓存当中的动态函式库所在的目录』,注意喔, 是目录而不是文件;
2. 接下来则是利用 ldconfig 这个执行档将 /etc/ld.so.conf 的资料读入快取当中;
3. 同时也将数据记录一份在 /etc/ld.so.cache 这个文件当中
    
        ldconfig [-f conf] [ -C cache]
        ldconfig [-p]       #列出目前有的所有函式库资料内容 (在 /etc/ld.so.cache 内的资料!)
    将动态函式库所在的目录名称写入
    /etc/ld.so.conf.d/yourfile.conf 当中,然后执行 ldconfig 就可以  

4. 查看可执行binary文件含有的动态函数库

        ldd [-vdr] [filename]      
# 检查软件正确性
目前广泛应用的加密机制 MD5 SHA1 SHA256 计算文件的指纹码

md5sum/sha1sum/sha356sum
    
    md5sum/sha1sum/sha256sum [-bct] filename
可以把Linux上的重要文件进行指纹数据库的建立,然后定期以shell script的方式检查    
# 安装问题
1. 缺少依赖库
    查看文件夹下对应的config.log 里面有对应的debug信息
2. 没有那个文件或目录  X11/Intrinsic.h
        
        yum provides */Intrinsic.h   # 使用了正则表达式 需要头文件属于哪个包 安装就好            
# 引用
1. [鸟哥Linux基础](http://linux.vbird.org/new_linux.php)   
2. [xpenguins 的安装（问题来源于鸟哥基础篇）](https://blog.csdn.net/king_on/article/details/7799695)        