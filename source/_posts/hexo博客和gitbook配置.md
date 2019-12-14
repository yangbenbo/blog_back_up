---
title: hexo博客和gitbook配置
date: 2019-10-06 08:36:02
categories:
- Linux
tags:
- hexo
- blog
- gitbook

---
环境：ubuntu16.04
# gitbook简介
创作和管理电子书，是目前最流行的开源书籍写作方案
gitbook基于Node.js 开发，需要通过 Node.js 包管理工具 NPM 安装

简单来说Node.js就是运行在服务端的JavaScript
npm 常用命令

    npm update <name>   # 更新指定包
    npm install npm -g  # 全局更新自己 
    npm view <package> version  #查看包版本号
## 环境配置

- nodejs
- gitbook

        sudo apt-get install nodejs   # 可能还需要安装nodejs-legacy 这是因为需要node 但是最好直接使用node升级 文章末尾
        sudo apt-get install npm
        sudo npm install gitbook-cli -g   #全局安装
        # 可能需要安装 ebook-convert    Binary install或者Source install
        sudo -v && wget -nv -O- https://download.calibre-ebook.com/linux-installer.sh | sudo sh /dev/stdin
## 常用命令
    gitbook init    #初始化 生成 README.md SUMMARY.md
    gitbook serve   #启动服务 浏览器打开 http://localhost:4000/

## 定义目录结构
1. 先定义好结构，然后通过gitbook init自动生成目录结构对应的文件夹和Markdown文件
2. 先创建好文件夹和Markdown文件再来编辑目录结构

        # Summary
        
        * [项目简介](README.md)
        * [快速开始](docs/快速开始.md)
         * [环境搭建](docs/环境搭建.md)
         * [简单使用](docs/简单使用.md)
        * [学入学习](docs/深入学习) 
        
# hexo博客搭建

    sudo apt install nodejs
    sudo apt install npm
    
    sudo npm install n -g
    sudo n stable
    
    sudo npm install hexo-cli
## 引用自己的文章

    {% post_link 文章文件名（不要后缀） 文章标题（可选） %}    
    
## 踩过的坑
1. hexo用着用着报错，提示node版本太低

    升级node：
        
        node -v     #查看当前版本
        sudo npm cache clean -f #清楚node缓存 有时候不用也可以
        sudo npm install n -g   #安装node版本管理工具n
        sudo n stable           #安装最新版node
        
        sudo n 8.9.4            #安装指定版本
        node -v
    



    



       
# 引用
1. [GitBook 使用入门](https://blog.csdn.net/wirelessqa/article/details/72616471)
2. [node升级的正确方法](https://blog.csdn.net/tlbaba/article/details/79412433)
3. [Node.js教程](https://www.runoob.com/nodejs/nodejs-tutorial.html)