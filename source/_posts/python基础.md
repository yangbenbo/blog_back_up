---
title: python基础
date: 2020-05-22 16:52:49
categories:
- program
tags:
- python
---

很多简单模块容易忘,在此记录一下

# 基础操作
1. 查看帮助文档
    
        import os
        # 主要有3中类型 ,也可以针对具体函数
        dir(os)
        help(os) 
        os.__doc__
2. 查看变量内存地址
    
        # 需要注意有时候变量的复制是浅拷贝,地址相同的
        # 使用pybullet获取机械臂位置就是这样,机械臂运动之后地址才变了
        print(id(object))
3. 查看当前路径
    
             import os
             os.getcwd()
             
## 安装模块
1. 常用安装命令
        
        # 安装 conda环境可以使用conda install xxx
        pip install package_name
        pip install package_name==package_version
        # 更新
        pip install -U package_name
        # 显示模块的信息
        pip show package_name
        # 查看对应包简单信息
        pip list |grep package_name
        # 卸载建议这种方式,不会会有残留文件(针对pip install -e .)
        pip uninstall package_name  
        

2. pip install -e . VS python setup.py install

        Editable   pip                    setup.py
        yes        pip install -e .       python setup.py develop    
        no         pip install .          python setup.py install    
- 可编辑模式安装(develop,可以在当前文件基础上修改)
  
  这种方式会在site-packages下创建一个文件指向项目目录,并不会复制对应代码;
  在项目根目录(setup.py所在)会生成egg-info 文件夹包含这个包的主要信息;
  查看sys.path会发现里面多了一个项目根目录的路径    
  
        pip install -e /path/to/your/setup.py  # 项目根目录                  
- install 这种方式会在site-packages生成egg文件,里面包含对应的函数和模块,无法修改    

        
       
## 引用
1. [在virtualenv中设置：`pip install -e .` vs `python setup.py install`](https://www.jb51.cc/python/241778.html)
2. [如何将自己的Python程序打包--setuptools详解](https://www.jianshu.com/p/9a54e9f3e059)       
        
              