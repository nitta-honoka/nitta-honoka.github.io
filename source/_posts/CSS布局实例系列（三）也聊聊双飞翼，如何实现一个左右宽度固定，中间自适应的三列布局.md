---
title: CSS 布局实例系列（三）也聊聊双飞翼，如何实现一个左右宽度固定，中间自适应的三列布局
date: 2016-01-27 00:50:15
categories: 前端技术
tags: CSS
---

今天聊聊一个经典的布局实例：

> 实现一个三列布局，其中左侧和右侧的部分宽度固定，中间部分宽度随浏览器宽度的变化而自适应变化

可能很多朋友已经笑了，这玩意儿通过双飞翼布局就能轻松实现。不过，还请容我在双飞翼之外，循序渐进地介绍一下我们可以如何实现一个三列布局。

<!--more-->

### 首先，使用浮动布局来实现
<p data-height="265" data-theme-id="0" data-slug-hash="MKVMoa" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="float-three-column" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/MKVMoa/">float-three-column</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

- 左侧元素与右侧元素优先渲染，分别向左和向右浮动
- 中间元素在文档流的最后渲染，并将 width 设为 100%，则会自动插入到左右两列元素的中间，随后设置 margin 左右边距分别为左右两列的宽度，将中间元素调整到正确的位置。

这是一种比较便利的实现方式，无需额外的元素辅助定位，同时兼容性也比较优秀。但有一个缺点就是该布局方式只能实现左右两列宽度固定，中间自适应这一种三列布局，灵活性不强。

### 其实，也可以试试利用 BFC
<p data-height="265" data-theme-id="0" data-slug-hash="vLRRPv" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="bfc-three-columns" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/vLRRPv/">bfc-three-columns</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

之前的文章《CSS 布局实例系列（二）如何通过 CSS 实现一个左边固定宽度、右边自适应的两列布局》已经谈到了利用 BFC 原理实现多列布局的方法。BFC 元素不会与浮动元素叠加，自然也可以完成这个实例。

- 同样的左右两列元素优先渲染，并分别左右浮动。
- 接下来将中间元素设置 overflow: hidden; 成为 BFC 元素块，不与两侧浮动元素叠加，则自然能够插入自己的位置啦。

### 接下来就尝试一下大名鼎鼎的双飞翼布局
<p data-height="265" data-theme-id="0" data-slug-hash="rxdpJK" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="grid-three-columns" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/rxdpJK/">grid-three-columns</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

双飞翼是由淘宝玉伯等前端大牛提出的一种多列布局方法，主要利用了浮动、负边距、相对定位三个布局属性，使三列布局就像小鸟一样，拥有中间的身体和两侧的翅膀。

接下来就简单介绍一下双飞翼的实现过程：

1. 假设我们现在需要一个如实例说明一样的三列布局，写出如下 div 结构：

	``` html
	<div class="grid">
	    <div id="div-middle-02"><span>div-middle</span></div>
	    <div id="div-left-02"><span>div-left</span></div>
	    <div id="div-right-02"><span>div-right</span></div>
	</div>
	```
	
2. 首先我们将中间元素放在文档流最前面优先渲染，然后使其向左浮动，并设置 width 为 100%：

	``` css
	#div-middle-02 {
	    float: left;
	    background-color: #fff9ca;
	    width: 100%;
	    height: 50px;
	}
	```
	中间元素直接占满全列，形成小鸟的身体。

