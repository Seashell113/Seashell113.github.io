---
layout: post
title:  js浮点数计算精度的问题
date:   2018-07-21 19:00:22 +0800
categories: document
tag: 笔记
---

* content
{:toc}

####  js计算浮点数乘法时偶尔会有多余小数

```
如下

```

​	![1541663814130](C:\Users\HABITA~1\AppData\Local\Temp\1541663814130.png)

```
×××简单的解决方法为按精度需要统一转成整数，运算完后转回小数。
	（使用Math.round四舍五入为整数）

```

```javascript
Math.round(num*1000)/1000;

```

​	tips： toFixed()方法使用的是银行家舍入法，即奇舍偶入，不适合使用。

