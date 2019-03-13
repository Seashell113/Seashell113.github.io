---
layout: post
title:  个人问题记录
date:   2018-07-24 00:00:00 +0800
categories: document
tag: 笔记
---

* content
{:toc}


# 问题记录

[TOC]

## CSS：

#### 1.未知图片大小垂直居中不失真：		

```css
            /*垂直居中容器*/			
            .img_wraper {
				width:230px;
				height:110px;
				margin: auto;
				text-align: center;
				vertical-align: middle;
				position: relative;
			}
			.img_wraper img {
                /*垂直居中hack*/				
                max-width: 100%;
                max-height: 100%;
                position: absolute;
                /*图片大小超出容器时生效*/
                top: -9999px;
                bottom: -9999px;
                left: -9999px;
                right: -9999px;
                margin: auto;
			}
```



#### 2.float 浮动  浮动元素被顶下来

```
浮动元素之前的非浮动元素不会受到影响 可通过改变元素在文档中的渲染顺序调整			

摘自：https://blog.csdn.net/gg654304469/article/details/65638723
div1 设置了float：left，因此div1可以说已经半脱离了文档流。
说到这里就不免要说一下 float浮动 和 position：absolute绝对定位的区别了。
设置了float浮动元素会脱离文档流 即从页面普通的布局排版中脱离，其他盒子元素会当这个float浮动元素不存在而进行定位。但是文本元素却会重视它，因此形成了文本环绕效果。	
所以我们可以理解为当元素设置了float浮动，则该元素脱离了文档流却没有脱离文本流。因此我们也可以称之为半脱离文档流。
设置了position：absolute的元素则完全脱离了文档流。其他任何盒子元素、文本元素和盒子内的文本元素都将无视它进行定位。
div2 并没有设置float，因此它是文档流里的第一个元素占页面第一行，而后面的div3设置了float，因此它也半脱离了文档流。
这个时候，由于在html文档中div2写在div3之前 ，因此浏览器根据文档流先行渲染出来了div2的内容，而到了div3时浏览器不能破坏页面前面已经渲染出的部分，因此只能从div2下方开始渲染。
于是就出现了第一次看到的div3在div2下方向右浮动的现象。
So，解决方法就很简单了，只要书写html时div3写在div2之前，浏览器就会根据文档流先渲染div1、div3内容。之后由于div1和div3都半脱离了文档流，因此渲染div2内容时会无视div1、div3进行渲染，但文字会环绕于div1、div3之间。也因此有了三栏式布局。
```



#### 3.图片src为空 出现浏览器默认裂图

```css
css：img[src=""]{
	opacity:0;
}
```

通过属性选择器选中src为空的图片 并控制透明度使其不显示

Tips: 

1. /img:not([src]){}
2. 路径失效的解决方法 标签中添加onerror属性指定src



#### 4.标签内连续英文不自动换行

考虑以下方面

1. word-break属性
2. text-overflow
3. white-space





#### 5. ios10下safari不能通过meta标签禁用缩放

![](C:\Users\HABITA~1\AppData\Local\Temp\1537423882913.png)

尝试采用js阻止默认事件的方式禁用缩放

```javascript
window.onload = function () {
        document.addEventListener('gesturestart', function (e) {
            e.preventDefault();
        });
        document.addEventListener('dblclick', function (e) {
            e.preventDefault();
        });
        document.addEventListener('touchstart', function (event) {
            if (event.touches.length > 1) {
                event.preventDefault();
            }
        });
        var lastTouchEnd = 0;
        document.addEventListener('touchend', function (event) {
            var now = (new Date()).getTime();
            if (now - lastTouchEnd <= 300) {
                event.preventDefault();
            }
            lastTouchEnd = now;
        }, false);
    };
```

