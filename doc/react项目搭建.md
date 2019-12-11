新的react项目搭建使用官方脚手架`create-react-app`

安装最新的`create-react-app`
<pre><code># 全局安装
npm install -g create-react-app
</code></pre>

如果不是最新的，可以先更新脚手架
<pre><code>npm update -g create-react-app</code></pre>

如果不放心，可以先把旧的包删掉，重新安装
<pre><code>npm uninstall -g create-react-app</code></pre>

创建项目
<pre><code>create-rect-app &lt;app-name&gt;</code></pre>

创建项目需要一段时间，需要耐心等待。等创建完成后，可以将`cmd`的上下文切入到项目目录
<pre><code>cd &lt;app-name&gt;</code></pre>

这个时候项目已经具备基本功能，可以运行起来了。可以尝试如下命令
<pre><code>npm run start</code></pre>

这个时候我们可以试着改动自动生成的代码，保存后可以立马看到修改后的效果。
不过因为react只是一个视图层的框架，一个完整的web应用需要更多的功能。正常来说react与react-router和react-redux是一个标准的搭配。

引入react-router
<pre><code>npm install react-router-dom --save</code></pre>
执行完这条命令后，可以看到package.json文件中的`dependencies`熟悉会新增一个依赖`react-router-dom`，然后我们就可以在项目中使用 react-touter 。

引入react-redux
<pre><code>npm install --save react-redux</code></pre>
同理，这一行命令执行完以后是可以在package.json文件中看到一个新增的依赖

到此为止，一个react项目基本搭建完成，剩下的就是往项目里面填充业务逻辑。
