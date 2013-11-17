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

## 其他写博的方式
上面用github pages写博快准狠，但是有个麻烦的地方，那就是私密性问题，由于现在的项目是私有仓库，基于阮一峰老师的模版上给项目写的文档就不能po到这上面，折腾了一两天的东西竟然得为了防谷歌seo等种种问题而不得不刹住，真心让我感觉受挫，而且感觉没有得到认可，但也罢，算是自己重新把项目温习一遍吧，现在想的就是如果有机会，拿到实验室的服务器的一个对外端口那就好了，我就可以把这个站点po出去，仅对项目成员来说，而且这样也可以考虑到私密性了，对吧。另外，今天在微博上看到玉伯大拿分享的一条微博，是个牛人自己写叫做 gitpress 的写博工具，只要简单的一个配置，就可以让你轻松写博客，[gitpress传送门](http://akira-cn.gitpress.org/~posts/2013-11-17-gitpress.org%20%E5%9F%BA%E4%BA%8Egithub%E7%9A%84%E6%87%92%E4%BA%BA%E5%8D%9A%E5%AE%A2%E7%B3%BB%E7%BB%9F.md)，具体大家可以去试用一下。另外一个是之前都不晓得玉伯大拿的用他自己用户名的那个github.io的url怎么链到了他的issue，现在终于晓得了，其实就是和我现在这个站是一样的道理，也是用的github pages的个人页，不同的是他在index.html中将url链到了issues的页面，所以就成了用issues写博客，直接用的最简单的模版，爽通透。