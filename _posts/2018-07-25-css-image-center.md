---
layout: post
title:  CSS未知大小的图片垂直居中的一个黑科技
date:   2018-07-25 15:34:22 +0800
categories: document
tag: 笔记
---

* content
{:toc}

通过对四边定位的方式实现，主流浏览器测试均通过
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

