---
title: CSS 布局实例系列（一）总结 CSS 居中的多种方法
date: 2016-01-21 00:30:33
categories: 前端技术
tags: CSS
---

使用 CSS 让页面元素居中可能是我们页面开发中最常见的拦路虎啦，接下来总结一下常见的几种居中方法吧。

<!--more-->

## 水平居中

1. `text-align` 与 `inline-block` 的配合

	<p data-height="265" data-theme-id="0" data-slug-hash="mVpVEr" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="mVpVEr" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/mVpVEr/">mVpVEr</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

	HTML 中在想要居中的元素外面套了一个父元素，然后在 CSS 中将父元素的 `text-align` 属性设为 center，接下来将子元素的 `display` 属性设为 inline-block 就可以水平居中了。

2. 通过 `margin` 实现

	<p data-height="265" data-theme-id="0" data-slug-hash="rxpxmR" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="rxpxmR" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/rxpxmR/">rxpxmR</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

	通过 `margin` 实现连父元素都不用套了，直接 `margin: 0 auto;` 搞定，对，就是这么简单快捷，恐怕是居中最常用的方法了吧。

好啦，现在老板表示只是水平居中不行，还得垂直居中，该怎么办呢？

## 垂直居中
1. 还是先用 `margin` 来实现绝对定位元素的水平垂直居中吧

	<p data-height="265" data-theme-id="0" data-slug-hash="NxXxBz" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="NxXxBz" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/NxXxBz/">NxXxBz</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

	注意代码中的几个关键点：

	- 子元素 div 绝对定位
	- 父元素需要被定位
	- 子元素 top、bottom、left、right 四个位置值均为 0
	- 子元素 margin: auto;

2. 接下来使用来自 CSS3 的新杀器 `flex`

	<p data-height="265" data-theme-id="0" data-slug-hash="xZpZMw" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="xZpZMw" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/xZpZMw/">xZpZMw</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

	使用 `flex` 容器布局实现水平垂直居中的关键点在于：

	- 父元素 display 属性设为 flex
	- 垂直布局的属性是 align-items，设为 center 时便垂直居中
	- 水平布局的属性是 justify-content，设为 center 时水平居中
	- 子元素弹性居中，增加子元素也不会有影响

	另外请注意兼容性问题，见下图：

	![flex 兼容性](http://7xinvi.com1.z0.glb.clouddn.com/flex-support.png)
	
## 总结

其实利用 CSS 实现居中还有许多方法我没有写在博文中，如何选择居中的技术方案，最后还是得取决于当前业务场景下的浏览器支持程度和适合度。顺带一提个人最喜欢使用的是 `flex` 方案。

源代码已同步至个人 [repo](https://github.com/nitta-honoka/baiduIFE_practice/tree/master/2015_spring/Intermediate/task0001)，欢迎参考交流（笑）