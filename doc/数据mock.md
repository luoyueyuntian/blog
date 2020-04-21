使用`mockjs`来模拟接口数据返回

`mockjs`是一个开源的数据模拟工具，它通过拦截请求，将已经设置为mock的接口直接拦截，并根据规则生成数据，而不会真正的将请求发送到后端

#### 安装`mockjs`
<pre><code>npm install mockjs</code></pre>

#### 使用`mockjs`
以下面这样一个接口为例：
请求URL为`/rest/todo/get-todo`，返回结果为格式如下的数组，数组长度不定
<pre><code>[{
    id: Number,
    name: String,
    desc: String,
    unit: '小时'|'天'|'周'|'月'|'年',
    count: Number,
    status: 0|1|2|3|4
}]</code></pre>
使用`mockjs`时可通过如下代码，来实现一个符合上面要求的接口mock
<pre><code>import Mock from 'mockjs'
// 设置接口响应时间为40-250ms之间的任意一个时间长度，可不配置
Mock.setup({
    timeout: '40-250'
})
// 以下代码实现了如上的要求
Mock.mock('/rest/todo/get-todo', 'get', () => {
    const length = Mock.Random.natural(20, 60)
    const result = []
    let startId = Mock.Random.natural(10,30)
    for (let i = 0; i < length; i++) {
        result.push({
            id: startId++,  // 递增的数据
            name: Mock.Random.csentence(5), // 长度为5的句子
            desc: Mock.Random.csentence(22),    // 长度为22的句子
            unit: ['小时','天','周','月','年'][Mock.Random.natural(0,4)],   // 指定数据中的一项
            count: Mock.Random.natural(1,15),   // 1-15之间的一个数字
            status: Mock.Random.natural(0,4)    // 0-4之间的一个数字
        })
    }
    return result
})</code></pre>

使用该功能需要在代码中引入以上部分，ajax不要使用`fetch`,使用中发现`fetch`方法不会被`mockjs`重写，使用`axios`则没有问题，而且以上内容的引入应该要晚于`axios`的引入，这样后引入的才会覆盖先引入的。

上面是一个稍微复杂的接口mock，如果简单的数据格式，可以不需要写代码逻辑，只需按简单的规则配置即可。


### 参考资料
[mockjs文档](https://github.com/nuysoft/Mock/wiki/Getting-Started)

[mockjs示例](http://mockjs.com/examples.html)

[前端模拟接口数据（mock）实践](https://segmentfault.com/a/1190000011766238?utm_source=tag-newest)

[CYBMOCK](https://cybmock.hestudy.com/docs/config/)

[[技术探索]前端实现后端数据模拟的几种方法](https://www.jianshu.com/p/8d66deef50c7)

[实用！前后端分离开发之前端模拟数据](https://cloud.tencent.com/developer/article/1493249)
