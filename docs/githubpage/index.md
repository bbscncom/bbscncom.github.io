---
layout: default
title: githubpage
nav_order: 2
has_children: true
---


github提供了部署静态网站的功能, 不用花钱租服务器了. 当然有流量限制

由于我第一次使用时, 网上找的教程, 就算加上使用gpt 也不是很顺利. 所以在我成功后, 就想着分享一下经验



所谓知其所以然, 才能更好使用这个功能,所以我会说说github提供的这个功能的原理

ruby生态下的Jekyll, 可以使用各种网站模板各种类型, 同时生成的这个网站可以直接通过命令很简单在本地部署

而github page则是使用Jekyll来进行网站部署的. (按我理解应该是



push后github page自动编译发布,新的发布生效还有10分钟的延迟. 说不定编译还会失败, 对于新手或者说对于频繁修改的情况根本不可接受.  肯定是要在本地调试,结果好了之后再发布上去.

所以首先就是了解ruby的jekyll是如何工作的.



