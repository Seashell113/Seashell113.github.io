---
layout: post
title:  js的event loop机制
date:   2019-01-21 14:30:21 +0800
categories: document
tag: 笔记
---

* content
{:toc}

#### js的event loop机制

​	js是单线程的，但使用单线程模拟了多线程的效果。

​	具体实现是将任务分为同步和异步进程，同步进入主线程执行，异步进入event table并注册函数，当指定的任务完成时，event table会将该函数移入event queue（任务队列）。主线程内任务执行完毕为空时，会去event queue读取函数调入主线程执行，即为event loop。

​	*对任务更精细的分裂是宏任务（macro-task）和微任务（micro-task），**不同类型的任务会进入不同的event queue**。

​	事件循环的顺序，决定js代码的执行顺序。进入整体代码(宏任务)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。

![img](https://user-gold-cdn.xitu.io/2017/11/21/15fdcea13361a1ec?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

​	详细：[掘金][https://juejin.im/post/59e85eebf265da430d571f89]