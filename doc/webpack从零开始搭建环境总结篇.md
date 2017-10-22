---
date: 2017-10-21 22:54
status: public
title: webpack从零开始搭建环境总结篇
---

#创建package.json文件
+ 创建项目文件夹
+ 执行命令 `npm init`
+ 执行命令 `npm install --save-dev webpack`
+ 执行命令 `npm install --save-dev style-loader css-loader`
+ 执行命令 `npm install --save-dev file-loader`
+ 执行命令 `npm install --save-dev html-webpack-plugin`
+ 执行命令 `npm install --save-dev webpack-merge`
+ 执行命令 `npm install --save-dev webpack-hot-middleware `
+ 执行命令 `npm install --save-dev uglifyjs-webpack-plugin `
+ 执行命令 `npm install --save-dev babel-loader babel-core babel-preset-env webpack`

> style-loader css-loader 用于加载样式
file-loader 用于加载图片、字体
html-webpack-plugin 用于将编译完的文件插入到index.html中
webpack-hot-middleware 用于模块热替换
webpack-merge 在提取公共配置项后，在开发环境和生产环境中用于配置项的合并
uglifyjs-webpack-plugin 压缩文件，删除未使用到的代码
babel-loader 用于转译js文件 > 

+ 创建 src 文件夹（源文件目录）
+ 创建 bulid 文件夹（webpack配置文件目录）
+ 新建文件webpack.common.js，存放开发环境和生成环境公共的配置
<pre><code>//webpack.common.js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const webpack = require('webpack');
function resolve(dir) {
    return path.join(__dirname, '..', dir)
}
module.exports = {
    entry: './src/index.js',
    plugins: [
        new CleanWebpackPlugin(['dist']),
        new HtmlWebpackPlugin({
            inject: true,
            template: 'index.html',
        })
    ],
    output: {
        filename: '[name].js',
        path: path.resolve(__dirname, 'dist')
    },
    module: {
        rules: [{
            test: /\.js$/,
            loader: 'babel-loader',
            include: [resolve('src'), resolve('test')]
        }, {
            test: /\.css$/,
            use: [
                'style-loader',
                'css-loader'
            ]
        }, {
            test: /\.(png|svg|jpg|gif)$/,
            use: [
                'file-loader'
            ]
        }, {
            test: /\.(woff|woff2|eot|ttf|otf)$/,
            use: [
                'file-loader'
            ]
        }]
    }
};</code></pre>
+ 新建文件webpack.dev.js，存放开发环境配置
<pre><code>//webpack.dev.js
const merge = require('webpack-merge');
const webpack = require('webpack');
const commonWebpackConfig = require('./webpack.common');
module.exports = merge(commonWebpackConfig, {
    plugins: [
        new webpack.HotModuleReplacementPlugin()
    ],
    devtool: 'inline-source-map',
    devServer: {
        contentBase: './dist',
        hot: true,
        port: 3000
    },
    output: {
        publicPath: '/'
    }
});</code></pre>
+ 新建文件webpack.prod.js，存放生成环境配置
<pre><code>//webpack.prod.js
const merge = require('webpack-merge');
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
const webpack = require('webpack');
const commonWebpackConfig = require('./webpack.common');
module.exports = merge(commonWebpackConfig, {
    plugins: [
        new UglifyJSPlugin({
            sourceMap: true
        })
    ],
    devtool: 'source-map'
});</code></pre>
+ 根目录下新建index.html
+ src目录下新建index.js文件
**QAB**
+ 怎样配置HTML文件的路径？<br/>
使用 html-webpack-plugin 插件
新建插件时，传入的配置中，template 选项设成HTML的相对路径
<pre><code>new HtmlWebpackPlugin({
        inject: false,
        template: 'index.html',
})</code></pre>

[一个不错的总结文档--webpack多页应用架构系列]( https://segmentfault.com/a/1190000006863968)
也可以看下vue-cli生成的webpack项目的配置，稍微有些不同，如果搞清楚了webpack这些基本配置，基本能看懂