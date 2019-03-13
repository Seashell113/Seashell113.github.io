---
layout: post
title:  图片src的优化显示
date:   2018-09-15 00:00:00 +0800
categories: document
tag: 笔记
---

* content
{:toc}

#### 图片src为空 出现浏览器默认裂图

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


