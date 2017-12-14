# PhantomJS学习总结
## 1、PhantomJS是什么？
PhantomJS是一个基于webkit的JavaScript API。它使用QtWebKit作为它核心浏览器的功能，使用webkit来编译解释执行JavaScript代码。任何你可以在基于webkit浏览器做的事情，它都能做到。它不仅是个隐形的浏览器，提供了诸如CSS选择器、支持Web标准、DOM操作、JSON、HTML5、Canvas、SVG等，同时也提供了处理文件I/O的操作，从而使你可以向操作系统读写文件等。PhantomJS的用处可谓非常广泛，诸如网络监测、网页截屏、无需浏览器的 Web 测试、页面访问自动化等。

PhantomJS官方地址：http://phantomjs.org/。

PhantomJS官方API：http://phantomjs.org/api/。

PhantomJS官方示例：http://phantomjs.org/examples/。

PhantomJS GitHub：https://github.com/ariya/phantomjs/。

## 2、PhantomJS下载与安装
官方下载地址：http://phantomjs.org/download.html。目前官方支持三种操作系统，包括windows\Mac OS\Linux这三大主流的环境。

下载完成后解压文件，单独放在一个文件夹里，比如D:\workspace\phantomjs。

双击运行phantomjs.exe，在弹出的命令行工具里面可以输入js代码运行

将phantomjs.exe添加到环境变量，可以使用

## 3、PhantomJS核心API
### 常用内置几大对象
1.system:获得系统操作对象，包括命令行参数、phantomjs系统设置等信息
<pre><code>var system=require('system');</code></pre>

2.webpage:获取操作dom或web网页的对象，通过它可以打开网页、接收网页内容、request、response参数，其为最核心对象。提供了一套可以访问和操作web文档的核心方法，包括操作DOM、事件捕获、用户事件模拟等等。
<pre><code>var page = require('webpage');</code></pre>

3.fs:获取文件系统对象，通过它可以操作操作系统的文件操作，包括read、write、move、copy、delete等。
<pre><code>var fs = require('fs');</code></pre>

4.webserver:可以基于它来实现自己的webserver，用来处理请求并且执行PhantomJS代码等。

