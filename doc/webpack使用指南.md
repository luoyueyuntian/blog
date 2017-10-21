---
date: 2017-10-12 22:20
status: public
title: webpack使用指南
---

以下总结来自[webpack中文文档](https://doc.webpack-china.org/guides/)，去掉了中间的详细说明部分，只保留了实际操作部分
#1.安装
在项目根目录下执行以下命令，创建开发依赖
<pre><code>npm install --save-dev webpack</code></pre>
#2.新建配置文件--webpack.config.js
<pre><code>const path = require('path')
module.exports = {
     entry: './src/index.js',
     output: {
         filename: '***.js',
         path: path.resolve(__dirname, 'dist')
     }
}; </code></pre>
#3.管理资源
##3.1加载 CSS
安装并添加 style-loader 和 css-loader
<pre><code>npm install --save-dev style-loader
npm install --save-dev css-loader</code></pre>
**webpack.config.js配置在 module.rules 增加配置 **
<pre><code>//webpack.config.js
{
    test: /\.css$/,
    use: [
         'style-loader',
          'css-loader'
       ]
}</code></pre>
import './style.css'，含有 CSS 字符串的 <style> 标签，将被插入到 html 文件的 <head> 中
##3.2加载图片
安装并添加 file-loader
<pre><code>npm install --save-dev file-loader</code></pre>
**webpack.config.js配置在 module.rules 增加配置 **
<pre><code>//webpack.config.js
{
    test: /\.(png|svg|jpg|gif)$/,
    use: [
        'file-loader'
    ]
}</code></pre>
import MyImage from './my-image.png'，该图像将被处理并添加到 output 目录，并且 MyImage 变量将包含该图像在处理后的最终 url
##3.3加载字体
**webpack.config.js配置在 module.rules 增加配置 **
<pre><code>//webpack.config.js
{
    test: /\.(woff|woff2|eot|ttf|otf)$/,
    use: [
        'file-loader'
    ]
}</code></pre>
##3.4加载数据
安装并添加 csv-loader xml-loader
<pre><code>npm install --save-dev csv-loader
npm install --save-dev xml-loader</code></pre>
**webpack.config.js配置在 module.rules 增加配置**
<pre><code>//webpack.config.js
{
    test: /\.(csv|tsv)$/,
    use: [
        'csv-loader'
    ]
},{
    test: /\.xml$/,
    use: [
        'xml-loader'
    ]
}</code></pre>
import 四种类型的数据(JSON, CSV, TSV, XML)中的任何一种，所导入的 Data 变量将包含可直接使用的已解析 JSON
##修改后的配置文件如下
<pre><code>//webpack.config.js
const path = require('path');
module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    },
    module: {
        rules: [{
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },{
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            },{
                test: /\.(woff|woff2|eot|ttf|otf)$/,
                use: [
                    'file-loader'
                ]
            },{
                test: /\.(csv|tsv)$/,
                use: [
                    'csv-loader'
                ]
            },{
                test: /\.xml$/,
                use: [
                    'xml-loader'
                ]
            }
        ]
    }
};</code></pre>
#4输出管理
##4.1如何向 HTML 动态添加 bundle
安装HtmlWebpackPlugin插件
<pre><code>npm install --save-dev html-webpack-plugin</code></pre>
**webpack.config.js引入插件**
<pre><code>const HtmlWebpackPlugin = require('html-webpack-plugin');</code></pre>
**webpack.config.js配置在 module.plugins 增加配置**
<pre><code>new HtmlWebpackPlugin({
   title: 'Output Management'
})</code></pre>
##4.2清理 /dist 文件夹
安装clean-webpack-plugin插件
<pre><code>npm install clean-webpack-plugin --save-dev</code></pre>
**webpack.config.js引入插件**
<pre><code>const CleanWebpackPlugin = require('clean-webpack-plugin');</code></pre>
**webpack.config.js配置在 module.plugins 增加配置**
<pre><code>new CleanWebpackPlugin(['dist']),</code></pre>
##最终修改后的文件为
<pre><code>//webpack.config.js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
module.exports = {
    entry: {
        app: './src/index.js'
    },
    plugins: [
        new CleanWebpackPlugin(['dist']),
        new HtmlWebpackPlugin({
            //title: 'Output Management'
        })
    ],
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'dist')
    }
};</code></pre>
#4配置开发环境
##使用 source map
**webpack.config.js 增加module.devtool配置**
<pre><code>devtool: 'inline-source-map'</code></pre>
##选择开发工具
+ webpack's Watch Mode
+ webpack-dev-server
+ webpack-dev-middleware 
###观察模式
命令行工具输入
<pre><code>webpack --watch</code></pre>
保存文件后 webpack 自动重新编译修改后的模块，需要手动刷新浏览器
###webpack-dev-server
**webpack.config.js 增加配置**
<pre><code>devServer: {
        contentBase: './dist'
}</code></pre>
以上配置告知 webpack-dev-server，在 localhost:8080 下建立服务，将 dist 目录下的文件，作为可访问文件。
命令行工具输入可以直接运行开发服务器(dev server)
<pre>webpack-dev-server --open</pre>
修改和保存任意源文件，web 服务器就会自动重新加载编译后的代码
###webpack-dev-middleware 
webpack-dev-middleware是将webpack处理的文件发送到服务器的包装器。在webpack-dev-server中这使用了该模块，但是它可以作为一个单独的包使用，以允许更多的自定义设置。 如将webpack-dev-middleware与express server相结合。
安装express 和 webpack-dev-middleware插件
<pre><code>npm install --save-dev express 
npm install --save-dev webpack-dev-middleware</code></pre>
webpack.config.js的output中增加“publicPath”，以确保服务器能正确的配置web服务器根目录
<pre><code>publicPath: '/'</code></pre>
新建server.js配置express server
<pre><code>//server.js
const express = require('express');
const webpack = require('webpack');
const webpackDevMiddleware = require('webpack-dev-middleware');
const app = express();
const config = require('./webpack.config.js');
const compiler = webpack(config);
// 告诉 express 使用 webpack-dev-middleware 并使用 webpack.config.js的publicPath配置
app.use(webpackDevMiddleware(compiler, {
       publicPath: config.output.publicPath
}));
// 配置服务器的端口是 3000.
app.listen(3000, function () {
        console.log('Example app listening on port 3000!\n');
});</code></pre>
启动服务器
<pre><code>node server.js</code></pre>
#5模块热替换
模块热替换允许在运行时更新各种模块，而无需进行完全刷新
安装 webpack-hot-middleware 插件
<pre><code>npm install --save-dev webpack-hot-middleware </code></pre>
**webpack.config.js 修改配置devServer**
<pre><code>devServer: {
        contentBase: './dist',
        hot: true
}</code></pre>
或者通过命令来修改 webpack-dev-server 的配置
<pre><code>webpack-dev-server --hotOnly</code></pre>
相关修改汇总如下：
<pre><code>//webpack.config.js 
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');
module.exports = {
        entry: {
            app: './src/index.js'
        },
        devtool: 'inline-source-map',
        devServer: {
            contentBase: './dist',
            hot: true
        },
        plugins: [
            new CleanWebpackPlugin(['dist']),
            new HtmlWebpackPlugin({
                title: 'Hot Module Replacement'
            }),
            new webpack.HotModuleReplacementPlugin()
        ],
        output: {
            filename: '[name].bundle.js',
            path: path.resolve(__dirname, 'dist')
        }
};</code></pre>
* dev-server.js
<pre><code>//dev-server.js
const webpackDevServer = require('webpack-dev-server');
const webpack = require('webpack');
const config = require('./webpack.config.js');
const options = {
        contentBase: './dist',
        hot: true,
        host: 'localhost'
};
webpackDevServer.addDevServerEntrypoints(config, options);
const compiler = webpack(config);
const server = new webpackDevServer(compiler, options);
server.listen(5000, 'localhost', () => {
       console.log('dev server listening on port 5000');
});</code></pre>
* 当使用 webpack dev server 和 Node.js API 时，不要将 dev server 选项放在 webpack 配置对象(webpack config object)中。而是，在创建选项时，将其作为第二个参数传递。例如：`new WebpackDevServer(compiler, options)`
#6移除未引用代码（Tree Shaking）
使用 tree shaking，你必须……
+ 1.使用 ES2015 模块语法（即 import 和 export）。
+ 2.引入一个能够删除未引用代码(dead code)的压缩工具(minifier)（例如 UglifyJSPlugin）。
安装 UglifyJSPlugin
<pre><code>npm install --save-dev uglifyjs-webpack-plugin</code></pre>
修改 webpack.config.js 配置
<pre><code>//webpack.config.js 
const path = require('path');
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
module.exports = {
        entry: './src/index.js',
        output: {
            filename: 'bundle.js',
            path: path.resolve(__dirname, 'dist')
        },
        plugins: [
            new UglifyJSPlugin()
        ]
};</code></pre>
#生产环境构建
开发环境(development)需要具有强大的、具有实时重新加载(live reloading)或热模块替换(hot module replacement)能力的 source map 和 localhost server
生产环境(production)期望更小的 bundle，更轻量的 source map，以及更优化的资源，以改善加载时间
开发环境和生成环境公共配置
<pre><code>//webpack.common.js
const path = require('path');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    entry: {
        app: './src/index.js'
    },
    plugins: [
        new CleanWebpackPlugin(['dist']),
        new HtmlWebpackPlugin({
            title: 'Production'
        })
    ],
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'dist')
    }
};</code></pre>
安装 webpack-merge
<pre><code>npm install --save-dev webpack-merge</code></pre>
开发环境配置
<pre><code>//webpack.dev.js
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
module.exports = merge(common, {
        devtool: 'inline-source-map',
        devServer: {
            contentBase: './dist'
        }
});</code></pre>
运行开发环境
<pre><code>webpack-dev-server --open --config webpack.dev.js</code></pre>
生产环境配置
<pre><code>//webpack.prod.js
const merge = require('webpack-merge');
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
const common = require('./webpack.common.js');
module.exports = merge(common, {
        plugins: [
            new UglifyJSPlugin()
        ]
});</code></pre>
运行生产环境
<pre><code>webpack --config webpack.prod.js</code></pre>
###source map
在生产环境中启用 source map：它们对调试源码(debug)和运行基准测试(benchmark tests)很有帮助
在生产环境中使用 source-map 选项，而不是我们在开发环境中用到的 inline-source-map：
<pre><code>//webpack.prod.js
const merge = require('webpack-merge');
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
const common = require('./webpack.common.js');
module.exports = merge(common, {
    devtool: 'source-map',
    plugins: [
        new UglifyJSPlugin({
            sourceMap: true
        })
    ]
})</code></pre>

