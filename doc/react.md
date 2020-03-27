### react
#### react定义组件的三种方式
1.函数式无状态组件
<pre><code>function CustomComponent(props){
    return 	&lt;div&gt; Hello {props.name}	&lt;/div&gt;
}
</code></pre>
2.es5方式React.createClass组件
<pre><code>var CustomComponent = React.createClass({
    propTypes: {//定义传入props中的属性各种类型        
        initialValue: React.PropTypes.string
    },
    defaultProps: { //组件默认的props对象       
         initialValue: " "
     },
    // 设置 initial state    
    getInitialState: function(){//组件相关的状态对象        
        return {
            text: this.props.initialValue || ''      
         };
    },
    handleChange: function(event){
        this.setState({ //this represents react component instance       
             text: event.target.value
        });
    },
    render: function() (
           &lt;div&gt;
                Type something:
                &lt;input onChange={this.handleChange} value={this.state.text} /&gt;
            &lt;/div&gt;
        );
    }
});
CustomComponent.propTypes = {
    initialValue: React.PropTypes.string
};
CustomComponent.defaultProps = {
    initialValue: ''
};
</code></pre>

3.es6方式extends React.Component
<pre><code>class CustomComponent extends React.Component{
    constructor(props) {
        super(props);
        // 设置 initial state       
        this.state = {
            text: props.initialValue || ''        
        };
        // ES6 类中函数必须手动绑定        
        this.handleChange = this.handleChange.bind(this);
    }
    handleChange(event) {
        this.setState({
            text: event.target.value
        });
    }
    render() {
        return (
           &lt;div&gt;
                Type something:
                &lt;input onChange={this.handleChange} value={this.state.text} /&gt;
            &lt;/div&gt;
        );
    }
}
CustomComponent.propTypes = {
    initialValue: React.PropTypes.string
};
CustomComponent.defaultProps = {
    initialValue: " "
};
</code></pre>

### state、prop 状态提升
state指的是能够影响视图更新的状态，如果一个状态和试图的更新无关，不应该放在state中，可以放在state外面。


组件自身维护的状态为state，由父组件传递过来的状态为prop，组件修改自身状态使用setState，组件不应该直接修改prop，而应该让父组件去修改prop，组件不维护prop的更新。

当兄弟组件需要共享一个状态时，应该将状态提升到共同的父组件，然后通过prop传递给子组件，当一个子组件通过通知父组件修改状态后，另一个组件可以接收到更新，并同步视图更新。

### setState
组件修改自身状态时，使用setState，state的值不能直接修改的，一个是react限制了对state的修改，另外一个是state是与视图相关的，当state变化后，视图需要变化，直接修改state，react不会收到通知需要修改视图。

setState在修改了state后，会异步去更新视图。setState对state的修改也不是同步的，如果你多次修改state，react会合并对state的修改，如果你的state更新依赖上一次更新后的结果，那么需要注意你拿到的state可能是还没有更新的state，对于这种情况，setState可以接收一个函数，函数的参数是更新后的state和prop。

react执行完合并后的state更新后，会按顺序执行以下钩子函数
+ static getDerivedStateFromProps()
+ shouldComponentUpdate()
+ render()
+ getSnapshotBeforeUpdate()
+ componentDidUpdate()

其中，可以在shouldComponentUpdate中阻止视图的更新，这个通常是一个可以用来性能优化的地方。阻止不必要的diff过程。

因为state的值的更新不是在调用setState后立即更新的，所以在读取state的值时可能遇到读取的值是为更新的state，为了获取更新后的值，可以在componentDidUpdate中读取，setState也支持另外一个可选参数，这个参数是state更新后的回调函数，也可以保证拿到更新后的state。

### 组件的生命周期
先放一张官方给的组件生命周期流程图
![react组件生命周期](../assert/img/react_hooks.PNG)
组件一般会有三个不同的阶段，第一个是组件的创建阶段，在这个阶段，组件分别执行以下钩子函数：
+ constructor() 初始化state和绑定事件的this
+ static getDerivedStateFromProps()
+ render()
+ componentDidMount()   组件挂载后调用，此时可执行依赖DOM节点的操作，请求数据等


组件的更新
+ static getDerivedStateFromProps()
+ shouldComponentUpdate()   props 或 state 发生变化后，渲染执行之前调用
+ render()
+ getSnapshotBeforeUpdate() DOM更新前调用
+ componentDidUpdate()  组件更新后调用

组件的销毁
+ componentWillUnmount()    组件卸载及销毁之前调用
