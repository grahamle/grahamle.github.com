---
layout: post
title: ng中的promise
categories:
- programming
tag:
- JavaScript
---

## 引子
其实一直知道 promise 是属于 Js 的非同步操作的一种模式，ng中的也一直有在用，就是一直没有整太明白到底它和 deferred 有什么区别。今天是在整合 PouchDB 和 AngularJS 的一个应用时，又遇到了这个问题，通过 ng 的 `$q.defer` 返回的 promise 到底是什么呢？又再一次勾起了我的困惑，所以，这次，我决心搞懂它来。    
[Sync Multiple AngularJS Apps Without Server via PouchDB](http://mircozeiss.com/sync-multiple-angularjs-apps-without-server-via-pouchdb/)

## Js中的promise
首先我们得来从Js中的promise说起。你要知道，我们所说的 `promise` 其实就是一个 POJO = Plain Old JavaScript Object 。而这个 promise object 又是被好多库用到了，包括我们熟知的 `jQuery`, `AngularJS`, `Dojo`, 还有 `WinJS` 等。其实呢，**promise模式可以看作是对回调模式的一种简化，通过在返回的promise对象上调用其 `then()` 方法，把在异步操作结束后需要进行的回调（操作）丢进去，这样看起来像是很顺畅的流程**。

关于 promise 的两种实现，请看 [jQuery与ng实现promise的不同](http://wildermuth.com/2013/8/3/JavaScript_Promises)

如果你想直接看代码，直接看下面这两个fiddle吧：

- [jQuery实现promise](http://jsfiddle.net/kWrpq/light/)
- [Angular实现promise](http://jsfiddle.net/s35B9/7/)

对比两种进行学习，虽然个人觉得 jQuery 这种不需要中间多一个对象来的好理解很多，但貌似现在这些都提倡像ng这样的实现，whatever，反正现在算是理解了，明天画个图。

## ng-promise进阶
说是进阶，其实说白了就是把东西细讲了，在之前的那篇的基础之上讲的更细一些了，适合阅读。因为这篇是有一个实际操作例子的，它的异步操作是数据库的存储/读写等，和上面的仅仅是个纯示例的 `setTimeout()` 的为了异步而异步完全是上了一个级别，真正的实例，让你爽。   
[Using AngularJS Promises](http://liamkaufman.com/blog/2013/09/09/using-angularjs-promises/)