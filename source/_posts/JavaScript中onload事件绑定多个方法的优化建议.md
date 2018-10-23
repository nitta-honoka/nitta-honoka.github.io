---
title: JavaScript 中 onload 事件绑定多个方法的优化建议
date: 2015-10-17 00:02:30
categories: 前端技术
tags: JavaScript
---
onload 事件中多个方法处理的优化方案

<!--more-->

页面加载完毕时会触发 onload 事件。基于内容（HTML）要与行为（JavaScript）分离的编码思想，我们需要将一些对页面的初始化操作写在方法内，并通过 `window.onload = functionName` 调用这些方法.

需要调用多个方法时，若使用

``` javascript
window.onload = functionA;
window.onload = functionB;
```

它们之中只有最后一个方法会被实际调用。那么应如何实现调用多个方法呢？

1. 直接在 HTML 中编写:

	``` html
<body onload="functionA();functionB()">
```
由于事件包含在 HTML 内，不符合上述『内容与行为分离』的思想，故不推荐使用这个方法。

2. 创建一个匿名函数容纳需要调用的方法，然后将该匿名函数绑定到 onload 事件上：

	``` javascript
window.onload = function () {
  functionA();
  functionB();
}
```
在需要调用的函数不是太多的时候，这是最简单的解决方案了。

3. 当需要调用的方法较多时，我们可以进一步优化，编写一个专门用于绑定 onload 事件的方法：

	``` javascript
	function addLoadEvent(func) {
    	//把现有的 window.onload 事件处理函数的值存入变量
    	var oldOnload = window.onload;
    	if (typeof window.onload != "function") {
      		//如果这个处理函数还没有绑定任何函数，就像平时那样添加新函数
      		window.onload = func;
    	} else {
      		//如果处理函数已经绑定了一些函数，就把新函数添加到末尾
      		window.onload = function() {
        		oldOnload();
        		func();
      		}
      	}
  	}
  
  //接下来，我们只需要调用这个方法添加自己需要的函数就行了
  addLoadEvent(functionA);
  addLoadEvent(functionB);
  	```
现在不管代码变得多么复杂，当我们需要在页面加载完毕时调用多少函数，只需要多写一条语句既可解决。