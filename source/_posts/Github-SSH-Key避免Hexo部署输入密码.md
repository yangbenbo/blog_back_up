---
title: Github SSH Key避免Hexo部署输入密码
date: 2019-10-01 16:19:16
categories:
- Linux
tags:
- blog
- ssh
- hexo

---
# 操作
前提: 已经生成ssh公钥放在github

修改_config.yaml,将部署方式从https改为ssh
    
    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git
      repository: https://github.com/yangbenbo/yangbenbo.github.io.git
      branch: master
      
改为

    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git
      repository: git@github.com:yangbenbo/yangbenbo.github.io.git
      branch: master   
# 注意事项
1. ubuntu换内核(kernel)启动 需要重新上传ssh-key              
      
# 参考
1. [使用Github SSH Key来避免Hexo部署时输入账户密码](https://www.cnblogs.com/yaoel/p/5381826.html)      