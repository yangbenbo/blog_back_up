---
title: hexo本地图片配置
date: 2019-07-16 08:45:17
categories:
- blog
tags:
- blog

---

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
            
## 引用

1. [hexo建站过程中踩的坑总结](https://alreadyright.github.io/2019/06/16/aboutHexo/)
2. [hexo配置本地图片和pdf](https://jankin987.github.io/2018/09/19/02.Hexo/03.hexo%E9%85%8D%E7%BD%AE%E6%9C%AC%E5%9C%B0%E5%9B%BE%E7%89%87%E5%92%8Cpdf/)