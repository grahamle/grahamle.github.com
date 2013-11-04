---
layout: post
title: 使用AngularJS开发web-app [译书]
categories:
- programming
tag:
- AngularJS
---

**书籍外文名**：*Mastering Web Application Development with AngularJS*

今天开始，我会陆续对这本书的部分章节（也许是全部）进行翻译，不包括前言等。**翻译说明**：

- 对于诸如 `directives` 这样的术语不做翻译
- 不全段翻译，有可能删去废话段落进行翻译
- 翻译属于边看边学边敲代码的过程，可能会加入自己的注解

这本书的主要能够学到以下三点：

- 如何利用目前已有的AngularJS的 `services` 和 `directives` 构建一个完整且健壮的应用
- 在没有现成的解决方案时如何扩展AngularJS
- 如何建立一个高质量的AngularJS开发项目（类似于种子）：包括代码组织、项目建立、测试、性能优化等

学习过程中，你需要准备如下开发环境/工具：

- web浏览器 + 文本编辑器 / IDE
- 安装node.js及其包管理工具npm
- Grunt + Karma
- 后端是跑在MongoDB上的（需要网络）

# Ch.1 AngularJS之道

前面是一堆废话说angular（以下用angular代称angularJS）将来会很好，blabla，此处省略几百字。接下来讲下这章的内容：

- 如何用些angular版本的hello world
- angular应用的基本组成部分：directives, scopes, controllers
- angular的依赖注入
- angular和别的框架或库（尤其是jQuery）的差别

## angular框架概览
angular特殊在于它的模版系统（templating system）：

- 用HTML作为模版语言
- 因为angular可以跟踪用户操作，浏览器事件以及model改变，所以不需要显式地DOM更新，它会找出什么时候哪个模版需要更新
- angular对HTML标签及属性进行了扩展，所以它能告诉浏览器去解析这些扩展

angular别的隐藏大招如：DI（依赖注入）、测试

## angular大杂烩
主要就是讲一些这个项目的起源，以及在github上开源，还有Google+以及别的社区上（如StackOverflow）可以获得的帮助。此处不翻。

很多东西你都可以在[angular官网](http://www.angularjs.org)上找到，虽然我觉得里面的文档写得很shi。另外推荐一个国内的关于angular资料整理的很好的链接：[AngularJS资源大集锦](http://www.csdn.net/article/2013-10-31/2817356-Resources-to-Get-You-Up-to-Speed-in-AngularJS)。

至于基于angular写的很多插件、库和扩展，可以到[这个网站](http://ngmoudles.org)上找到。

一些工具：

- 传统工具：IDE如webstorm, sublime text
- angular团队贡献的：Batarang, Plunker, jsFiddle

## angular的hello-world
要是真像标题所说的有crash course这样逆天的存在，真的就不用学了，angular的学习曲线很陡，这是个人经验。

**通过hello-world例子学习angular基础组成部分**：

首先直接看以下hello-world的代码：

	<html>
	<head>
		<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.0.7/angular.js"></script>
	</head>
	<body ng-app ng-init="name = 'world'">
		<h1>Hello, {{name}}!</h1>
	</body>
	</html>

1. 在项目页中引入angular库：1）通过google的CDN；2）也可以下载下来
2. 引用了库之后，就是在你的应用里如何启用angular了，通过：`ng-app` 这个HTML属性
3. 在body标签中还有一个非标准的HTML属性：`ng-init` ，这个属性可以让你在模版被渲染之前准备好数据模型
4. 最后需要介绍的是 \{\{name\}\} 这个表达式，它做的工作很直接，直接将 `ng-init` 准备好的数据模型里的值渲染输出

从以上的示例我们可以看出，**angular模版系统的两个特点**：

- angular使用自定义HTML标签和属性来为静态的HTML页添加动态行为
- 双大括号（\{\{表达式\}\}）被用来让表达式输出数据模型的值

**directives**：

从上面的示例我们首先要引出的第一个angular的关键术语：**directives**，它是指所有angular这个框架能够理解并解析的特殊的（非标准的）HTML标签和属性。

**双向绑定**：

从示例我们要讲的第二个关键术语就是：**双向数据绑定**。从 `ng-init` 我们可以看到准备好的数据模型是可以通过*双大括号包含表达式*的方式渲染出来值的。如果在此基础上我们引入 `ng-model`的话，那么angular就可以做到：

1） 实时检测到数据模型的变化，而在下面这个例子中的数据模型的变化又恰恰是由视图的变化引起的；
2） 根据数据模型值的变化对模版（也就是DOM）进行实时更新。

也就是说，**angular为我们省去了那些多余的为了让模版进行更新而所需的DOM操作**。代码如下：

	<body ng-app ng-init="name = 'World'">
		Say hello to: <input type="text" ng-model="name">
		<h1>Hello, {{name}}!</h1>
	</body>

其实从上述代码不难发现，所谓的双向绑定，实质上涉及的是两种对象，三个实例：

- 数据模型对象（一个实例）：`name`
- 模版对象/视图对象（两个实例）：一个是 `<input>` （视图对象），另一个是 \{\{name\}\} （模版对象）；而模版对象其实是可以看作是视图对象的一部分，只不过它特殊一些，会动态变化，因为模版是和数据模型绑定的

所以上述的数据绑定过程其实很简单，发生在三个对象实例之间，**关键是全部都是实时的**，大概是这样：

- `<input>` 接收用户的实时输入，实时改变数据模型name
- name的实时改变直接引起了模版对象\{\{name\}\}的渲染输出结果，并最终影响了 `<h1>` 中的渲染结果