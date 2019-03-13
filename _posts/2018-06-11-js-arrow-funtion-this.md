---
layout: post
title:  箭头函数的this指向
date:   2018-06-11 14:30:21 +0800
categories: document
tag: 笔记
---

* content
{:toc}

#### 箭头函数与常规函数的this指向问题

​	常规函数	 `  function (){} ` 中的this指向使用`function()`的宿主对象，而箭头函数中指向的是封闭的函数上下文。

eg:

```javascript
let car = {
    a: '1',
    b: '2',
    fun1: function (){
        console.log(this,this.a);
    },
    fun2: () => {
        console.log(this,this.b);
    }
}
car.fun1(); // {a:'1',b:'2'......}(car对象)，1
car.fun2(); // Window对象，undefined

