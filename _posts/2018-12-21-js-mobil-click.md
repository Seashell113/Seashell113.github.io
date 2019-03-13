---
layout: post
title:  移动端click事件bug
date:   2018-12-21 14:30:21 +0800
categories: document
tag: 笔记
---

* content
{:toc}

#### 移动端click事件延迟或触发异常

​	产生原因可能是移动端触摸事件判定相关，需要一定的延时（300ms）来判断是否滑动、双击等。

​	可以通过touchend事件的方式解决，但某些ios系统和设备不兼容。

​	解决方式：可引入轻量级库 fastclick.js [github][https://github.com/ftlabs/fastclick]

​	使用方式：在dom解析前引入js文件，并在页面load后实例化。

```javascript
// recommanded method
if ('addEventListener' in document) {
	document.addEventListener('DOMContentLoaded', function() {
		FastClick.attach(document.body);
	}, false);
}

// Jq
$(function (){
    FastClick.attach(document.body);
});


```
