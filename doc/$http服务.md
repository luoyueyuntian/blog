$http服务是只能接受一个参数的函数，这个参数是一个对象，包含了用来生成HTTP请求的配置内容。这个函数返回一个promise对象，具有success和error两个方法。
这两个方法最基本的使用场景如下：
<pre><code>$http({
	method: 'GET',
	url: '/api/users.json'
}).success(function(data, status, headers, config) {
	// 当相应准备就绪时调用
}).error(function(data, status, headers, config) {
	// 当响应以错误状态返回时调用
});</code></pre>
$http方法返回一个promise对象，我们可以在响应返回时用then方法来处理回调。如果使用then方法，会得到一个特殊的参数，它代表了相应对象的成功或失败信息，还可以接受两个可选的函数作为参数。或者可以使用success和error回调代替。
<pre><code>var promise = $http({
	method: 'GET',
	url: '/api/users.json'
});
promise.then(function(resp) {
	// resp是一个响应对象
}, function(resp) {
	// 带有错误信息的resp
});

// 或者使用success/error方法
promise.success(function(data, status, headers, config) {
	// 处理成功的响应
});
promise.error(function(data, status, headers, config) {
	//处理非成功的响应
});</code></pre>
如果响应状态码在200和299之间，会认为响应是成功的，success回调会被调用，否则error回调会被调用。<br/>
注意，如果响应结果是重定向，XMLHttpRequest会跟进这个重定向，error回调并不会被调用<br/>
我们可以调用HttpPromise对象上的then()、success()和error()方法。then()方法与其他两种方法的主要区别是，它会接收到完整的响应对象，而success()和error()则会对响应对象进行析构。<br/>
调用http方法后，在下一个$digest循环运行之前它并不会被真正执行。尽管大部分情况下我们都是在$apply代码快内部使用$http，但也可以在AngularJS的$digest循环以外执行这个方法。<br/>
如果要在AngularJS的$digest循环以外执行$http函数，需要将其封装在一个$apply代码快中。这样会强制digest循环执行，我们的promise可以按预期那样被resolve。
<pre><code>$scope.$apply(function() {
	$http({
		method: 'GET',
		url: '/api/users.json'
	});
});</code></pre>
# 快捷方法
$http服务提供了一些顺手的快捷方法供我们使用，这些方法简化了复杂设置，只需要提供URL和HTTP方法（或者POST或PUT请求中包含的数据）即可。
用这些快捷方法，可以将上面$http的GET请求修改成：
<pre><code>// 快捷的GET请求
$http.get('/api/users.json');</code></pre>
+ get()<br/>
这个方法是发送GET请求的快捷方式。<br/>
get()函数可以接受两个参数。<br/>
url（字符串）：一个绝对或相对路径的URL，代表请求的目的地。<br/>
config（可选，对象）：这是一个可选的设置对象。<br/>
get()方法返回HttpPromise对象。<br/>
+ delete()<br/>
这是用来发送DELETE请求的快捷方式。<br/>
delete()函数可以接受两个参数。<br/>
url（字符串）：一个绝对或相对路径的URL，代表请求的目的地。<br/>
config（可选，对象）：这是一个可选的设置对象。<br/>
delete()方法返回HttpPromise对象。<br/>
+ head()<br/>
这是用来发送HEAD请求的快捷方式。<br/>
head()函数可以接受两个参数。<br/>
url（字符串）：一个绝对或相对路径的URL，代表请求的目的地。<br/>
config（可选，对象）：这是一个可选的设置对象。<br/>
head()方法返回HttpPromise对象。<br/>
+ jsonp()<br/>
这是用来发送JSONP请求的快捷方式。<br/>
jsonp()函数可以接受两个参数。<br/>
url（字符串）：一个绝对或相对路径的URL，代表请求的目的地。为了发送JSONP请求，其中必须包含JSON_CALLBACK字样。例如：<br/>
$http.jsonp("/api/users.json?callback=JSON_CALLBACK");<br/>
config（可选，对象）：这是一个可选的设置对象。<br/>
jsonp()方法返回HttpPromise对象。<br/>
+ post()
这是用来发送POST请求的快捷方式。<br/>
post()函数可以接受三个参数。<br/>
url（字符串）<br/>
一个绝对或相对路径的URL，代表请求的目的地。<br/>
data（对象或字符串）：这个对象包含请求的数据。<br/>
config（可选，对象）：这是一个可选的设置对象。<br/>
post()方法返回HttpPromise对象。<br/>
+ put()
这是用来发送PUT请求的快捷方式。<br/>
put()函数可以接受三个参数。<br/>
url（字符串）：一个绝对或相对路径的URL，代表请求的目的地。<br/>
data（对象或字符串）：这个对象包含请求的数据。<br/>
config（可选，对象）：这是一个可选的设置对象。<br/>
put()方法返回HttpPromise对象。<br/>
# 设置对象
当我们将$http当作函数来调用时，需要传入一个设置对象，用来说明如何构造XHR对象。例如，可以像下面这样将$http当作函数来调用：
<pre><code>$http({
	method: 'GET',
	url: '/api/users.json',
	params: {
		'username': 'auser'
	}
});</code></pre>
设置对象可以包含以下键。
1. method（字符串）<br/>
这个键是我们希望发送的请求的HTTP方法。它的值是下列各项其中之一：‘GET’、‘DELETE’、‘HEAD’、‘JSONP’、‘POST’、‘PUT’。
2. url（字符串）<br/>
绝对或相对的URL，是请求的目标。
3. params（字符串map或对象）<br/>
这个键的值是一个字符串map或对象，会被转换成查询字符串追加在URL后面。如果值不是字符串，会被JSON序列化。
<pre><code>// 参数会转化为?name=ari的形式
$http({
	params: {
		'name': 'ari'
	}
})</code></pre>
4. data（字符串或对象）<br/>
这个对象中包含了将会被当作消息体发送给服务器的数据。通常在发送POST请求时使用。<br/>
从AngularJS 1.3开始，它还可以在POST请求里发送二进制数据。要发送一个blob对象，你可以简单地通过使用data参数来传递它。例如：
<pre><code>var blob = new Blob(['Hello World'], {type: 'text/plain'});
$http({
	method: 'POST',
	url: '/',
	data: blob
});</code></pre>
5. headers（对象）<br/>
一个列表，每一个元素都是一个函数，它会返回代表随请求发送的HTTP头。如果函数的返回值是null，对应的头不会被发送。
6. xsrfHeaderName（字符串）<br/>
保存XSFR令牌的HTTP头的名称。
7. xsrfCookieName（字符串）<br/>
保存XSFR令牌的cookie的名称。
8. transformRequest（函数或函数数组）<br/>
这是一个函数或函数数组，用来对HTTP请求的请求体和头信息进行转换，并返回转换后的版本。通常用于在请求发送给服务器之前对其进行序列化。
这个函数看起来是这样的：
<pre><code>function(data,headers) {}</code></pre>
9. transformResponse（函数或函数数组）<br/>
这是一个函数或函数数组，用来对HTTP响应的响应体和头信息进行转换，并返回转换后的版本。通常用来进行反序列化。
这个函数看起来是这样的：
<pre><code>function(data,headers) {}</code></pre>
10. cache（布尔型或缓存对象）<br/>
如果cache属性被设置为true，那么AngularJS会用默认的$http缓存来对GET请求进行缓存。如果cache属性被设置为一个$cacheFactory对象的实例，那么这个对象会被用来对GET请求进行缓存。
11. timeout（数值型或promise对象）<br/>
如果timeout被设置为一个数值，那么请求将会在推迟timeout指定的毫秒数后再发送。如果被设置为一个promise对象，那么当该promise对象被resolve时请求会被中止。
12. withCredentials（布尔型）<br/>
如果该属性被设置为true，那么XHR请求对象中会设置withCredentials标记。
默认情况下，CORS请求不会发送cookie，而withCredentials标记会在请求中加入Access-Control-Allow-Credentials头，这样请求就会将目标域的cookie包含在请求中。
13. responseType（字符串）<br/>
responseType选项会在请求中设置XMLHttpRequestResponseType属性。我们可以使用以下HTTP请求类型其中之一：
+ ""（字符串，默认）；
+ "arraybuffer"（ArrayBuffer）；
+ "blob"（blob对象）；
+ "document"（HTTP文档）；
+ "json"（从JSON对象解析而来的JSON字符串）；
+ "text"（字符串）；
+ "moz-blob"（Firefox的接收进度事件）；
+ "moz-chunked-text"（文本流）；
+ "moz-chunked-arraybuffer"（ArrayBuffer流）。
# 响应对象
AngularJS传递给then()方法的响应对象包含四个属性。
+ data（字符串或对象）<br/>
这个数据代表转换过后的响应体（如果定义了转换的话）。
+ status（数值型）<br/>
响应的HTTP状态码。
+ headers（函数）<br/>
这个函数是头信息的getter函数，可以接受一个参数，用来获取对应名字的值。例如，用如下代码获取X-Auth-ID的值：
<pre><code>$http({
	method: 'GET',
	url: '/api/users.json'
}).then(function(resp) {
	// 读取X-Auth-ID
	resp.headers('X-Auth-ID');
});</code></pre>
+ config（对象）<br/>
这个对象是用来生成原始请求的完整设置对象。
statusText（字符串）
这个字符串是响应的HTTP状态文本。
# 缓存HTTP请求
默认情况下，$http服务不会对请求进行本地缓存。在发送单独的请求时，我们可以通过向$http请求传入一个布尔值或者一个缓存实例来启用缓存。
<pre><code>$http.get('/api/users.json', {
	cache: true
})
.success(function(data) {})
.error(function(data) {});</code></pre>
第一次发送请求时，$http服务会向/api/users.json发送一个GET请求。第二次发送同一个GET请求时，$http服务会从缓存中取回请求的结果，而不会真的发送一个HTTP GET请求。<br/>
在这个例子里，由于设置了启用缓存，AngularJS默认会使用$cacheFactory,这个服务是AngularJS在启动时自动创建的。<br/>
如果想要对AngularJS使用的缓存进行更多的自定义控制，可以向请求传入一个自定义的缓存实例代替true。<br/>
例如，如果要使用LRU（Least Recenlty Used，最近最少使用）缓存，可以像下面这样传入缓存实例对象：
<pre><code>var lru = $cacheFactory('lru', {
	capacity: 20
});</code></pre>
// $http请求
<pre><code>$http.get('/api/users.json', {
		cache: lru
	})
	.success(function(data) {})
	.error(function(data) {});</code></pre>