5.其他详见官网：[http://phantomjs.org/api/phantom/](http://phantomjs.org/api/phantom/)

### 常用API
1.通过page对象打开url链接，并可以回调其声明的回调函数，其回调发生的时机为该URL被彻底打开完毕，即该URL所引发的请求项被全部加载完，但ajax请求是与它的加载完成与否没有关系
<pre><code>page.open(url,function (status) {})</code></pre>

2.当page.open调用时，回首先执行该函数，在此可以预置一些参数或函数，用于后边的回调函数中
<pre><code>page.onLoadStarted = function() {}</code></pre>

3.page的所要加载的资源在加载过程中，出现了各种失败，则在此回调处理
<pre><code>page.onResourceError = function(resourceError) {}</code></pre>

4.page的所要加载的资源在发起请求时，都可以回调该函数
<pre><code>page.onResourceRequested = function(requestData, networkRequest) {}</code></pre>

5.page的所要加载的资源在加载过程中，每加载一个相关资源，都会在此先做出响应，它相当于http头部分,  其核心回调对象为response，可以在此获取本次请求的cookies、userAgent等
<pre><code>page.onResourceReceived = function(response) {}</code></pre>

6.欲在执行web网页时，打印一些输出信息到控制台，则可以在此回调显示。
<pre><code>page.onConsoleMessage = function (msg) {}</code></pre>

7.phantomjs是没有界面的，所以对alert也是无法直接弹出的，故phantomjs以该函数回调在page在执行过程中的alert事件
<pre><code>page.onAlert = function(msg) {}</code></pre>

8.当page.open中的url，它自己（不包括所引起的其它的加载资源）出现了异常，如404、no route to web site等，都会在此回调显示。
<pre><code>page.onError = function(msg, trace) {}</code></pre>

9.当page.open打开的url或是该url在打开过程中基于该URL进行了跳转，则可在此函数中回调。
<pre><code>page.onUrlChanged = function(targetUrl) {}</code></pre>

10.当page.open的目标URL被真正打开后，会在调用open的回调函数前调用该函数，在此可以进行内部的翻页等操作
<pre><code>page.onLoadFinished = function(status){}</code></pre>

11.在所加载的web page内部执行该函数，像翻页、点击、滑动等，均可在此中执行
<pre><code>page.evaluate(function(){});</code></pre>

12.将当前page的现状渲染成图片，输出到指定的文件中去。
<pre><code>page.render("");</code></pre>

## 4、注意事项
+ 1、区分phantomjs的对象和打开的web page的对象，如document、window等，两者都有，在调用page.evaluate和不调用的时候，注意区分二者的范围，容易在调试时出现很多的问题，且不好发现。

+ 2、page.injectJs和page.includeJs的区别，前者侧重本地的js文件，与libraryPath挂购，后者侧重网络js文件，尤其在引入jQuery等第三方库时，会经常遇到。

+ 3、编码问题，两个重要参数，--output-encoding,--script-encoding，前者为输出编码，后者为所使用js、参数配置文件的编码，为方便起鉴，建议均采用utf-8编码，并注所应用到的目标文件的编码，以免引起很不可思议的异常，又无从查起。

## 5、执行PhantomJS的命令格式
<pre><code>phantomjs [switches] [options] [script] [argument [argument [...]]]</code></pre>
其中，各种参数都是可选的。
**示例：**<br/>
执行脚本hello.js
<pre><code>phantomjs hello.js</code></pre>
打开debug模式（该模式用于开发，可提供必要提示信息）：
<pre><code>phantomjs --debug=yes hello.js</code></pre>
设置cookie路径：
<pre><code>phantomjs --cookie-file=cookie.txt hello.js</code></pre>

## 功能示例
### 加载页面
<pre><code>var page = require('webpage').create();
page.open('http://blog.csdn.net/luanpeng825485697', function (status) {
    console.log("Status: " + status);
    if (status === "success") {
        page.render('example.png');
    }
    phantom.exit();
});
</code></pre>

### 网络监听/测试页面加载速度
<pre><code>var page = require('webpage').create(),
  system = require('system'),
  t, address;

if (system.args.length === 1) {
  console.log('Usage: loadspeed.js <some URL>');
  phantom.exit();
}

t = Date.now();
address = system.args[1];
page.open(address, function(status) {
  if (status !== 'success') {
    console.log('FAIL to load the address');
  } else {
    t = Date.now() - t;
    console.log('Loading ' + system.args[1]);
    console.log('Loading time ' + t + ' msec');
  }
  phantom.exit();
});</code></pre>
程序判断了参数的多少，如果参数不够，那么终止运行。然后记录了打开页面的时间，请求页面之后，再纪录当前时间，二者之差就是页面加载速度。
<pre><code>phantomjs loadspeed.js http://blog.csdn.net/luanpeng825485697</code></pre>

### 页面自动化处理/操作page content/事件处理
<pre><code>var url = 'http://www.baidu.com';
var page = require('webpage').create();
page.open(url, function(status) {
	page.evaluate(function() {
		/*
		可疑在这里使用DOM选择器
		常用的getElementById、getElementByClassName、
		getElementByName、getElementByTagName、querySelector（CSS选择器）。
		模仿用户点击事件等
		DOM操作
		 */
	});
	phantom.exit();
});</code></pre>
监听还没开始加载事件，获取初始加载时间：
<pre><code>var startTime = null;
page.onLoadStarted = function() {
	startTime = new Date().getTime();
}</code></pre>
监听资源文件请求事件，获取资源发起请求的时间：
<pre><code>var resources = [];
page.onResourceRequested = function(request) {
	resource = {
		"startTime": request.time,
		"url": request.url
	};
	resources[request.id] = resource;
};</code></pre>
监听资源文件加载完成事件，获取加载完成时间：
<pre><code>page.onResourceReceived = function(response) {
	if (response.stage == "start") {
		resources[response.id].size = response.bodySize;
	} else if (response.stage == "end") {
		resources[response.id].endTime = response.time;
	}
};</code></pre>
监听文档加载完成事件，记录完成时间，并打印出所有资源文件的耗时：
<pre><code>page.onLoadFinished = function() {
	endTime = new Date();
	timeInSeconds = (endTime - startTime) / 1000;
	console.log("Loading takes " + timeInSeconds + " seconds.");
	resources.forEach(function(resource) {
		st = new Date(resource.startTime).getTime();
		et = new Date(resource.endTime).getTime();
		timeSpent = (et - st) / 1000;
		console.log(timeSpent + " seconds : " + resource.url);
	});
	phantom.exit(0);
};</code></pre>

### 使用附加库
<pre><code>var page = require('webpage').create();
page.open('http://blog.csdn.net/luanpeng825485697', function() {
	page.includeJs("http://code.jquery.com/jquery-1.10.1.min.js", function() {
		page.evaluate(function() {
			//加载jquery后可以在这里使用jquery
			$("button").click();
		});
		phantom.exit()
	});
});</code></pre>

### 屏幕捕获
保存为图片：
<pre><code>var page = require('webpage').create();
page.open("http://www.cnblogs.com/front-Thinking/", function(status) {
	if (status === "success") {
		console.log(page.title);
		page.render("front-Thinking.png");
	} else {
		console.log("Page failed to load.");
	}
	phantom.exit(0);
});</code></pre>
保存为pdf:
<pre><code>var page = require('webpage').create();
page.open("http://www.baidu.com", function(status) {
	if (status === "success") {
		console.log(page.title);
		page.paperSize = {
			format: 'A4',
			orientation: 'portrait',
			border: '1cm'
		};
		page.render("front-Thinking.pdf");
	} else {
		console.log("Page failed to load.");
	}
	phantom.exit(0);
});</code></pre>

## 参考链接
[PhantomJS核心API](http://blog.csdn.net/lipinganq/article/details/72654905)

[PhantomJS快速入门](http://blog.csdn.net/libsyc/article/details/78199850)

[python网络爬虫系列教程——PhantomJS包应用全解](http://blog.csdn.net/luanpeng825485697/article/details/78428696)

无关链接，后续移到相应文档[python开发大全、系列文章、精品教程](http://blog.csdn.net/luanpeng825485697/article/details/78347433)
