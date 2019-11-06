---
title: ubuntu系统配置
date: 2019-07-14 20:17:42
categories:
- Linux
tags:
- software
- ubuntu

---
最近在自己的笔记本安装了ubuntu16，记录一下环境配置

软件安装
===

- chorme
- RedShift(如果不行的话考虑 护眼软件flux安装：先安装crossover，然后安装的windos下的flux)
- terminator
- ssr
- 输入法 sogo for Linux

RedShift
---
首选挺好用的护眼工具 如果无法使用就用flux

    sudo apt-get install redshift  
      
    gedit .config/redshift.conf     # 对应修改相应配置文件
    redshift -t 5700:3600       # 设置白天和黑夜对应温度   默认5000-4100
    tldr redshift               # 忘记命令可以使用tldr 或者cheat

flux
---
最初直接装linux版本的在我电脑效果不好，所以装的windows下的。

crossover免费使用方法：将下载的so文件替换/opt/cxoffice/lib/wine/里的同名文件，记得先备份源文件。

terminator
---

1. 安装

		sudo apt-get install terminator

2. 配置（没找到文件夹就创建一个）

		cd ~/.config/terminator/ && sudo gedit config
3. 修改配置文件

		[global_config]
		  focus = system
		  suppress_multiple_term_dialog = True
		  title_transmit_bg_color = "#d30102"
		[keybindings]
		  close_term = <Primary><Alt>w
		[layouts]
		  [[default]]
		    [[[child1]]]
		      parent = window0
		      profile = default
		      type = Terminal
		    [[[window0]]]
		      parent = ""
		      type = Window
		[plugins]
		[profiles]
		  [[default]]
		    background_color = "#2d2d2d"
		    background_darkness = 0.85
		    background_image = None
		    copy_on_selection = True
		    cursor_color = "#EEE9E9"
		    font = Ubuntu Mono 13
		    foreground_color = "#eee9e9"
		    palette = "#2d2d2d:#f2777a:#99cc99:#ffcc66:#6699cc:#cc99cc:#66cccc:#d3d0c8:#747369:#f2777a:#99cc99:#ffcc66:#6699cc:#cc99cc:#66cccc:#f2f0ec"
		    show_titlebar = False
		    use_system_font = False

ssr
---
1. 使用：双击[electron-ssr](https://github.com/qingshuisiyuan/electron-ssr-backup)
2. 账号:
	1. 免费账号：[github](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7)
	2. 免费账号+服务器订阅：[github](https://github.com/the0demiurge/ShadowSocksShare)

感谢他们的付出


资源
===

1. [软件资源](https://pan.baidu.com/s/1B4lO9MiZVEqehld8ffoeFg),提取码：87q8

开机启动软件设置
===
1. 启动软件
2. ps auxww |grep qq    # 找到软件对应的命令
3. 在startup里面添加对应命令

引用
===
1. terminator[配置](https://blog.csdn.net/ipatient/article/details/51547658)
2. [Ubuntu桌面启动后自动执行指定的命令或程序的三种方法](https://blog.csdn.net/davidhzq/article/details/102725116)
