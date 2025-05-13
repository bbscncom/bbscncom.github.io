---
layout: default
title: githubpage
nav_order: 1
---



# githubpage

github提供了部署静态网站的功能, 不用花钱租服务器了. 当然有流量限制

由于我第一次使用时, 网上找的教程, 就算加上使用gpt 也不是很顺利. 所以在我成功后, 就想着分享一下经验



所谓知其所以然, 才能更好使用这个功能,所以我会说说github提供的这个功能的原理

ruby生态下的Jekyll, 可以使用各种网站模板各种类型, 同时生成的这个网站可以直接通过命令很简单在本地部署

而github page则是使用Jekyll来进行网站部署的. (按我理解应该是



push后github page自动编译发布,新的发布生效还有10分钟的延迟. 说不定编译还会失败, 对于新手或者说对于频繁修改的情况根本不可接受.  肯定是要在本地调试,结果好了之后再发布上去. 

所以首先就是了解ruby的jekyll是如何工作的.



## 准备工作

**安装ruby** 

https://rubyinstaller.org/downloads/

**安装 jekyll** 

gem install jekyll bundler



**寻找主题theme**

比如个人主页, 一个博客, 图片展示等

找到你想要theme,我也不知道最常用的分享网站是那些,我只知道我用的是搜索引擎搜到的

比如这个 http://jekyllthemes.org/page2/]



找到想要的theme后, 可以在ruby的gem仓库 ,github 等找说明书

比如我使用的 https://github.com/just-the-docs/just-the-docs



## ruby

ruby是一个语言,我也不熟悉, 但只需要知道他有强大的打包功能,还有一个常用分发平台,就像是java和maven仓库



**bundle**

ruby的依赖管理工具



**jekyll**

核心的静态网站生成工具



## 使用jekyll

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



## 部署到github

创建public github项目,  setting 页面的page 选项

设置source为branch

设置分支(必须初始化项目,不然没分支) 

设置docs为根目录,也就是Gemfile和_config.yaml存放的路径



然后将本地成功运行的项目push,就会自动部署,等10分钟就能看到网站刷新了



**原理**

github应该就是把本地执行的流程走一遍, 具体我也不知道, 反正结果都是生成静态网站 

最后把生成的网站往 github的服务器部署





## 进阶修改

原理上最终是生成html, 既然是html 那就可以灵活修改

我这里例子是修改样式



**sass**

我也不知道sass是什么鬼东西,反正就是一个gem的依赖, 一个工具 ,用来设置css

在Gemfile的目录创建一个_sass文件夹



然后在里面创建一个scss样式文件,这个文件就是我们需要自定义的样式

~~~css
.highlight {
 font-size: 1.2rem
}
.highlight .c1{
 color: red;
 font-style: normal;
}
~~~

在Gemfile的目录创建一个scss文件,我的路径是 assets\css\style.scss, 应该是路径随意都行

---好像是必须有不然不识别这个文件, @import就是引入刚才我们写的样式, 这就是sass的特殊语法

~~~bash
---
---

@import "custom_styles";
~~~

然后在jekyll运行的时候, 因为我们有sass依赖(github page依赖里面有), 会将所有sass转换成一个同名css, 这个 css就是我们的目标

好像从结果上来说sass根本没有必要, 直接写css不行吗 ,起码我这个例子好像没有, 总之就这样



**这里有一个重点**

theme是本地文件优先的, 也就是说完全可以去github将整个theme 拉下来, 里面有如何通过md生成网站的所有逻辑, 然后想改哪里改哪里,  也就是说theme对于用户完全是透明的, 你有什么需求theme的配置没有提供的 直接自己动手修改
就是以后theme版本更新你也收不到, 等于fork了一个自己管理的分支



所以现在我从theme下载一个head.html文件, 这个文件就是从md生成html的时候 设置head标签的模板

我在里面加一个link标签, 把刚才新生成的css加进去, 这样 css就生效了



这就是修改方法, 原理就这样, 不仅限于我这个theme





## 多级页面

这里的重点就是文件夹名字和 层级无关, 层级信息完全靠md头部的front 定义

文件夹定义url路径

~~~bash
#第一级
layout: default
title: 目标
nav_order: 4
has_children: true

#第二级
layout: default
title: 创建一个方块
parent: 目标
nav_order: 1

#第三级
layout: default
title: 给方块打开界面和保存数据功能
parent: 目标
nav_order: 2
has_children: true
~~~



## 页面生成脚本

由于我觉得设置子层级需要新md文件很麻烦

我直接用ai写了个py脚本

通过将h1作为第一级, h2作为第二级以此类推

具体看github项目



https://github.com/bbscncom/bbscncom.github.io

