---
layout: post
title:  will-change 硬件加速的使用
date:   2018-11-15 14:00:00 +0800
categories: document
tag: 笔记
---

* content
{:toc}

#### 硬件加速will-change属性

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

