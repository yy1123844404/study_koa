2018-5-21  关于koa

koa是一种简单好用的web框架。它特点是优雅，简洁，表达力强，自由度高。本身代码只有1000多行，所有功能都能通过插件实现。

首先：准备

1、检查node版本
	node -v
	v8.1.2
2、koa必须使用7.6以上的版本。如果版本低于这个要求，就要升级node。
	然后，克隆配套示例库：https://github.com/ruanyf/koa-demos。  （如果不方便使用git，也可以下载ZIP文件解压）。

	git clone https://github.com/ruanyf/koa-demos.git
	
然后，进入示例库，安装依赖

	cd koa-demos
	npm install
所有的示例源码：https://github.com/ruanyf/koa-demos/tree/master/demos

正式开始：

壹：基本用法

	
1.1架设HTTP服务


只要三行代码，就可以用koa 架设一个HTTP服务。		


		//demos/01.js
		const Koa = require('koa');
		const app = new Koa();
		
		app.listen(3000);

	
然后运行这个脚本
	
		node demos/01.js
	
打开浏览器，访问http://127.0.0.1:3000.就能看到页面显示的内容--》  "Not Found"，表示没有发现任何内容。因为我们并没有告诉我们的koa应该显示什么内容。
	
	
	