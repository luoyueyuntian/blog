## 访问作用域

通过一行简单的JS程序访问页面中任何作用域（甚至是隔离的作用域！）
<pre><code>angular.element(targetNode).scope()</code></pre>

## 抓取任何服务
无论ngApp在哪里定义，我们都可以使用注入器功能来抓取任何的服务的引用（如果使用angular的bootstrap方法，则可以手动抓取$rootElement）
<pre><code>angular.element('html').injector().get('MyService')</code></pre>

## Chrome 控制台特性
Chrome浏览器的控制台有一堆不错的捷径 来调试浏览器应用。这是一些Angular开发中最好的做法：
+ $0-$4: 访问最近在查看窗口中进行选取的 5 个DOM元素。选择抓取的范围非常方便。
+ $(selector)和$$(selector): 分别是querySelector() 和 querySelectorAll的一个快速的替代
