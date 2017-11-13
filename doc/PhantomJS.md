PhantomJS是一个基于webkit的JavaScript API。它使用QtWebKit作为它核心浏览器的功能，使用webkit来编译解释执行JavaScript代码。任何你可以在基于webkit浏览器做的事情，它都能做到。它不仅是个隐形的浏览器，提供了诸如CSS选择器、支持Web标准、DOM操作、JSON、HTML5、Canvas、SVG等，同时也提供了处理文件I/O的操作，从而使你可以向操作系统读写文件等。PhantomJS的用处可谓非常广泛，诸如网络监测、网页截屏、无需浏览器的 Web 测试、页面访问自动化等。

参考信息：
[PhantomJS快速入门](http://www.cnblogs.com/front-Thinking/p/4321720.html)

[PhantomJS](http://www.cnblogs.com/firstdream/p/5122798.html)

[phantomjs 中文文档](http://www.cnblogs.com/bangejingting/p/6907628.html)

## PhantomJS的应用
[Highcharts结合PhantomJS在服务端生成高质量的图表图片](http://www.cnblogs.com/jasondan/p/3504120.html)

[nodejs搭配phantomjs highcharts后台生成图表](http://www.cnblogs.com/kenkofox/p/4959797.html)

[用webdriver+phantomjs实现无浏览器的自动化过程](http://www.cnblogs.com/LanTianYou/p/5578621.html)

[利用PhantomJS进行网页截屏，完美解决截取高度的问题](http://www.cnblogs.com/jasondan/p/4108263.html)

[Selenium + PhantomJS + python 简单实现爬虫的功能](http://www.cnblogs.com/luxiaojun/p/6144748.html)

[生成PDF的新选择-Phantomjs](http://www.cnblogs.com/whitewolf/p/3468120.html)

[PhantomJS在Windows7下实现网站自动下载截图](http://www.cnblogs.com/huangcong/archive/2013/04/18/3027654.html)

[动态爬虫——selenium2搭载phantomjs入门范例](http://www.cnblogs.com/chenqingyang/p/3772673.html)

应用场合：
+ web测试,主要是可以又轻又快捷的进行web测试，还不用去再去依赖浏览器，用过selenium的朋友都知道，打开一个浏览器是多么痛苦的事，特别是FireFox，并且他支持很多测试框架，比如RobotFrame， WebDrive等。
+ 页面自动化渲染.可以通过标准的domApi来操作页面元素，并且，你也可以注入Jquery，这样就可以通过jquery来操作页面元素了.
+ 屏幕捕捉,这个好。有的时候case失败了，想捕捉屏幕的时候，用selenium自带的捕捉老是遇到浏览器兼容问题，用这个来捕捉应该会稳定很多、
+ 网络监视.这个我接触的不多，主要说是可以自动分析页面的加载速度，并且还可以导出标准的HAR格式文件。
