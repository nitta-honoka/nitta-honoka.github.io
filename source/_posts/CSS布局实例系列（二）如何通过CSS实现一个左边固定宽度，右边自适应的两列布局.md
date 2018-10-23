---
title: CSS 布局实例系列（二）如何通过 CSS 实现一个左边固定宽度，右边自适应的两列布局
date: 2016-01-24 00:41:42
categories: 前端技术
tags: CSS
---

最近在百度 IFE 训练营中看见的一道题目：

> 用两种不同的方法来实现一个两列布局，其中左侧部分宽度固定、右侧部分宽度随浏览器宽度的变化而自适应变化

聊聊相应的实现思路  
<!--more-->

### 通过绝对定位实现
<p data-height="265" data-theme-id="0" data-slug-hash="ZQrMXR" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="absolute-two-column" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/ZQrMXR/">absolute-two-column</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script> 

注意点如下：

- 需要套一个相对定位的父元素
- div-a 绝对定位，并将位置调整为浏览器左上角
- div-b `margin-left` 属性值为 div-a 的宽度（因为 div-a 绝对定位已经脱离文档流，故不设定为 div-a 宽度的话，会相互覆盖）
- div-c 绝对定位并将位置调整为正下方
- 需要自适应的 div 均设定宽度为100% 

### 通过浮动实现
<p data-height="265" data-theme-id="0" data-slug-hash="PZQdEG" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="float-two-column" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/PZQdEG/">float-two-column</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

注意点如下：

- div-a 设定为左浮动
- div-b 与上面一样需要将 `margin-left` 属性值设为 div-a 的宽度，原因同上，浮动也会脱离文档流
- div-c 最好清除浮动，避免浮动的影响

### 通过 BFC 规则实现
<p data-height="265" data-theme-id="0" data-slug-hash="JGpapq" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="bfc-two-column" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/JGpapq/">bfc-two-column</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

此处便不花大量篇幅介绍 BFC 了，可以参见下面两篇博文：

- [深入理解BFC和Margin Collapse](http://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html)
- [CSS之BFC详解](http://www.html-js.com/article/1866)

简而言之，BFC 可以帮助我们解决布局中左边元素脱离文档流后，右边元素的左外边距会触碰到包含块容器的左外边框的问题，就像下图：

![](http://7xinvi.com1.z0.glb.clouddn.com/css2.png)

现在我们仅需注意将 div-b 设定为 BFC 元素即可。

以上便是个人总结出的三种两列布局方法，源代码已同步至个人 [repo](https://github.com/nitta-honoka/baiduIFE_practice/tree/master/2015_spring/Intermediate/task0001)，欢迎参考交流（笑）

 