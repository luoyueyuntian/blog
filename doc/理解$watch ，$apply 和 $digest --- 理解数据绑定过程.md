### $watch 队列（$watch list）
每次你绑定一些东西到你的UI上时你就会往$watch队列里插入一条$watch。<br/>
当我们的模版加载完毕时，也就是在linking阶段（Angular分为compile阶段和linking阶段---译者注），Angular解释器会寻找每个directive，然后生成每个需要的$watch。

### $digest循环
当浏览器接收到可以被angular context处理的事件时，$digest循环就会触发。这个循环是由两个更小的循环组合起来的。一个处理evalAsync队列，另一个处理$watch队列。 这个是处理什么的呢？$digest将会遍历我们的$watch，检测有没有$watch更新过。如果有至少一个更新过，这个循环就会再次触发，直到所有的$watch都没有变化。这样就能够保证每个model都已经不会再变化。记住如果循环超过10次的话，它将会抛出一个异常，防止无限循环。 当$digest循环结束时，DOM相应地变化。

### $apply(exp)
$apply方法就是将$digest方法包装了一层，exp是可选参数，可以是一个string，也可以是function（scope）。
$apply方法使得我们可以在angular里面执行angular框架之外的表达式。
$apply被调用后最终都会触发$digest()，触发$digest循环。<br/>
* 1、$apply 方法作用：<br/>Scope提供$apply方法传播Model的变化
* 2、$apply方法使用情景：<br/>AngularJS 外部的控制器（DOM 事件、外部的回调函数如 jQuery UI 空间等）调用了 AngularJS 函数之后，必须调用$apply。在这种情况下，你需要命令 AngularJS 刷新自已（模型、视图等） ，$apply 就是用来做这件事情的。
* 3.$apply的使用注意事项:<br/>
在一个具有$apply的环境中使用apply，会抛出异常来的。angularjs一些自带的指令和服务已经封装进了apply（比如ng-click、ng-model、 $timeout 等 ）。<br/>
所以如果说我们的代码不在$apply环境中，结果是异步返回的，我们就需要手动触发$apply

Apply接受一个函数作为参数，函数中绑定的对象会被脏检查，function不能是异步的！当然，当apply函数的参数为空的是时候，它会把当前作用域中所有的脏对象都检查一遍。浪费性能！<br/>
只要可以，请把要执行的代码和函数传递给$apply去执行，而不要自已执行那些函数然后再调用$apply。因为$apply无法监控到外面的代码是否执行成功。
### $watch(watchExpression, listener, objectEquality)
$watch()方法监视Model 的变化。<br/>
参数的说明：<br/>
+ watchExpression：监听的对象，它可以是一个angular表达式如'name'（要监视的变量的名字）,或函数如function(){return $scope.name}。
+ listener:当watchExpression变化时会被调用的函数或者表达式,它接收3个参数：newValue(新值), oldValue(旧值), scope(作用域的引用)
+ objectEquality：是否深度监听，如果设置为true,它告诉Angular检查所监控的对象中每一个属性的变化. 如果你希望监控数组的个别元素或者对象的属性而不是一个普通的值, 那么你应该使用它。

注意：第三个参数为true时会牵涉到性能问题。全等监控运行起来的时候是先监控到整个对象，然后在每一次吧$diges跑起来之前先用angualr.copy()将整个对象先拷贝一遍之后再调用angular.equal（）方法来进行比较，所以这一监控可能会消耗大量的资源！在angularJS 1.1.4又出来了一个$watchCollection()方法，专门来监控数组集合的，他的性能介于引用监控和全等监控之间，它不会对数组的每一项内容 进行监控，而是当数组的pop和push时候做出反应。<br/>
监听的对象在每次修改后调用$digest方法重新算值，如果返回值发生了改变，listener就会执行。在判断newValue和oldValue是否相等时，会递归的调用angular.equals方法。在保存值以备后用的时候调用的是angular.copy方法。listener在执行的时候，可能会修改数据从而触发其他的listener或者自己直到没有检测到改变为止。Rerun Iteration的上限是 10 次，这样能够保证不会出现死循环的情况。 



### $digest()
该方法会触发当前scope以及child scope中的所有watchers，因为watcher的listener可能会改变model，所以$digest方法会一直触发watchers直到不再有listener被触发。当然这也有可能会导致死循环，不过angular也帮我们设置了上限 10 ！否则会抛出“ Maximum iteration limit exceeded. ”。<br/>
通常，我们不在controller或者directive中直接调用$digest方法，而是调$apply方法，让$apply方法去调用$digest方法。 

### angular.equals方法
AngularJS中的angular.equals()方法用于比较两个对象、值或表达式是否相等。AngularJs文档中对equals方法比较的原则是这样描述的：
+ （1）比较的两个对象或值能够通过 === 表达式。===要求两个值不仅值相同，类型也要相同，也就是说，1 === “1”是不成立的。
+ （2）比较的两个对象或值是相同类型的，而且它们所有的属性通过angular.equals()方法判断都是相等的。
+ （3）两个值都为（NaN）。（在JavaScript中认为NaN == NaN是false）
+ （4）两个值代表字面上相等的表达式，如两个正则表达式：/abc/与/abc/是相等的。

