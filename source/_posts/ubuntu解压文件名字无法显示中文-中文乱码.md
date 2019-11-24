---
title: ubuntu解压文件名字无法显示中文(中文乱码)
date: 2019-11-23 11:23:52
categories:
- Linux
tags:
- 编码
---
1. zip解压文件名乱码
    
    由于zip格式中并没有指定编码格式，Windows下生成的zip文件中的编码是GBK/GB2312等。因此，导致这些zip文件在Linux下解压时出现乱码问题，因为Linux下的默认编码是UTF8。

        unzip -O gbk xxx.zip        
        unzip -O gbk xxx.zip -d xxx  ## 解压到指定文件夹    
2. gedit 文件内容乱码 
    
        gsettings set org.gnome.gedit.preferences.encodings candidate-encodings "['UTF-8','CURRENT','GB18030','ISO-8859-15','UTF-16']"
3. vim 中文乱码
        
        # 建议修改本地配置文件
        sudo gedit /etc/vim/vimrc   # 系统配置文件
        sudo gedit .vimrc   # 本地配置文件
        
        # 增加下面内容
        set fileencodings=utf-8,gb2312,gbk,gb18030
        
        set termencoding=utf-8
        
        set encoding=prc        

# 引用
1. [ubuntu 16.4解压中文名文件夹，中文乱码](https://blog.csdn.net/weixin_40009624/article/details/79019907)      
2. [Ubuntu 16.04 解压缩zip文件中文乱码](https://blog.csdn.net/w7619370/article/details/53024281)    
