---
layout: post
title:  “+”号等符号带来的自动转换效果
date:   2018-12-20 15:34:22 +0800
categories: document
tag: 笔记
---

* content
{:toc}

####  “+”号等符号带来的自动转换效果

猜想原理是通过符号将对象变为表达式并执行输出后的结果
```javascript
// 16进制转换:
+”0xFF”;              // -> 255

// 获取当前的时间戳,相当于`new Date().getTime()`:
+new Date();

// 比 parseFloat()/parseInt()更加安全的解析字符串
parseInt(“1,000″);    // -> 1, not 1000
+”1,000″;             // -> NaN, much better for testing user input
parseInt(“010″);      // -> 8, because of the octal literal prefix
+”010″;               // -> 10, `Number()` doesn't parse octal literals
//一些简单的缩写比如： if (someVar === null) {someVar = 0};
+null;                // -> 0;

// 布尔型转换为整型
+true;                // -> 1;
+false;               // -> 0;

//其他:
+”1e10″;              // -> 10000000000
+”1e-4″;              // -> 0.0001
+”-12″;               // -> -12：

```

