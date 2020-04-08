## 不使用官方脚手架搭建一个react项目
### 初始化项目
##### 新建一个空目录，初始化项目
<pre><code>npm init</code></pre>
按照提示一步步执行，设置项目名称等信息，大多数可使用默认，即可创建项目的`package.josn`文件

### 安装开发环境依赖
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

### 配置webpack.config.js文件，搭建开发环境
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
},
devtool: 'inline-source-map'
</code></pre>
+ `hot: true`表示启用 webpack 的模块热替换特性，当文件修改后，webpack自动编译更新后会自动替换文件内容，浏览器不需要刷新就可以看到修改后的效果
+ `open: true`表示启动`webpack-dev-server`后自动打开浏览器，并加载默认页面
+ `port: 8800`表示打开的端口，可以不配置，`webpack-dev-server`会自动使用一个未使用的端口来加载页面

`devtool`是用来控制生产`source map`的工具，在开发环境时，我们需要使用source map来定位出问题的代码行数，生产环境则可以不用生成source map，或者生成少量的source map。


其他一些有用的配置：
+ `https: true`: 使用https协议
+ `openPage: '/different/page'`: 指定启动`webpack-dev-server`时，打开的浏览器默认页面路径
+ `proxy: { "/api": "http://localhost:3000" }`: 可以将请求代理到服务器，一般都需要配置这个，可以将本地的后端请求转发到指定的服务器，这样就可以前端代码使用本地开发环境编译的代码，后端服务使用远程服务器上的，类似的也可以使用nginx，不过修改后要每次编译替换静态资源文件，并且要自己手动刷新页面，没有这个方便。

##### 配置打包输出
这个配置是设置项目构建时的代码输出目录，后面发布上线时直接将该目录的代码部署的服务器上
<pre><code>output: {
    filename: '[name].[hash].js',
    chunkFilename: '[name].[chunkhash].js',
    path: path.resolve(__dirname, 'dist')
}
</code></pre>
+ `filename: '[name].[hash].js',`: 入口文件打包输出文件名称
+ `chunkFilename: '[name].[chunkhash].js'`: 非入口(non-entry) chunk 文件的名称s
+ `path: path.resolve(__dirname, 'dist')`:用来指定打包编译完后输出的文件目录

##### 代码压缩与提取公共代码
开发环境和生产环境对于代码的打包有不同的要求，开发环境需要一些调试信息，所以代码不要求极致压缩，但部署到生产环境时，因为用户访问服务时的网络环境是不确定的，而且我们总是希望用户能够尽快看到我们的页面，不希望页面加载时间过长，所以会对代码进行压缩，这个压缩包括去掉空行和注释，还包括代码混淆，另外通过`treeShaking`来去掉源码中没有用到的代码，可以进一步压缩代码。

开启`treeShaking`需要在`package.json` 的通过 `sideEffects` 属性来指定哪些文件是无副作用的，可以安全的删除么有引用到的代码。通过设置为`false`可以将所有代码都标记为无副作用的，也可以通过一个数组，来指定哪些代码是有副作用的，不要删除里面没有引用到的代码。

对于一些公用的模块，如果每个bundle都生成一份，显然是冗余的，可以在webpack构建时，使用插件将这些公共的代码提取到一个公共的模块中，这样就可以避免一些不必要的重复代码，在最新webpack4中，可以通过`optimization.splitChunks`配置实现这个功能：
<pre><code>optimization: {
    splitChunks: {
        splitChunks: {
            cacheGroups: {
                name: 'commons',
                chunks: 'initial',
                minChunks: 2
            }
        }
    }
}
</code></pre>
具体配置可参考[SplitChunksPlugin配置](https://webpack.js.org/plugins/split-chunks-plugin/)

##### 插件
loader只能对一种文件进行处理，而且只在文件的加载阶段处理代码，而plugin可以在构建的任何阶段对任何代码进行处理，所以plugin能够做更多的事情。构建过程中的代码压缩、代码拆分就是通过插件实现的。

+ CleanWebpackPlugin：官方推荐的删除上次编译文件的插件
+ HtmlWebpackPlugin：用于将编译完的代码通过script标签插入到HTMl文件中的插件，该插件很重要，我们编译生成的文件，文件名都是动态的，手工引入显然麻烦且不靠谱，使用该插件可以完美处理这些问题，该插件还支持自定义HTML模板，在模板中插入一些内容，控制插入的位置等配置。
+ webpack.HotModuleReplacementPlugin：热加载插件，用于开发环境不熟悉页面更新代码。
+ webpack.NamedModulesPlugin：当开启 HMR 的时候使用该插件会显示模块的相对路径，建议用于开发环境。 