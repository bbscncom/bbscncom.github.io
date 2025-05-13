---
layout: default
title: 使用jekyll
parent: githubpage
nav_order: 3
---

# 使用jekyll


jekyll相当于java的jar包, 首先就是导包

~~~bash
#Gemfile文件(没有后缀)

source "https://rubygems.org" #添加仓库
gem "github-pages", "~> 230", group: :jekyll_plugins #jekyll 特供版
gem "webrick" #bug解决(可能是我版本没选对,缺了这个依赖)
~~~

导完包后就开始配置jekyll

~~~bash
#_config.yaml

title: mcmod入门分享
description: 版本1.12
#这里有两个选择,使用remote_theme从github仓库找theme.
#使用theme,使用Gemfile文件导的包 比如 gem ""
remote_theme: just-the-docs/just-the-docs


#jekyll在运行的时候会打包所有文件,要排除不需要的
exclude:
  - .sass-cache/
  - .jekyll-cache/
  - gemfiles/
  - Gemfile
  - Gemfile.lock
  - node_modules/
  - vendor/bundle/
  - vendor/cache/
  - vendor/gems/
  - vendor/ruby/
  - bin/
  - lib/
  - "*.gemspec"
  - "*.gem"
  - LICENSE.txt
  - package.json
  - package-lock.json
  - Rakefile
  - README.md
  - CODE_OF_CONDUCT.md
  - docker-compose.yml
  - Dockerfile
  - fixtures/
  - startserver.bat
#后续用到
sass:
  style: compressed
~~~

主页面index.md

~~~bash
---
layout: default
title: 前言
nav_order: 1
---
#必须放在最前面,设置各种参数.
	#第一个我也不知道,反正就是default.
	#第二个就是标题,是url路径名,还用来绑定子层级
	#同层级排序

主页内容..........
~~~



**启动**

~~~bash
#读取gemfile加载依赖
bundle
#运行jekyll
bundle exec jekyll serve

当执行bundle exec jekyll serve时,就会通过 remote_theme: just-the-docs/just-the-docs 设置的theme
将md文件转成html.
转换完成后会启动一个http服务, 打开后自动进入index.html.
网站是正常显示的话这时就完成了本地部署了
~~~



