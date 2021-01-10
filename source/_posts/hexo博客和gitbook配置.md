---
title: hexo博客和gitbook配置
date: 2019-10-06 08:36:02
categories:
- blog
tags:
- hexo
- blog
- gitbook

---
环境：ubuntu16.04

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=3 orderedList=false} -->

<!-- code_chunk_output -->

- [环境配置](#环境配置)
- [常用命令](#常用命令)
- [定义目录结构](#定义目录结构)
- [博客撰写工具](#博客撰写工具)
- [引用自己的文章](#引用自己的文章)
- [hexo 本地图片，避免图床](#hexo-本地图片避免图床)
- [hexo 配置pdf显示](#hexo-配置pdf显示)
- [添加目录](#添加目录)
- [踩过的坑](#踩过的坑)

<!-- /code_chunk_output -->

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
---

# hexo博客搭建

    sudo apt install nodejs
    sudo apt install npm
    
    sudo npm install n -g
    sudo n stable
    
    sudo npm install hexo-cli

# hexo博客使用

## 博客撰写工具
可以使用`markdown`语言进行快速编写,参考{% post_link Markdown语法 Markdown %}   
软件可以使用visual studio code结合插件`Markdown Preview Enhanced`.可以预览公式,可参考[知乎文章 在 VSCode 下用 Markdown Preview Enhanced 愉快地写文档](https://zhuanlan.zhihu.com/p/56699805)

## 引用自己的文章

    {% post_link 文章文件名(不要后缀) 文章标题(可选) %}    

## hexo 本地图片，避免图床

官网有相关说明，[hexo图片上传说明](https://hexo.io/zh-cn/docs/asset-folders)

	1. 设置站点配置_config.yml: 将post_asset_folder: false改为post_asset_folder: true
	2. 执行 npm install hexo-asset-image –save 装插件(或者npm install https://github.com/CodeFalling/hexo-asset-image --save)
	3. 执行hexo new [xxxx],生成xxxx.md和xxxx文件夹
	4. 把要引用的图片拷贝到xxxx文件夹中
	5. 使用\![]\(xxxx/example.jpg)来引用本地图片

## hexo 配置pdf显示
1. 安装hexo-pdf插件

        npm install -save hexo-pdf
2. 拷贝pdf到资源文件夹
    与文章同名的资源文件夹在配置图片时已经配置过,这里直接用
3. 文章中引用
    
        {% pdf pdf文件名 %}
    外部链接使用外部链接：
        
        {% pdf http://7xov2f.com1.z0.glb.clouddn.com/bash_freshman.pdf %}

## 添加目录
这是在visual studio code中使用的
可以在想要添加目录的地方加入下面这句话,就能自动为文章增加目录,Awesome!
还可以设置需要显示的目录级别

    <!-- @import "[TOC]" {cmd="toc" depthFrom=１ depthTo=3 orderedList=false} -->
    
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
4. [hexo建站过程中踩的坑总结](https://alreadyright.github.io/2019/06/16/aboutHexo/)
5. [hexo配置本地图片和pdf](https://jankin987.github.io/2018/09/19/02.Hexo/03.hexo%E9%85%8D%E7%BD%AE%E6%9C%AC%E5%9C%B0%E5%9B%BE%E7%89%87%E5%92%8Cpdf/)
