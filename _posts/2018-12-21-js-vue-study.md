---
layout: post
title:  Vue学习相关（保持更新）
date:   2018-12-21 11:00:22 +0800
categories: document
tag: 笔记
---

* content
{:toc}


1. #### v-for 指令 配合key属性使用可以有效提升性能

   解释：由v-for指令渲染的元素在绑定的data发生变化时总是会引发整体的重新渲染，造成不必要的性能损耗，而以索引做key属性时，可以通过key对应的虚拟DOM元素快速找到发生变化的真实DOM并作出更改，从而提升性能。

2. #### 父子组件、v-html指令 内部样式问题

   在组件中，子组件以及通过v-html指令动态添加的dom元素，无法通过在该组件内对父元素类名选择添加样式来预先调整本身样式，原因是组件渲染完成时，内部动态添加的html还未获得。因此需要通过引入外部全局css的方式（import ‘global.css‘）来实现。

3. #### vue-router 

   this.$route与this.$router快速记忆：获取信息用route，操作路由用router。
```
