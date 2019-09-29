---
title: hexo本地图片配置
date: 2019-07-16 08:45:17
categories:
- blog
tags:
- blog

---

hexo 本地图片，避免图床
---

官网有相关说明，[hexo图片上传说明](https://hexo.io/zh-cn/docs/asset-folders)

	1. 设置站点配置_config.yml: 将post_asset_folder: false改为post_asset_folder: true
	2. 执行 npm install hexo-asset-image –save 装插件(或者npm install https://github.com/CodeFalling/hexo-asset-image --save)
	3. 执行hexo new [xxxx],生成xxxx.md和xxxx文件夹
	4. 把要引用的图片拷贝到xxxx文件夹中
	5. 使用\![]\(xxxx/example.jpg)来引用本地图片


引用
---
1. [hexo建站过程中踩的坑总结](https://alreadyright.github.io/2019/06/16/aboutHexo/)