附相应链接：[博客地址](https://www.cnblogs.com/peakleo/p/6203424.html) 另内含各种css尺寸单位对比



#### 6. 硬件加速will-change属性

​	核心内容是通过will-change属性通知浏览器进行GPU而不是CPU去处理相关操作。

​	相关内容有浏览器渲染过程中创建的层树（layer tree），即类似PS中图层的概念，将添加了will-change属性的css元素独立为一层，交由GPU处理，即硬件加速，可以显著提升复杂变化的性能（如transform、animation、translate3d等动画操作）。

```javascript
// 使用
//【使用hover】

// 不要像下面这样直接写在默认状态中，因为will-change会一直挂着：

.will-change {
  will-change: transform;
  transition: transform 0.3s;
}
.will-change:hover {
  transform: scale(1.5);
}
// 可以让父元素hover的时候，声明will-change，这样，移出的时候就会自动remove，触发的范围基本上是有效元素范围

.will-change-parent:hover .will-change {
  will-change: transform;
}
.will-change {
  transition: transform 0.3s;
}
.will-change:hover {
  transform: scale(1.5);
}
//【使用javascript脚本】

.sidebar {
  will-change: transform;
}   


/* 以上示例在样式表中直接添加了will-change属性，会导致浏览器将对应的优化工作一直保存在内存中，这其实是不必要的。下面展示如何使用脚本正确地应用will-change属性 */

var el = document.getElementById('element');
// 当鼠标移动到该元素上时给该元素设置 will-change 属性
el.addEventListener('mouseenter', hintBrowser);
// 当 CSS 动画结束后清除 will-change 属性
el.addEventListener('animationEnd', removeHint);
function hintBrowser() {
  // 填写在CSS动画中发生改变的CSS属性名
  this.style.willChange = 'transform, opacity';
}
function removeHint() {
  this.style.willChange = 'auto';
}

/*
	【直接使用】
  但是，如果某个应用在按下键盘的时候会翻页，比如相册或者幻灯片一类，它的页面很大很复杂，此时在样式表中写上will-change是合适的。这会使浏览器提前准备好过渡动画，当键盘按下的时候就能即看到灵活轻快的动画
*/
.slide {
  will-change: transform;
}
 
```



> ​	1、不要将will-change应用到太多元素上：浏览器已经尽力尝试去优化一切可以优化的东西了。有一些更强力的优化，如果与will-change结合在一起的话，有可能会消耗很多机器资源，如果过度使用的话，可能导致页面响应缓慢或者消耗非常多的资源
>
>   2、有节制地使用：通常，当元素恢复到初始状态时，浏览器会丢弃掉之前做的优化工作。但是如果直接在样式表中显式声明了will-change属性，则表示目标元素可能会经常变化，浏览器会将优化工作保存得比之前更久。所以最佳实践是当元素变化之前和之后通过脚本来切换will-change的值
>
>   3、不要过早应用will-change优化：如果页面在性能方面没什么问题，则不要添加will-change属性来榨取一丁点的速度。will-change的设计初衷是作为最后的优化手段，用来尝试解决现有的性能问题。它不应该被用来预防性能问题。过度使用will-change会导致大量的内存占用，并会导致更复杂的渲染过程，因为浏览器会试图准备可能存在的变化过程。这会导致更严重的性能问题
>
>   4、给它足够的工作时间：这个属性是用来让页面开发者告知浏览器哪些属性可能会变化的。然后浏览器可以选择在变化发生前提前去做一些优化工作。所以给浏览器一点时间去真正做这些优化工作是非常重要的。使用时需要尝试去找到一些方法提前一定时间获知元素可能发生的变化，然后为它加上will-change属性
>
> [csdn链接]：https://www.cnblogs.com/xiaohuochai/p/6321790.html



## js:

#### 1. “+”号等符号带来的自动转换效果

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





#### 2. 自执行函数

```
函数前的一元操作符会使函数成为自执行函数，原理猜想是通过一元操作符使原本的函数定义语句被解析为运算语句/函数表达式执行，等同于将函数用括号包括。
```

```javascript
!function(){}()     //最后的()作为参数输入

//等于下面这种形式
(function(){})()

```



#### 3. 回车自动触发页面第一个button

```
通常是由于button未正确设置type=button引起的，待补充

```



#### 4.  js计算浮点数乘法时偶尔会有多余小数

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



#### 5. 单一变量多值判断时使用数组方式

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
// 此方法在值数量较多时效果较好

```



#### 6. 移动端click事件延迟或触发异常

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



#### 7. jq奇奇怪怪报错

​	优先考虑是否对jQuery重复引用。



#### 8. js的event loop机制

​	js是单线程的，但使用单线程模拟了多线程的效果。

​	具体实现是将任务分为同步和异步进程，同步进入主线程执行，异步进入event table并注册函数，当指定的任务完成时，event table会将该函数移入event queue（任务队列）。主线程内任务执行完毕为空时，会去event queue读取函数调入主线程执行，即为event loop。

​	*对任务更精细的分裂是宏任务（macro-task）和微任务（micro-task），**不同类型的任务会进入不同的event queue**。

​	事件循环的顺序，决定js代码的执行顺序。进入整体代码(宏任务)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。

![img](https://user-gold-cdn.xitu.io/2017/11/21/15fdcea13361a1ec?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

​	详细：[掘金][https://juejin.im/post/59e85eebf265da430d571f89]

#### 9. 箭头函数与常规函数的this指向问题

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


```



## vue：

1. #### v-for 指令 配合key属性使用可以有效提升性能

   解释：由v-for指令渲染的元素在绑定的data发生变化时总是会引发整体的重新渲染，造成不必要的性能损耗，而以索引做key属性时，可以通过key对应的虚拟DOM元素快速找到发生变化的真实DOM并作出更改，从而提升性能。

2. #### 父子组件、v-html指令 内部样式问题

   在组件中，子组件以及通过v-html指令动态添加的dom元素，无法通过在该组件内对父元素类名选择添加样式来预先调整本身样式，原因是组件渲染完成时，内部动态添加的html还未获得。因此需要通过引入外部全局css的方式（import ‘global.css‘）来实现。

3. #### vue-router 

   this.$route与this.$router快速记忆：获取信息用route，操作路由用router。



## npm:

1. #### --save与--save-dev

   --save命令指的是在packdge.json文件的dependencies中自动写入新安装的依赖，而添加-dev则是指定添加到devDependencies下，即开发依赖。

   相同点：

   1，都会将依赖下载到当前项目的 node_modules 目录里；

   2，都会记录在packge.json里记录；

   不同点：

    --save-dev

   只是开发和打包时会用到的，但实际上线并不需要；（例如webpack）

   会在 devDependencies 里记录；

   npm install 的时候，也是下载这个里面记录的依赖文件；

   --save

   除了开发需要，实际上线的时候也需要的；（例如React等框架及插件）会在 dependencies 里记录；

   **· --save = -S   --save-dev = -D** 



## 浏览器：

#### 1. chrome默认不开启flash  需要手动开启

#### 2. 火狐event需要兼容

## 杂项：

#### 1. gitbash的ctrlC命令在windows下不能正常结束node.exe进程 即无法解除端口占用  需要手动结束进程或改用cmd

[https://github.com/ftlabs/fastclick]: 