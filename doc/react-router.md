`React-router`是`React`常用的路由组件

在使用时我们辉显式的安装了`react-router-dom`这个库，实际上`react-router-dom`还依赖`react-router`库，在早期的版本中，是需要显式安装这两个库的，后来库开发者将`react-router`添加到`react-router-dom`的依赖中，所以现在不需要显式按爪给你`react-router`。

`react-router`提供了`withRouter`这个组件，可以帮我们将路由相关的属性和方法通过props传入组件，这样我们就可以在组件中获取一些url上的参数，也可以通过js去实现导航的功能

`react-router-dom`我们常用到的组件有：`BrowserRouter`、`HashRouter`、`Switch`、`Route`、`Link` 、`Redirect` 、`NavLink`，通过这些组件，我们可以使用申明式的方式来实现路由功能

`React-router`并没有像`angular`、`vue`一样实现路由守护功能，对于这样的功能，也没有支持配置式声明路由的功能，所以如果有需要，需要我们自己的实现这样的功能，但这个并不是太难，网上也有相关的实现方案。

在React router中有三种类型的组件，一是路由组件第二种是路径匹配组件第三种是导航组件。

+ 路由组件: `BrowserRouter` 和 `HashRouter`
+ 路径匹配的组件: `Route` 和 `Switch`
+ 导航组件: `Link` `Redirect` `NavLink`

## 安装
<pre><code>npm install react-router-dom</code></pre>

## 使用示例
### `BrowserRouter` 和 `HashRouter`
`BrowserRouter` 和 `HashRouter`是一个容器，所有的需要通过路由管理的组件，都需要在放在这容器里面，这两个组件有一些不同，前者是通过html5的history.pushState来修改应用的状态，这个可以直接修改网页的path而不触发页面的重新加载，但网页path是不能和后端的资源组件直接对应的，刷新页面时，由于是前端创建的资源路径，后端无法匹配到对应资源，会返回404，针对这种情况，解决的办法时修改后端配置，当所有路径都无法匹配时，返回默认的`index.html`，将路由控制权交给前端，前端通过路由配置正确的显示对应的组件即可。后者是通过修改URL后面的fragment，来实现路由切换，这个不存在兼容性问题，也不需要后端做任何修改，但整个网页的URL看起来会比较怪。这两个组件只需根据个人喜好选择其一即可，实际上对我们的开发不会有任何影响。

`BrowserRouter` 和 `HashRouter`和`react-redux`搭配使用时，应该放在'react-redux'的Provider内部，而不是相反。

### `Switch` 和 `Route`
`Route`组件可以指定当路由匹配时跳转的路径，已经相关参数，但所有的`Route`都需要放到`Switch`里面。`Switch` 和 `Route`是可以嵌套的，通过嵌套，我们可以实现多级路由。

需要注意的是这两个组件一般都是搭配使用，而且一定要放在`BrowserRouter` 或 `HashRouter`内部，但不限制深度。

以下是一个声明式路由的配置例子：
<pre><code>&lt;Switch&gt;
    &lt;Route path="/home"&gt;
        &lt;Home/&gt;
    &lt;/Route&gt;
    &lt;Route path="/add"&gt;
        &lt;EditTodo/&gt;
    &lt;/Route&gt;
    &lt;Route path="/edit"&gt;
        &lt;EditTodo/&gt;
    &lt;/Route&gt;
    &lt;Route path="/history"&gt;
        &lt;History/&gt;
    &lt;/Route&gt;
    &lt;Route path="/"&gt;
        &lt;NotFound/&gt;
    &lt;/Route&gt;
&lt;/Switch&gt;
</code></pre>

上面的例子种我们可以在`<History/>`组件中再次使用`Switch` 和 `Route`，从而实现了多级路由

### `Link` `Redirect` `NavLink`
`Redirect`用于重定向，比如在某个组件中，如果不满足一定条件（比如必须登录），可以重定向到另一个路径下（比如登录页面），如果满足条件，我们可以显示相关的组件。

比如在下面这个例子中，如果在需要登录且未登录的情况下，自动重定向到`/login`下，其他情况下直接显示`<UserComponent/>`：
<pre><code>const CustomComponent = props => {
    if (props.needLogin && props.isLogin) return &lt;Redirect to="/login"/&gt;
    return &lt;UserComponent/&gt;
}
</code></pre>

### `withRouter`
`withRouter`是用来像组件中注入`location`、`history`、`match`三个对象，这三个对象挂载在组件的props对象上，使用的时候可通过`props.location`、`props.history`、`props.match`来使用这三个对象。

具体来说，`location`上面包含了URL相关的内容，不包含方法，`history`上面提供了路由前进、后退、新增和替换的方法，`match`包含了路由匹配的情况，是否精确匹配，以及一些路径参数等。

`withRouter`和`react-redux`的`connect`一起使用时，应该将`withRouter`放在外面，`connect`放在里面，如下：
<pre><code>withRouter(connect(mapStateToProps, mapDispatchToProps)(CustomComponent))</code></pre>

### 路由传参
#### 通配符传参
Route定义方式：
<pre><code>&lt;Route path='/path/:name' component={Path}/&gt;</code></pre>
Link组件：
<pre><code>&lt;Link to="/path/通过通配符传参"&gt;通配符&lt;/Link&gt;</code></pre>
参数获取：
<pre><code>// 这里的this.props.match.params.name === ‘通过通配符传参’。
this.props.match.params.name</code></pre>
优点:简单快捷,并且，在刷新页面的时候，参数不会丢失。

缺点:只能传字符串，并且，如果传的值太多的话，url会变得长而丑陋。

如果需要传对象的话，可以用JSON.stringify(),想将其转为字符串，然后另外的页面接收后，用JSON.parse()转回去。

#### query
Route定义方式：
<pre><code>&lt;Route path='/query' component={Query}/&gt;</code></pre>
Link组件：
<pre><code>var query = {
    pathname: '/query',
    query: '我是通过query传值 '
}
&lt;Link to={query}&gt;通配符&lt;/query&gt;</code></pre>
参数获取：
<pre><code>// 这里的this.props.location.query === '我是通过query传值'
this.props.location.query</code></pre>

优点：优雅，可传对象

缺点：刷新页面，参数丢失

#### state
Route定义方式：
<pre><code>var state = {
    pathname: '/state',
    state: '我是通过state传值'
}
&lt;Route to={state}&gt;state&lt;/Link&gt;</code></pre>
Link组件：
<pre><code>&lt;Link path='/state' component={State}/&gt;</code></pre>
参数获取：
<pre><code>// 这里的this.props.location.state === '我是通过state传值'
this.props.location.state</code></pre>

优点：优雅，可传对象

缺点：刷新页面，参数丢失

### 参考链接
[react router官方文档](https://reacttraining.com/react-router/web/guides/quick-start)

[react router路由传参](https://www.cnblogs.com/yky-iris/p/9161907.html)