在调用$apply.apply手动触发digest（脏检查）时有可能遇到报错，说digest（脏检查）正在执行（Error: $digest already in progress），可通过如下函数来代替$apply下apply函数
<pre><code>function safeApply(scope, fn) {
	(scope.phase || scope.$root.phase) ? fn(): scope.$apply(fn);
}</code></pre>
先判断是否正在做“脏检查”，如果是就直接执行函数，不用$apply；反之没有启动脏检查，那么就$apply执行该函数。

另外可以通过$timeout来实现$apply
