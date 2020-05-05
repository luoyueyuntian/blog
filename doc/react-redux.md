`react-redux`是`react`常用的状态管理工具。使用过程中，要区分`redux`和 `react-redux`，二者的功能是不一样的，`redux` 是 JavaScript 状态容器，提供可预测化的状态管理。`redux` 除了可以与`react`搭配外，还可以与其他框架一起使用。在用的过程中，我们所用到的store、reduce、action、dispatch都是 `redux` 提供的内容，而 `react-redux` 中最主要的就是`Provider`和`connect`。
 `Provider`主要用来存储全局store，而`connect`是通过react的组件context特性，将store中的数据和方法以properties属性的方式注入组件。

 #### 单向数据流
 以下是flux 的单向数据流图
 <pre><code>
                 _________                ____________                ___________
                |         |              |            |              |           |
                | Action  |------------▶| Dispatcher  |------------▶| callbacks |
                |_________|              |____________|              |___________|
                     ▲                                                    |
                     |                                                    |
                     |                                                    |
 _________        ___|_____                                           ____▼____
|         |◀----|  Action  |                                        |         |
| Web API |      | Creators |                                        |  Store  |
|_________|----▶|__________|                                        |_________|
                     ▲                                                    |
                     |                                                    |
                 ____|_________           ___________                 ____▼____
                |   User       |         |   React   |               | Change  |
                | interactions |◀--------|   Views   |◀-------------| events  |
                |______________|         |___________|               |_________|
 </code></pre>
store是全局的状态数据仓库，公共的状态数据存放在store中，store的数据在组件树中自上而下传递到所需的组件中，组件收到数据，渲染出相应的视图。用户与视图之间交互，产生action，然后通过dispatch通知store修改数据，store数据修改后，又通知view更新视图。

从上面的图可以看出，数据总是从右边的store到左边的view，而视图层修改数据的方式，是从右到左通过派发action，通知store修改数据，数据的修改始终发生在最右端的store中。

从这个图可以看出有几个关键的概念：store、action、dispatch

#### Action
Action是一个对象，描述了应该对数据做什么，其中包含type和其他属性，type定义了应该对哪个数据做什么样的处理，而其他属性是可选的，而且可以是任意数据。可以看以下例子：
<pre><code>{
    type: 'add-todo',
    name: 'learn react-redux'
}</code></pre>
这个action的type定义为'add-todo'（可以为其他任意字符串），表示的意思是要新增一个'todo'，而name属性表示的意思是这个'todo'的名字是'learn react-redux'(可以根据需要使用其他任意字符串)，type是固定每个action都会有的属性，而name是我自定义的数据，这个属性名可以是其他字符串，只要符合js对属性名的限制即可，而name的值除了可以是字符串，还可以根据需要是其他任何值，比如：数字、布尔值、对象等等，属性名和属性值都是可以根据需要自行设置的。另外，如果有需要，还可以增加其他属性，和name属性一样，没有任何限制。

action的type除了在action中有到外，在reduce里也会用到，一般都会把action定义成常量，在action和reduce中共享，好处就是当需要对actionType修改时，两边都会同步修改，而不需要修改两遍，也不会出现修改了一处，而另一处忘记修改的情况。
<pre><code>// action-type.js
const ADD_TODO = 'ADD_TODO'
export default {
    ADD_TODO
}</code></pre>
每次需要修改数据时，我们就需要创建一个新的action，比如我们新增了两个todo，那么我们就需要生成两个action，每个action里面都涉及到了`ADD_TODO`，为了方便，我们可以加一个函数，返回这样一个action。具体模式为：
<pre><code>const actionCreater = (...prop) => {
    type: someType,
    ...prop
}</code></pre>
对于上面的新增todo的action，可以按以下方式来：
<pre><code>// actions
import { ADD_TODO } from './action-type.js'
const addTodo = (names) => {
    type: ADD_TODO,
    name: name,
    id: generateNewTodoId()
}
export default {
    addTodo
}</code></pre>

