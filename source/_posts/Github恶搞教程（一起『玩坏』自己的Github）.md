---
title: Github 恶搞教程（一起『玩坏』自己的 Github 吧）
date: 2015-10-10 00:14:13
categories: 趣味生活
tags: Git, JustForFun
---

最近在伯乐在线读到一篇趣文，[《如何在 Github『正确』做贡献》](http://blog.jobbole.com/48809/)，里面各种能人恶搞 Github 的『Public contributions』，下面截取几个小伙伴的战绩：

![fun1](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-1.jpg)

![fun2](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-2.jpg)

顺藤摸瓜，发现原来有人已经做出『玩坏』Github 的工具啦，名叫 [gitfiti](https://github.com/gelstudios/gitfiti)。主要对应预先定义的模板，进行相应日期的 commit 操作，push 至 Github 后在贡献栏中生成相应像素点，并且利用 Github 贡献数不同颜色深度不同的机制，就可以在自己的贡献栏里面看见像素画了。怎么样，是不是心动啦，那么下面就让我们开始『玩坏』之旅吧。

<!--more-->

1. 首先得将插件下载到本地，有 Git 经验的朋友可以直接 clone 这个 repo

	```
	git clone git@github.com:gelstudios/gitfiti.git
	```
	或者点击 Github 页面的下载链接将整个项目下载到本地

	![download](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-3.png)

2. 下载的同时在自己的 Github 中创建一个新的 repo，名字自取，不要创建 license 和 readme（防止后面 push 的时候产生冲突）。
3. 下载到本地后在命令行中运行 gitfiti.py，显出欢迎界面，此时第一条交互信息不用填写什么内容，直接回车即可。

	![](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-4.png)
4. 接着输入自己的 Github 用户名和刚刚新建的 repo 名。
 
	![](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-5.png)
	
	![](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-6.png)
	
5. 然后选择从哪里开始绘图，输入一个数字，代表从贡献栏的第几周开始（从左开始数），如果此处不输入直接回车则默认从最左边开始。
	
	![](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-7.png)

6. 接下来会提醒你，对比你已有的贡献后，一天中最大的贡献数是多少，然后让你选择本次绘图生成的像素点的最大贡献数（Github 的像素块颜色机制为贡献相对越大的那天颜色越深）。建议此处直接使用自己的最大贡献数，否则自己之前的贡献就全部变成浅绿了。

	![](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-8.png)
	
	此处输入 gitfiti 就表示使用自己的最大贡献数。

7. 然后就可以选择绘图模板了，此处可以使用自定义模板或者开发者已经设定好的模板。

	![](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-9.png)

	要使用自定义模板就在上面那一行中输入模板的文件路径，自定义模板的方法可以参见[该项目的说明页](https://github.com/gelstudios/gitfiti)。如果使用开发者设定的模板，上面一行就直接回车，然后下面选择模板，输入心仪的模板名字（模板名对应图案效果同样参见项目说明页）。

8. 一切搞定后，项目会自动生成一个 shell 脚本 gitfiti.sh，接下来运行这个脚本便可以自动commit 并 push 至你新建的那个 repo，等待一段时间，你便能在自己的贡献栏看见有趣的像素画啦。

	![](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-10.png)

	**注意**：此处可能会遇见两个问题

	- 生成的 shell 脚本没有运行权限。直接运行一下 `chmod 777 gitfiti.sh` 即可。
	- push 不成功，一般是因为新 repo 里面已经有文件，push 产生冲突。可以打开 gitfiti.sh，修改最后一行

		```
		git push -u origin master //改为下面这行
		git push -f -u origin master //如果有冲突强制合并
		```
9. 晒晒自己的战果（这次『恶搞』生成了 4W+ 的贡献o(╯□╰)o），也欢迎来我的 Github 看看。

	![](http://7xinvi.com1.z0.glb.clouddn.com/gitfun-11.png)

10. 最后如果想取消这个效果，直接删除创建的 repo，贡献栏和贡献数就会回归正常。

祝大家玩得愉快！