现在，最新的20个请求会被缓存。第21个请求会导致LRU从缓存中将时间比较老的请求移除掉。<br/>
每次发送请求时都传入一个自定义缓存是很麻烦的事情（即使是在服务中）。可以通过应用的.config()函数给所有$http请求设置一个默认的缓存：
<pre><code>angular.module('myApp', [])
	.config(function($httpProvider, $cacheFactory) {
		$httpProvider.defaults.cache = $cacheFactory('lru', {
			capacity: 20
		});
	});</code></pre>
这样所有的请求都会使用我们自定义的LRU缓存了。
# 拦截器
任何时候如果我们想要为请求添加全局功能，例如身份验证、错误处理等，在请求发送给服务器之前或者从服务器返回时对其进行拦截，是比较好的实现手段。<br/>
例如对于身份验证，如果服务器返回401状态码，我们会希望将用户重定向到登录页面。<br/>
AngularJS通过拦截器提供了一个从全局层面对响应进行处理的途径。<br/>
拦截器，尽管名字听起来很唬人，实际上是$http服务的基础中间件，用来向应用的业务流程中注入新的逻辑。<br/>
拦截器的核心是服务工厂（查看第14章获得更多关于服务的信息），通过向$httpProvider.interceptors数组中添加服务工厂，在$httpProvider中进行注册。<br/>
一共有四种拦截器，两种成功拦截器，两种失败拦截器。
+ request<br/>
AngularJS通过$http设置对象来对请求拦截器进行调用。它可以对设置对象进行修改，或者创建一个新的设置对象，它需要返回一个更新过的设置对象，或者一个可以返回新的设置对象的promise。
+ response<br/>
AngularJS通过$http设置对象来对响应拦截器进行调用。它可以对响应进行修改，或者创建一个新的响应，它需要返回一个更新过的响应，或者一个可以返回新响应的promise。
+ requestError<br/>
AngularJS会在上一个请求拦截器抛出错误，或者promise被reject时调用此拦截器。
+ responseError<br/>
AngularJS会在上一个响应拦截器抛出错误，或者promise被reject时调用此拦截器。

