[npm 中文官方文档](https://www.npmjs.cn/)

#### npm简介
NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

+ 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
+ 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
+ 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

由于新版的nodejs已经集成了npm，所以之前npm也一并安装好了。同样可以通过输入 "npm -v" 来测试是否成功安装。



#### 查看npm版本
<pre><code>npm -v</code></pre>

#### npm安装包
<pre><code>npm install &lt;Module Name&gt;</code></pre>

### 全局安装与本地安装
#### 本地安装
1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
2. 可以通过 require() 来引入本地安装的包。
#### 全局安装
1. 将安装包放在 /usr/local 下或者你 node 的安装目录。
2. 可以直接在命令行里使用。

下面是安装express的两种方式：
<pre><code>npm install express          # 本地安装
npm install express -g       # 全局安装</code></pre>

#### npm卸载包
<pre><code>npm uninstall &lt;Module Name&gt;    // 卸载本地安装的包
npm uninstall &lt;Module Name&gt; -g    // 卸载全局安装的包
</code></pre>

#### npm帮助
<pre><code>npm help &lt;command&gt;</code></pre>
可查看某条命令的详细帮助

例如`npm help install`。

#### npm更新包
<pre><code>npm update &lt;package&gt;  // 更新本地安装包
npm update &lt;package&gt; -g   // 更新全局安装包</code></pre>

#### npm初始化一个项目
<pre><code>npm init</code></pre>

Package.json 属性说明
+ name - 包名。
+ version - 包的版本号。
+ description - 包的描述。
+ homepage - 包的官网 url 。
+ author - 包的作者姓名。
+ contributors - 包的其他贡献者姓名。
+ dependencies - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。
+ repository - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
+ main - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
+ keywords - 关键字

#### 查看安装信息
<pre><code></code></pre>

#### 搜索
<pre><code>npm search &lt;package&gt;</code></pre>

#### 清空缓存
<pre><code>npm cache clear</code></pre>
清空NPM本地缓存，用于更新使用相同版本号发布新版本代码的包

#### npm scripts
使用npm scripts可以在package.json文件中自定义脚本
<pre><code>{
  "script": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}</code></pre>
可以使用npm run命令来执行scripts，在上面例子中，执行npm run build就等同于执行对应的npm脚本：react-scripts build

### 语义版本号
语义版本号分为X.Y.Z三位，分别代表主版本号、次版本号和补丁版本号。当代码变更时，版本号按以下原则更新。
+ 如果只是修复bug，需要更新Z位。
+ 如果是新增了功能，但是向下兼容，需要更新Y位。
+ 如果有大变动，向下不兼容，需要更新X位。

### 淘宝镜像
国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。
淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
<pre><code>npm install -g cnpm --registry=https://registry.npm.taobao.org</code></pre>
安装完成后可以使用 cnpm 命令来安装模块了：
<pre><code>cnpm install &lt;package&gt;</code></pre>

