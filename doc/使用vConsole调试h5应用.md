### vConsole 介绍
由于移动端项目在手机中调试时不能使用chrome的控制台，用 Chrome DevTools 连接手机操作又太过复杂。而vconsole是对pc端console的改写,用来调试移动端网页的工具。

vConsole 是腾讯推出的一个轻量、可拓展、针对手机网页的前端开发者调试面板。

### 功能
+ 查看 console 日志
+ 查看网络请求
+ 查看页面 element 结构
+ 查看 Cookies、localStorage 和 SessionStorage
+ 手动执行 JS 命令行
+ 自定义插件

#### 使用方法

##### 多页面应用
在每个页面
<pre><code>&lt;script src="path/to/vconsole.min.js"&gt;&lt;/script&gt;
&lt;script&gt;
  // init vConsole
  var vConsole = new VConsole();
  console.log('Hello world');
&lt;/script&gt; </code></pre>

#### 单页应用
使用 npm 安装：
<pre><code>npm install  vconsole</code></pre>

在项目的入口引用这个文件
<pre><code>import VConsole from 'vconsole/dist/vconsole.min.js' //import vconsole
let vConsole = new VConsole() // 初始化</code></pre>

或者找到这个模块下面的 dist/vconsole.min.js ，然后复制到自己的项目中:

<pre><code>&lt;head&gt;
    &lt;script src="dist/vconsole.min.js"&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;!--建议在 `&lt;head&gt;` 中引入哦~ --&gt;
&lt;script&gt;
  // 初始化
  var vConsole = new VConsole();
  console.log('VConsole is cool');
&lt;/script&gt;</code></pre>

### 项目托管地址
[vConsole ](https://github.com/Tencent/vConsole)

同类产品还有开源项目 [Eruda](https://github.com/liriliri/eruda)，功能上略胜于vConsole