3. 接下来我们开始为小鸟加上双翼，将左右两列元素均设为左浮动，然后通过调整负边距将其定位在各自的位置上：

	``` css
	#div-middle-02 {
	    float: left;
	    background-color: #fff9ca;
	    width: 100%;
	    height: 50px;
	}
	#div-left-02 {
	    float: left;
	    background-color: red;
	    width: 150px;
	    margin-left: -100%;
	    height: 50px;
	}
	#div-right-02 {
	    float: left;
	    background-color: yellow;
	    width: 200px;
	    margin-left: -200px;
	    height: 50px;
	}
	```
	看起来，双翼安装成功啦。
	
	![](http://7xinvi.com1.z0.glb.clouddn.com/css3-01.png)

4. 这样三列布局就大功告成了？No，no，no，仔细看看上面的效果图，可以发现 div-middle 的字块消失了。这是因为通过负边距调整浮动元素位置时，会产生层叠的效果，上面的布局其实只是左右两列元素分别定位在自己的位置上并覆盖中间元素的那部分而已，中间元素的定位并未成功。中间元素要怎样定位在自己的位置上呢？小鸟的身体不是还缺少骨架嘛，那么我们在小鸟体内加上骨架吧：

	``` html
	<div id="div-middle-02">
	    <div id="middle-wrap-02"><span>div-middle</span></div>
	</div>
	```
	在中间元素中再增加一层包裹，通过这层骨架我们就可以方便地控制小鸟身体的位置啦，方法就是调整骨架的左右边距，使其分别等于左右两列的宽度：
	``` css
	#div-middle-02 {
	    float: left;
	    background-color: #fff9ca;
	    width: 100%;
	    height: 50px;
	}
	
	#middle-wrap-02 {
	    margin: 0 200px 0 150px;
	}
	
	#div-left-02 {
	    float: left;
	    background-color: red;
	    width: 150px;
	    margin-left: -100%;
	    height: 50px;
	
	}
	
	#div-right-02 {
	    float: left;
	    background-color: yellow;
	    width: 200px;
	    margin-left: -200px;
	    height: 50px;
	}
	```
	好啦，一个左右定宽，中间自适应的三列布局以双飞翼的方式成功完成。

5. 总结整个过程，就是先放好身体，再加上翅膀，然后让身体包裹一层骨架，通过骨架将身体定位到正确的位置。这就是双飞翼布局的完全体吗？当然不是，接下来我们要请出大杀器相对布局啦，就像小鸟可以通过各种不同的姿势飞翔一般，通过 `position: relative;` 双飞翼可以实现任意的三列或双列布局。本实例加上相对定位，便成为了这样的完全体：

	``` css
	#div-middle-02 {
	    float: left;
	    background-color: #fff9ca;
	    width: 100%;
	    height: 50px;
	}
	
	#middle-wrap-02 {
	    margin: 0 200px 0 150px;
	}
	
	#div-left-02 {
	    float: left;
	    position: relative;
	    background-color: red;
	    width: 150px;
	    margin-left: -100%;
	    height: 50px;
	
	}
	
	#div-right-02 {
	    float: left;
	    position: relative;
	    background-color: yellow;
	    width: 200px;
	    margin-left: -200px;
	    height: 50px;
	}
	```

6. 双飞翼能够兼容到 IE6，其可以实现的各种布局在此便不作展开了，有兴趣可以参考玉伯分享的 [DEMO](http://www.dqqd.me/avatar/fly/grids_test1.html)

### 跟上潮流，试试 flex
<p data-height="265" data-theme-id="0" data-slug-hash="zrWRzg" data-default-tab="css,result" data-user="honoka" data-embed-version="2" data-pen-title="flex-three-columns" class="codepen">See the Pen <a href="https://codepen.io/honoka/pen/zrWRzg/">flex-three-columns</a> by xal821792703 (<a href="https://codepen.io/honoka">@honoka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

看完了强大的双飞翼布局，是不是已经心急火燎，想亲手试试啦。别急，客官，再听我唠唠 CSS3 的新布局 flex 呗。先让我说明一下上面的 DEMO 中是怎样实现本次实例的：

1. 设计一个弹性容器包裹需定位的三个元素，然后将该弹性容器的排列属性设为水平排列（flex-flow: row）
2. 现在三个元素已经是三列布局了，再将三列元素分别设定一下宽度就行了，左右元素设定为定宽，自适应的中间元素设定为 100%。

	``` css
	.flex {
	    display: flex;
	    flex-flow: row;
	}
	
	#div-left-03 {
	    background-color: red;
	    width: 150px;
	    height: 50px;
	}
	
	#div-middle-03 {
	    background-color: #fff9ca;
	    width: 100%;
	    height: 50px;
	}
	
	#div-right-03 {
	    background-color: yellow;
	    width: 200px;
	    height: 50px;
	}
	```

3. 搞定收工！大哥你瞪着我是怎么回事儿？~ 什么？效果不对？我的代码怎么可能不对？！哎呦，别打我，我马上检查[捂脸]好吧，宽度不对，左右两侧的宽度均不符合设定的定值。什么情况呢？原来在 flex 布局中不能将被定位的元素宽度或高度设定为 100%，这样会影响其他定值大小的元素。那么该如何设置中间元素的宽度呢，`flex: 1;` 即可，可以试一下 DEMO 中去掉注释与不去掉的区别。

4. 最后简单介绍一下 flex：flex 是 CSS3 的一种弹性容器布局，通过 flex，几行简单的 CSS 语句便可以实现各种布局（对！我就是 flex NC粉~被拍飞~）。那么 flex 有什么缺点呢？兼容性！

5. 所以在使用 flex 的时候还请注意是否兼容当前浏览器，是否需要 -webkit- 标签。flex 的具体语法和各类实例因为篇(lan)幅(de)过(xie)多的原因，也不做过多介绍了，可以参考阮一峰老师的博文：

	- [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
	- [Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

最后感谢大家的阅读，欢迎前往我的 [repo](https://github.com/nitta-honoka/baiduIFE_practice/tree/master/2015_spring/Intermediate/task0001) 查看源代码整理，有任何问题也请尽情向我吐槽。