调用模块的.factory()方法来创建拦截器，可以在服务中添加一种或多种拦截器：
<pre><code>angular.module('myApp', [])
	.factory('myInterceptor', function($q) {
		var interceptor = {
			'request': function(config) {
				// 成功的请求方法
				return config; // 或者 $q.when(config);
			},
			'response': function(response) {
				// 响应成功
				return response; // 或者 $q.when(config);
			},
			'requestError': function(rejection) {
				// 请求发生了错误，如果能从错误中恢复，可以返回一个新的请求或promise
				return response; // 或新的promise
				// 或者，可以通过返回一个rejection来阻止下一步
				// return $q.reject(rejection);
			},
			'responseError': function(rejection) {
				// 请求发生了错误，如果能从错误中恢复，可以返回一个新的响应或promise
				return rejection; // 或新的promise
				// 或者，可以通过返回一个rejection来阻止下一步
				// return $q.reject(rejection);
			}
		};
		return interceptor;
	});</code></pre>
我们需要使用$httpProvider在.config()函数中注册拦截器：
<pre><code>angular.module('myApp', [])
	.config(function($httpProvider) {
		$httpProvider.interceptors.push('myInterceptor');
	});</code></pre>
1.3之前，可以通过返回定义拦截器函数的方式注册响应拦截器。但是在Angular 1.3中不再支持这种方式，你必须使用返回拦截器配置对象的方式，就像上面那样。这种语法适用于Angular 1.4及以上版本。<br/>
# 设置$httpProvider
使用.config()可以向所有请求中添加特定的HTTP头，这非常有用，尤其是我们希望将身份验证的头同请求一同发送，或设置响应类型的时候。<br/>
默认的请求头保存在$httpProvider.defaults.headers.common对象中。默认的头如下所示：<br/>
Accept: application/json, text/plain, */*
通过.config()函数可以对这些头进行修改或扩充，如下所示：
<pre><code>angular.module('myApp', [])
	.config(function($httpProvider) {
		$httpProvider.defaults.headers
			.common['X-Requested-By'] = 'MyAngularApp';
	});</code></pre>
也可以在运行时通过$http对象的defaults属性对这些默认值进行修改。例如，通过如下方法可以动态添加一个头：
<pre><code>$http.defaults.common['X-Auth'] = 'RandomString';</code></pre>
这个功能可以通过使用请求转换器实现，对于单个请求，也可以通过设置$http请求的headers选项实现。<br/>
也可以只对POST和PUT类型的请求进行设置。POST请求的默认头如下所示：<br/>
`Content-Type: application/json`<br/>
可以在.config()函数中对POST请求的头进行修改或扩充，如下所示：
<pre><code>angular.module('myApp', [])
	.config(function($httpProvider) {
		$httpProvider.defaults.headers
			.post['X-Posted-By'] = 'MyAngularApp';
	});</code></pre>
也可以对所有的PUT请求做同样的设置。PUT请求的默认头如下所示：<br/>
`Content-Type: application/json`<br/>
可以在.config()函数中对PUT请求的头进行修改或扩充，如下所示：
<pre><code>angular.module('myApp', []).config(function($httpProvider) {
	$httpProvider.defaults.headers
		.put['X-Posted-By'] = 'MyAngularApp';
});</code></pre>
