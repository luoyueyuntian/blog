

## Web 应用程序开发框架
#### express
Express 是一个保持最小规模的灵活的 Node.js Web 应用程序开发框架，为 Web 和移动应用程序提供一组强大的功能。
+ [Express —— 官方文档](http://www.expressjs.com.cn/)
+ [github地址](https://github.com/expressjs/express)


#### koa
Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。 Koa 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。
+ [Koa —— 官方文档](https://koa.bootcss.com/)

#### egg
Egg.js，为企业级框架和应用而生，是阿里开源的企业级 Node.js 框架
+ [egg —— 官方文档](https://eggjs.org/zh-cn/index.html)
+ [egg —— 源码](https://github.com/eggjs/egg)

#### ThinkJS
ThinkJS 是一款面向未来开发的 Node.js 框架，整合了大量的项目最佳实践，让企业级开发变得更简单、高效。从 3.0 开始，框架底层基于 Koa 2.x 实现，兼容 Koa 的所有功能。
+ [文档](https://thinkjs.org/)
+ [github地址](https://github.com/thinkjs/thinkjs)

#### Restify
restify是一个基于Nodejs的REST应用框架，支持服务器端和客户端。restify比起express更专注于REST服务，去掉了express中的template, render等功能，同时强化了REST协议使用，版本化支持，HTTP的异常处理。

restify提供了DTrace功能，为程序调式带来新的便利！
+ [restify —— 官方文档](http://restify.com/)

## 数据库相关
#### 数据库驱动 - Mongoose
Mongoose为模型提供了一种直接的，基于scheme结构去定义你的数据模型。它内置数据验证， 查询构建，业务逻辑钩子等，开箱即用。
+ [Mongoose —— 文档](http://www.mongoosejs.net/)


#### 数据库链接 - sequelize
Sequelize 是 Node 的一个 ORM(Object-Relational Mapping) 框架，用来方便数据库操作。
+ [Sequelize —— 文档](https://sequelize.org/)

#### 数据库链接 - mysql2
适用于Node.js的MySQL客户端，侧重于性能。 支持预备语句，非utf8编码，二进制日志协议，压缩，ssl等
+ [mysql2 —— 源码](https://github.com/mysqljs/mysql)

#### elasticsearch-js
适用于Node.js的官方Elasticsearch客户端库
+ [官方github](https://github.com/elastic/elasticsearch-js)

#### node_redis
node的redis客户端
+ [github地址](https://github.com/NodeRedis/node_redis)

## 请求连接
#### Web Socket - Socket.IO
Socket.IO支持实时，双向和基于事件的通信。
它适用于所有平台，浏览器或设备，并同时关注可靠性和速度。
+ [Socket.IO —— 文档](https://socket.io/)
+ [github地址](https://github.com/socketio/socket.io)

#### ws
最广泛的WebSocket模块，用来创建WebSocket的服务器
+ [github地址](https://github.com/websockets/ws)

#### Request
Request被设计为进行http调用的最简单方法。 它支持HTTPS，默认情况下遵循重定向。

#### axios
Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。
+ [axios —— 文档](https://ejs.bootcss.com/)

## 前端模板 
#### EJS
EJS 是一套简单的模板语言，帮你利用普通的 JavaScript 代码生成 HTML 页面。EJS 没有如何组织内容的教条；也没有再造一套迭代和控制流语法；有的只是普通的 JavaScript 代码而已。
+ [EJS —— 官方文档](http://restify.com/)
+ [EJS —— 源码](https://github.com/mde/ejs)


## 日志
#### morgan
morgan是Node.js中间件，用来记录日志，Express使用的就是该中间件记录日志。

#### Log4js
log4js是javascript的log框架

## 生成文档：
#### apidoc
apiDoc通过源代码中的API注释创建文档。
+ [apidoc —— 文档](https://apidocjs.com/)

#### slate
Slate可帮助您创建美观，智能，响应迅速的API文档。
+ [github地址](https://github.com/slatedocs/slate)


## 应用管理
+ nodemon
文件变动监控(自动重启)，(启动服务器脚本中替换 node 即可)

+ pm2
进程管理:

+ forever
部署

+ serve-favicon
带重启(nodemon用于开发环境)，日志，负载均衡

## 加密
+ md5加密
[md5加密](https://www.npmjs.com/package/md5)

+ aes 加解密
crypto-js

+ 创建uuid
[创建uuid](https://www.npmjs.com/package/uuid)

anywhere
+ 静态服务器
[anywhere](https://www.npmjs.com/package/anywhere)

+ node-cron

定时任务[node-cron](https://www.npmjs.com/package/cron)

## 工具
+ Node.js-FriendlyMai
一个简洁现代且易于使用的Nodejs邮件发送包

+ Sinon 模拟输入

+ faker 数据伪造:

+ mockjs 模拟接口数据

lodash 实用工具库，封装了诸多对字符串、数组、对象等常见数据类型的处理函数

+ moment 日期处理库

+ 单元测试 Mocha,Karma,Jasmine。

+ Async 异步流程控制

+ js-cookie 操作cookie [js-cookie](https://github.com/js-cookie/js-cookie)

+ express-validator
数据验证:

+ exceljs
操作Excel，[文档](https://github.com/exceljs/exceljs/blob/HEAD/README_zh.md)

+ chokidar
对nodejs的fs.watch / fs.watchFile / FSEvents 的包装
[源码](https://github.com/paulmillr/chokidar)

+ node-html-pdf
将HTML到成pdf， 内如使用phantomjs。
[源码](https://github.com/marcbachmann/node-html-pdf)

+ md5-password-cracker.js
使用JavaScript Web Workers破解MD5密码
[源码](https://github.com/feross/md5-password-cracker.js)

## Express中间件
+ body-parser

body-parser是一个HTTP请求体解析的中间件，使用这个模块可以解析JSON、Raw、文本、URL-encoded格式的请求体，

+ helmet

设置 HTTP 头(提高安全性)

+ cors

允许 cors 请求

+ response-time

记录服务响应时间，从请求进入该中间件到响应头写入客户端所经过的时间

+ compression

压缩请求

+ serve-favicon

serve-favicon是Node.js中间件，用于服务图标。

更多中间件可参看[Nodejs基础中间件Connect](https://www.cnblogs.com/chris-oil/p/5625437.html)
