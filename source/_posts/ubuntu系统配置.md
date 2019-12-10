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

# 软件安装

- chorme
- RedShift(如果不行的话考虑 护眼软件flux安装：先安装crossover，然后安装的windos下的flux)
- terminator
- ssr
- 输入法 sogo for Linux
- wine
- GoldenDict
- shutter
- draw.io

## RedShift
首选挺好用的护眼工具 如果无法使用就用flux

    sudo apt-get install redshift  
      
    gedit .config/redshift.conf     # 对应修改相应配置文件
    redshift -t 5700:3600       # 设置白天和黑夜对应温度   默认5000-4100
    tldr redshift               # 忘记命令可以使用tldr 或者cheat

## terminator
终端分屏工具

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

## ssr

1. 使用：双击[electron-ssr](https://github.com/qingshuisiyuan/electron-ssr-backup)
2. 账号:
	1. 免费账号：[github](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7)
	2. 免费账号+服务器订阅：[github](https://github.com/the0demiurge/ShadowSocksShare)

感谢他们的付出

## wine
可以安装windows程序,这里我用于安装mathtype,写博客用公式就不同切换会windows了

    sudo apt-get install wine
wine安装软件3中方式
    
    wine xxx.exe
    ./xxx.exe
    直接双击(可能需要选择wine打开)
    
    winecfg （wine的设置~）
    wine  taskmgr （任务管理器）
    wine  uninstaller （卸载软件）
    wine  regedit （注册表）
    wine  notepad （记事本）
    wineboot （ 重启wine）
    
## GoldenDict
一款强大的翻译软件
1. 安装

        sudo apt install goldendict
2. 配置
    - 网络词典 Edit -> Dictionaries -> Sources -> Websites -> Add
    
    常用的有:必应、有道、百度,选择一个就可以了，这里选择bing,(youdao的网页不能播放发音)
    
        必应：https://cn.bing.com/dict/search?q=%GDWORD%
        有道：https://dict.youdao.com/search?q=%GDWORD%&ue=utf8
        百度：https://fanyi.baidu.com/#en/zh/%25GDWORD%25      
    - 本地词典 Edit -> Dictionaries -> Sources -> Files -> Add
    
    《牛津英汉双解美化版》 下载网址[http://download.huzheng.org/zh_CN/](http://download.huzheng.org/zh_CN/)
    - 屏幕取词 Edit -> Preferences -> Scan Popup
    - 词形匹配(如果主要使用网络词典，可以不用配置此项) Edit->Dictionaries->Morphology  
    
        下载英文构词法规则库,[地址](https://sourceforge.net/projects/goldendict/files/better%20morphologies/1.0/)
3. 使用　
    选中单词 Ctrl+C+C　(可以设置)        
          
## shutter
截图软件

        sudo apt install shutter    
## draw.io
- 支持谷歌浏览器app(推荐)
- 支持在线绘图,类似visio
- [桌面版本](https://github.com/jgraph/drawio-desktop/releases)
 


导出jpg可设置更改缩放为1000%就可以有较好清晰度

[draw.io](https://www.draw.io/)        

# 资源

1. [软件资源](https://pan.baidu.com/s/1B4lO9MiZVEqehld8ffoeFg),提取码：87q8

# 开机启动软件设置
1. 启动软件
2. ps auxww |grep qq    # 找到软件对应的命令
3. 在startup里面添加对应命令

# 引用
1. terminator[配置](https://blog.csdn.net/ipatient/article/details/51547658)
2. [Ubuntu桌面启动后自动执行指定的命令或程序的三种方法](https://blog.csdn.net/davidhzq/article/details/102725116)
3. [Ubuntu 18.04 LTS版本 GoldenDict安装与配置](https://www.cnblogs.com/creasing/p/11333728.html)
4. [Ubuntu,Linux下goldendict词典安装及配置](https://blog.csdn.net/www_helloworld_com/article/details/85019862)
5. [强烈推荐：Goldendict 及其词典详述(5 月 26 日更新)](https://forum.ubuntu.org.cn/viewtopic.php?f=95&t=265588)
6. [Linux 下非常好用的字典 GoldenDict](http://einverne.github.io/post/2018/08/goldendict.html)
7. [wine 安装（ubuntu中安装windows下软件）](https://blog.csdn.net/qq_34638161/article/details/81271977)
