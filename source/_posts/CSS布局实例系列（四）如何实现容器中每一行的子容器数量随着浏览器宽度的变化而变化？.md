---
title: CSS 布局实例系列（四）如何实现容器中每一行的子容器数量随着浏览器宽度的变化而变化？
date: 2016-01-28 01:08:46
categories: 前端技术
tags: CSS
---
Hello，小朋友们，还记得我是谁吗？对了，我就是~超威~好啦，言归正传，今天的布局实例是：

> 实现一个浮动布局，红色容器中每一行的蓝色容器数量随着浏览器宽度的变化而变化

肯定有人心里犯嘀咕了，哈~这么简单，不就是全部左浮动嘛，这也好意思拿出来讲？别急啊，其实里面的坑还是挺多的，且待我一个个填上。
<!--more-->

要实现什么样的效果呢，如图：

![](http://7xinvi.com1.z0.glb.clouddn.com/css4-01.jpg)
![](http://7xinvi.com1.z0.glb.clouddn.com/css4-02.jpg)

### 通过左浮动实现
1. 要实现这样一个布局，我们首先需要如下的 HTML：

	``` html
	<div id="float-container">
     <div class="float-element"></div>
     <div class="float-element"></div>
     <div class="float-element"></div>
     <div class="float-element"></div>
     <div class="float-element"></div>
     <div class="float-element"></div>
     <div class="float-element"></div>
     <div class="float-element"></div>
     <div class="float-element"></div>
	</div>
	```
2. 然后开始写 CSS 吧：

	``` css
	#float-container {
	    background-color: red;
	}
	.float-element {
	    float: left;
	    width: 50px;
	    height: 30px;
	    background-color: blue;
	    margin: 10px;
	}
	```
	使每个小容器向左浮动，再设定一个边距，一个根据浏览器宽度自适应变化位置的浮动布局就搞定收工了？当然不会，效果会像这样：
	
	![error-float](http://7xinvi.com1.z0.glb.clouddn.com/css4-03.png)
	
3. 说好的红色背景大容器呢，怎么躲起来啦？检查一番，原来是忘了给大容器 div 设定宽高度了，那就设定一下宽高度。既然要求了大容器自适应，那么我们就分别设定为 100% 吧：

	``` css
	#float-container {
	    background-color: red;
	    height: 100%;
	    width: 100%;
	}
	.float-element {
	    float: left;
	    width: 50px;
	    height: 30px;
	    background-color: blue;
	    margin: 10px;
	}
	```
	好啦，刷新一下。大容器咋还是没出来？

4. 现在让我们分析一下吧：
	- 为何看不见大容器？
	
		因为 div 如果没有包裹元素的话，百分比宽高度是不会产生效果的。

	- 那么为什么大容器明明包裹着九个 div，百分比宽高度却没有产生效果呢？

		因为小容器都设定为左浮动，已经脱离文档流，大容器并没有包围小容器，表现出高度为0（高度塌陷）。
	
	- 接下来我们想要大容器自适应，又不想使小容器失去浮动的特性，能够随着宽度变化自动调整每一行的个数，应该怎么办？

		我们需要闭合浮动元素，使其包含框表现出正常的高度。是时候请出我们的 BFC 大神啦，对，我已经连续三篇实例博文提到 BFC 原理了，因为这个原理就是这么有用啊（该处原理的详细介绍请参考一丝大神的[《那些年我们一起清除过的浮动》](http://www.iyunlu.com/view/css-xhtml/55.html)）。现在我们在大容器加上 `overflow: hidden;` 就可以自动清理其包含的任何浮动元素，接下来看看最终的 DEMO，试着调整一下宽度，是不是已经实现了想要的效果？

	<p data-height="265" data-theme-id="0" data-slug-hash="pgVomb" data-default-tab="html,result" data-user="honoka" data-embed-version="2" data-pen-title="float-container" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/pgVomb/">float-container</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### 通过 inline-block 实现
只能通过左浮动完成这个实例？并不是，我们还有 inline-block。inline-block 可以像行内元素一样水平地依次排列，但框的内容仍然符合块级框的行为。通过这一特性，我们可以利用 inline-block 像浮动一样创建很多网格铺满容器，并且不需要清除浮动。当然整个布局过程也比左浮动简便了一些，如下面的 DEMO：

<p data-height="265" data-theme-id="0" data-slug-hash="WrJvwd" data-default-tab="html,result" data-user="honoka" data-embed-version="2" data-pen-title="inline-block-container" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/WrJvwd/">inline-block-container</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

 
其实还有更加简便的方案，使用 `flex`，这里提供一个思路给大家看看如何实现（笑）

最后感谢大家的阅读，欢迎前往我的 [repo](https://github.com/nitta-honoka/baiduIFE_practice/tree/master/2015_spring/Intermediate/task0001) 查看源代码整理，有任何问题也请尽情向我吐槽。