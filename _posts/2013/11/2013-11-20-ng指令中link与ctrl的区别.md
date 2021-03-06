---
layout: post
title: ng指令中controller与link的区别
categories:
- programming
tag:
- AngularJS
---

## 几个有用的链接

- 首先是stackoverflow上赞同率超高的一个回答：[link与ctrl的不同](http://stackoverflow.com/questions/12546945/difference-between-the-controller-and-link-functions-when-defining-an-angula)
- 其次是另一个，个人觉得有抄袭嫌疑：[link vs compile vs controller](http://stackoverflow.com/questions/15676614/directive-link-vs-compile-vs-controller)

好了，接下来差不多进入正题，这个是个人读 [Difference between controller and link](http://jasonmore.net/angular-js-directives-difference-controller-link/) 这篇博客时的一些笔记，不喜勿喷。

## 指令上controller跟link的区别
我们都知道在ng的指令中，返回的对象中有两个重要的属性：

{% highlight javascript %}
// link function
{
    link: function(scope, iElem, iAttrs, ctrl) { ... },
    controller: function($scope, element, attrs) { ... }
}
{% endhighlight %}

这两个都可以获取到作用域，元素，属性等引用，也都会执行一次，在我还是个ng菜鸟的时候，当然，现在也还是，当我每次想要扩展个自定义指令时，脑海中总是萦绕着“where the fuck should I put my code?”，在controller，还是link function中呢。  
**简短的回答是**：优先放在 `link function` 中。当然啦，这要取决于你想要你的代码什么时候运行。

- Before compilation? – Controller
- After compilation？ - Link function

另外，他们的**基本区别**是：

- 控制器可以暴露一个API，而link可以通过require与其他的指令控制器交互
- 所以如果要开放出一个API给其他指令用就写在controller中，否则写在link中

## 执行先后问题

首先，看下面一个很有趣的示例：[log-compile指令](http://plnkr.co/edit/CW7cF0?p=preview)，通过这个 plnkr ，我们可以知道，**子级指令的所有 link function ：包括 pre 和 post 两个link都会在父级的post link之前被执行**，我们通常所说的link function，其实*是 post link 的快捷方式罢了*。所以，像下面这个例子中，这样用，是会有问题的：[说明 link 在 ctrl 之后执行的一个例子](http://plnkr.co/edit/SHOe0O?p=preview)，上面这个例子非常给力，值得学习。