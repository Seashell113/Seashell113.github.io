---
layout: post
title:  自执行函数
date:   2018-06-20 15:34:22 +0800
categories: document
tag: 笔记
---

* content
{:toc}

#### 自执行函数

```
函数前的一元操作符会使函数成为自执行函数，原理猜想是通过一元操作符使原本的函数定义语句被解析为运算语句/函数表达式执行，等同于将函数用括号包括。
```

```javascript
!function(){}()     //最后的()作为参数输入

//等于下面这种形式
(function(){})()

```