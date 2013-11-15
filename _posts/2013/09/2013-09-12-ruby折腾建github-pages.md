---
layout: post
title: github-pages建个站折腾篇
categories:
- 旁门左道
tags:
- jekyll
- gem
---

## github-pages之ruby搞死我了

今天折腾了一天，不对，要加上昨天的半天和晚上，总算是勉强把github-pages的项目博客页给弄好了。

昨天开始想要通过jekyll在github pages上折腾小博客，折腾jekyll，要跑github pages，发现得要gem，那就要ruby，而且还要1.9.3，然后就要去装rvm，然后rvm我翻墙了还是没装上，用淘宝的镜像也是不行~~我只是想简单装上个1.9.3要那么难么~~

## 后来发现不需要装ruby

经过很多很多调研，突然醒悟过来，不需要装ruby，你按照jekyll语法搭建的静态网页的站，配合markdown写的文件，都是在github服务器上解析执行，生成 _site 文件夹的，然后就直接可以访问了。我们装ruby可能仅仅是为了在本地跑server看效果而已。

总体来讲 github pages 分成两种：

- 项目主页：
	+ 通过 `http://username.github.io/project_name/` 访问
	+ push到一个 orphan 分支：gh-pages
	+ 使用 jekyll 语法，用 markdown 书写
- 个人主页：
	+ 直接访问：`http://username.github.io` （username为你的github账户名）
	+ push到 master 分支
	+ 使用 jekyll 语法，用 markdown 书写

## 项目站点
建站步骤相对简单：

	$ git clone https://github.com/USERNAME/PROJECT.git PROJECT
	$ git checkout --orphan gh-pages
	$ git rm -rf .
	$ git add .
	$ git commit -a -m "First pages commit"
	$ git push origin gh-pages

过大概十分钟之后，如果你没有配域名，那么就可以通过上面讲的方法访问了。这里面最麻烦最DT的就是如果用模版，那改模版为己所用就需要非常多的功夫了。