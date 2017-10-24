### jqLite只提供了以下jQuery方法：
+ addClass()
+ after()
+ append()
+ attr()
+ bind() - 不支持namespaces、selectors或eventData
+ children() - 不支持selectors
+ clone()
+ contents()
+ css()
+ data()
+ empty()
+ eq()
+ find() - 只限于根据标签名查找
+ hasClass()
+ html()
+ next() - 不支持selectors
+ on() - 不支持namespaces、selectors或eventData
+ off() - 不支持namespaces、selectors
+ one() - 不支持namespaces、selectors
+ parent() - 不支持selectors
+ prepend()
+ prop()
+ ready()
+ remove()
+ removeAttr()
+ removeClass()
+ removeData()
+ replaceWith()
+ text()
+ toggleClass()
+ triggerHandler() - 传递一个虚拟的事件对象到handlers
+ unbind() - 不支持namespaces
+ val()
+ wrap()

### Angular同时提供了以下额外的方法和事件到jQuery和jqLite:

+ 事件<br/>
$destroy - AngularJS拦截所有jqLite/jQuery的DOM销毁api，并在所有DOM 节点被移除时触发这个事件。这可以在它被移除前用于清除任何绑定到DOM元素的第三方内容。

+ 方法<br/>
controller(name) -获取当前元素或它父亲的控制器。默认获取的控制器与ngController指令相关。如果name 是驼峰式的指令名，这个指令的控制器会被获取到(如： 'ngModel')。

+ injector() - 获取当前元素或它父亲的注入。

+ scope() - 获取当前元素或它父亲的 scope 。

+ isolateScope() - 获取直接附加到当前元素的独立 scope。这个获取器只能用于包含开始一个新的独立域的指令的元素上。在这个元素上调用scope()永远返回原始的非独立域。

+ inheritedData() - 等同于 data(), 但是会遍历DOM直到找到一个值或到达最顶部父元素。 