#### Reducer
reducer 就是一个纯函数，接收旧的 state 和 action，返回新的 state。
<pre><code>(previousState, action) => newState</code></pre>
reducer就是用来更新数据。因为react非常依赖数据的不可变性，所以reducer修改完数据后，应该返回一个新的数据，而不是直接在原有的数据上修改。举例来说，如果有一个数组，用来存放一些数据，当我们新增一个数据时，不应该直接push到数组中，而是将用一个新数组，把原有的数据拷贝进去，最后再把新增的数据push进去，最后返回这个新的数组。

以`addTodoReduce`为例，向store.todos里面添加一个新的todo，代码如下：
<pre><code>import { ADD_TODO } from './action-type.js'
/**
 * @param { object } previousState store.todos
 * @param { object } action {  type: string , ...}
 * return { object } newTodos
 */
const todosReducer = (previousState, action) => {
    if (action.type === ADD_TODO) {
        return [...previousState, { name: action.name, id: action.id }]
    } else {
        return previousState
    }
}</code></pre>
返回的结果是一个新的数组。

如果正对todo有更多的action，则应该这样：
<pre><code>import { ADD_TODO, DELETE_TODO, UNPATE_TODO } from './action-type.js'
/**
 * @param { object } previousState store.todos
 * @param { object } action {  type: string , .../*其他自定义属性*/}
 * return { object } newTodos
 */
const todosReducer = (previousState, action) => {
    switch (action.type) {
        case ADD_TODO:
            return [...previousState, { name: action.name, id: action.id }]
        case DELETE_TODO:
            return previousState.filter(item => item.id !== action.id)
        case UNPATE_TODO:
            return previousState.map(item => item.id === action.id ? { ...item, id: action.id } : item)
        default:
            return previousState
    }
}</code></pre>

以上是store.todos的reducer，store下面会有多个属性，所以其他每个属性都会有一个reducer，比如可以有historyReducer、visibilityFilterReducer等等，那么最终就会合成一个能够处理整个store的reducers。redux的combineReducers 就是做这个工作的。
<pre><code>import { combineReducers } from 'redux'
export default {
    todos: todosReducer,
    history: historyReducer,
    visibilityFilter: visibilityFilterReducer
}</code></pre>
最终store只有一个reducer，他负责管理整个store中数据的更新。

#### Store
action描述应该怎么该做数据，reducer负责更新数据，而store则负责把所有的联系在一起，store具有以下职能：
+ 维持应用的 state；
+ 提供 `getState()` 方法获取 state；
+ 提供 `dispatch(action)` 方法更新 state；
+ 通过 `subscribe(listener)` 注册监听器;
+ 通过 `subscribe(listener)` 返回的函数注销监听器。

可以看出，store持有了state，如果需要修改store，就需要派发action，派发action后，store内部会运行reducer，得到一个新的state，然后通知订阅者执行回调。当然在与react结合时，我们不会直接对store进行操作，而是交给`react-redux`处理。`react-redux`提供了两个模块：

#### Provider
Provider是一个react容器组件，其接收一个属性：store，被Provider包裹的组件，可以通过connect去包装组件，可以将store中的state注入到组件中。Provider一般应该在最外层，这样所有的组件都可以通过connect访问store中的state。

#### connect
connect方法可以将store中的state和actionCreater注入到组件中，connect接收两个方法，一个是mapStateToProps，一个是mapDispatchToProps。mapStateToProps可以将state注入到组件中，而mapDispatchToProps可以将actionCreater注入到组件中，使组件可以dispatch action来修改store中的state。

<pre><code>// 返回从store中提取的state,返回值中的属性key可以自定义，不需要与store中的对象key一致
const mapStateToProps = state => ({ todos: state.todos })
// 第二个参数可选的，指的是传递给react组件的props
const mapStateToProps = (state, ownProps) => ({
  todo: state.todos[ownProps.id]
})</code></pre>

<pre><code>const mapDispatchToProps = dispatch => {
  return {
    // dispatching plain actions
    increment: () => dispatch({ type: 'INCREMENT' }),
    decrement: () => dispatch({ type: 'DECREMENT' }),
    reset: () => dispatch({ type: 'RESET' })
  }
}</code></pre>

#### 参考资料
+ [Redux](https://redux.js.org/introduction/getting-started)
+ [react-redux](https://react-redux.js.org/introduction/quick-start)