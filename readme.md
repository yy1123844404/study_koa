2018-5-21  关于koa

koa是一种简单好用的web框架。它特点是优雅，简洁，表达力强，自由度高。本身代码只有1000多行，所有功能都能通过插件实现。

首先：准备

1、检查node版本
	node -v
	v8.1.2
2、koa必须使用7.6以上的版本。如果版本低于这个要求，就要升级node。
	
然后，进入示例库，安装依赖

	cd koa-demos
	npm install

正式开始：

# 壹：基本用法 #

	
## 1.1架设HTTP服务 ##


只要三行代码，就可以用koa 架设一个HTTP服务。		


		const Koa = require('koa');
		const app = new Koa();
		
		app.listen(3000);

	
然后运行这个脚本。
	
	
打开浏览器，访问http://127.0.0.1:3000.就能看到页面显示的内容--》  "Not Found"，表示没有发现任何内容。因为我们并没有告诉我们的koa应该显示什么内容。
	
## 1.2 Context 对象 ##

Koa提供了一个Context对象，表示一次对话的上下文（包括HTTP请求和HTTP回复）。通过加工这个对象，就可以控制返回给用户的内容。

*Context.response.body* 属性就是发送给用户的内容。看下面的例子


	// demos/02.js
	const Koa = require('koa');
	const app = new Koa();
	
	const main = ctx => {
	  ctx.response.body = 'Hello World';
	};
	
	app.use(main);
	app.listen(3000);

上面的代码中，main 函数用来设置 ctx.response.body。然后使用 app.use 方法加载 main函数。

你可能已经猜到了，ctx.response 代表 HTTP Response。同样地，ctx.request代表HTTP Request.

运行这个代码，然后访问http://127.0.0.1:3000 就能看到页面上出现hello world

## 1.3 HTTP Response 的类型 ##

Koa默认的返回类型是 text/plain,如果想要返回其他类型的内容，就可以先用 ctx.request.accepts 判断一下，客户端希望接受市民类型的数据（根据HTTP Request的Accept字段），然后使用 ctx.response.type指定返回类型。可以看一下下面的例子。

	// demos/03.js
	const main = ctx => {
	  if (ctx.request.accepts('xml')) {
	    ctx.response.type = 'xml';
	    ctx.response.body = '<data>Hello World</data>';
	  } else if (ctx.request.accepts('json')) {
	    ctx.response.type = 'json';
	    ctx.response.body = { data: 'Hello World' };
	  } else if (ctx.request.accepts('html')) {
	    ctx.response.type = 'html';
	    ctx.response.body = '<p>Hello World</p>';
	  } else {
	    ctx.response.type = 'text';
	    ctx.response.body = 'Hello World';
	  }
	};

然后运行这个demo，并访问http://127.0.0.1:3000 ，然后就能看到这是一个XML 文档

## 1.4 网页模板 ##

在实际的开发中，返回给用户的网页基本上都是写成模板文件。我们可以先让Koa读取这个模板文件并将这个模板文件返回给用户。

	// demos/04.js
	const fs = require('fs');
	
	const main = ctx => {
	  ctx.response.type = 'html';
	  ctx.response.body = fs.createReadStream('./demos/template.html');
	};

