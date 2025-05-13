---
layout: default
title: 部署到github
parent: githubpage
nav_order: 4
---

# 部署到github


创建public github项目,  setting 页面的page 选项

设置source为branch

设置分支(必须初始化项目,不然没分支)

设置docs为根目录,也就是Gemfile和_config.yaml存放的路径



然后将本地成功运行的项目push,就会自动部署,等10分钟就能看到网站刷新了



**原理**

github应该就是把本地执行的流程走一遍, 具体我也不知道, 反正结果都是生成静态网站

最后把生成的网站往 github的服务器部署





