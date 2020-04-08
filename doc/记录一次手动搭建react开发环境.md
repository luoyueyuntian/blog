## 初始化项目
##### 新建一个空目录，初始化项目
<pre><code>npm init</code></pre>
按照提示一步步执行，设置项目名称等信息，大多数可使用默认，即可创建项目的`package.josn`文件

## 安装开发环境依赖
##### 安装webpack
<pre><code>npm install webpack webpack-cli webpack-dev-server -save-dev</code></pre>
+ `webpack`：打包工具
+ `webpack-cli`：webpack脚手架工具
+ `webpack-dev-server`：本地开发服务，本地开发时，可实现静态资源服务器，本地请求代理到远程服务器，不需要开发完后上传到服务器上才能看到效果

##### 安装各种文件的loader
<pre><code>npm install --save-dev babel-loader babel-core babel-preset-env</code></pre>
注意：`babel` 和 `babel-loader`版本要匹配
+ `babel-loader 8.x | babel 7.x`
+ `babel-loader 7.x | babel 6.x`
<pre><code>npm install --save-dev babel-runtime babel-plugin-transform-runtime</code></pre>
<pre><code>npm install --save-dev less css-loader less-loader file-loader</code></pre>
如果使用到了`less`，安装webpack官方文档上只安装loader是不行的，需要额外在安装`less`才可以正常使用less

##### 安装插件
清除上一次打包编译结果
<pre><code>npm install --save-dev clean-webpack-plugin</code></pre>
将编译后的代码插入到HTML文档中
<pre><code>npm install --save-dev html-webpack-plugin</code></pre>

##### 安装工具类
<pre><code>npm install --save-dev webpack-merge</code></pre>

## 安装前端使用的框架和类库
##### 安装react
<pre><code>npm install react -save</code></pre>

##### 安装react-dom
<pre><code>npm install react-dom -save</code></pre>

##### 安装react-redux
<pre><code>npm install react-redux -save</code></pre>

##### 安装react-router-dom
<pre><code>npm install react-router-dom -save</code></pre>

##### 安装normalize.css
<pre><code>npm install normalize.css -save</code></pre>

## 配置webpack.config.js文件，搭建开发环境
##### 配置文件入口
改模板只有一个入口，因此简单配置如下：
<pre><code>entry: path.resolve('./src/index.js')</code></pre>
项目的入口文件根据实际来配，如在`./src/index.js`为入口文件时，配置如上，其他的文件可修改为相应文件目录

##### 配置module
webpack只能处理js文件，对于`html`、`css`、`png\jpg\gif`等其他文件，需要配置对应的loader，webpack才能处理。

<pre><code>module: {
    rules: [
        // 处理CSS文件
        { test: /\.css$/, use: 'css-loader' },
        // 处理Less文件，处理的顺序是从后往前，依次是less-loader、css-loader、style-loader
        {
            test: /\.less$/,
            use: [
                { loader: 'style-loader', },
                { loader: 'css-loader' },
                { loader: 'less-loader' }
            ]
        },
        // 处理js文件，使用babel-loader是为了将js输出为es5的语法，方便开发过程中直接使用es6语法，而不用担心浏览器对于es6语法的支持
        {
            test: /\.js$/,
            exclude: /node_modules/,
            use: {
                loader: 'babel-loader',
                options: {
                    presets: [require.resolve('babel-preset-react-app')]
                }
            }
        },
        // 处理图片文件
        {
            test: /\.(png|svg|jpg|gif)$/,
            use: [
                'file-loader'
            ]
        },
        // 处理字体文件，可根据需要，如果项目中没有用到字体，可以不配，当然配了也没有问题，建议配上
        {
            test: /\.(woff|woff2|eot|ttf|otf)$/,
            use: [
                'file-loader'
            ]
        }
    ]
}
</code></pre>

##### 配置开发工具
开发工具要配合`webpack-dev-server`使用，在这里配置的选择会影响到`webpack-dev-server`的行为
<pre><code>devServer: {
    hot: true,
    open: true,
    port: 8800
}
+ `hot: true`表示启用 webpack 的模块热替换特性，当文件修改后，webpack自动编译更新后会自动替换文件内容，浏览器不需要刷新就可以看到修改后的效果
+ `open: true`表示启动`webpack-dev-server`后自动打开浏览器，并加载默认页面
+ `port: 8800`表示打开的端口，可以不配置，`webpack-dev-server`会自动使用一个未使用的端口来加载页面

其他一些有用的配置：
+ `https: true`: 使用https协议
+ `openPage: '/different/page'`: 指定启动`webpack-dev-server`时，打开的浏览器默认页面路径
+ `proxy: { "/api": "http://localhost:3000" }`: 可以将请求代理到服务器，一般都需要配置这个，可以将本地的后端请求代理到指定的服务器，这样可以在后端修改后还没有上传到服务器时，将请求代理到后端开发人员的服务器上，也可以将请求转发到服务器上，这样就可以前端代码使用本地，后端服务使用远程服务器上的，类似的也可以使用nginx，不过修改后要每次编译替换静态资源文件，并且要自己手动刷新页面，没有这个方便。
</code></pre>

##### 配置打包输出

##### 代码压缩

##### 代码插件

