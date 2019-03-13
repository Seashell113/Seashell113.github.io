---
layout: post
title:  ES7多值比较的一个小技巧
date:   2018-03-11 11:00:22 +0800
categories: document
tag: 笔记
---

* content
{:toc}

####  单一变量多值判断时使用数组方式

```javascript
// 如下
if( a == b || a == c || a == d ){
    // function
}

// 可改写成
var arr=[b,c,d];
if(arr.includes(a)){
    // function
}
// 此方法在值数量较多时效果较好，其中arr.includes()是ES7语法，返回一个布尔值

```
