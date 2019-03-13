---
layout: post
title:  ios10下safari不能通过meta标签禁用缩放
date:   2018-10-15 14:00:00 +0800
categories: document
tag: 笔记
---

* content
{:toc}

#### ios10下safari不能通过meta标签禁用缩放

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
