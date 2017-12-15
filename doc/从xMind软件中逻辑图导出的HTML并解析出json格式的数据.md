# 从xMind软件中将树形逻辑图解析成对应结构的JSON数据

### 从XMind软件中将树形逻辑图导出HTML格式文件，结构类似下面
<pre><code></code>&lt;body&gt;
	&lt;h1&gt;
		&lt;a href=""&gt;0&lt;/a&gt;
	&lt;/h1&gt;
	&lt;h2&gt;
		&lt;a href=""&gt;0.1&lt;/a&gt;
	&lt;/h2&gt;
	&lt;h3&gt;
		&lt;a href=""&gt;0.1.0&lt;/a&gt;
	&lt;/h3&gt;
	&lt;h3&gt;
		&lt;a href=""&gt;0.1.1&lt;/a&gt;
	&lt;/h3&gt;
	&lt;h2&gt;
		&lt;a href=""&gt;0.2&lt;/a&gt;
	&lt;/h2&gt;
	&lt;h3&gt;
		&lt;a href=""&gt;0.2.0&lt;/a&gt;
	&lt;/h3&gt;
	&lt;h2&gt;
		&lt;a href=""&gt;0.3&lt;/a&gt;
	&lt;/h2&gt;
	&lt;h3&gt;
		&lt;a href=""&gt;0.3.0&lt;/a&gt;
	&lt;/h3&gt;
	&lt;h3&gt;
		&lt;a href=""&gt;0.3.1&lt;/a&gt;
	&lt;/h3&gt;
&lt;/body&gt;</code></pre>
DOM结构特点如下：
+ 树的结构是从根节点开始解析，第一级用H1，第二级用H2，第三级用H3，后面每增加一级就用在上一级基础上多加一个空格，空格在HTML里面用的是实体字符`&nbsp;`

### 从HTML文件中解析出数据的js代码如下：
<pre><code>/**
 * h1 最顶级标题
 * h2 次一级标题
 * h3 此次一级标题
 * h3+n*(&nbsp;)没多一个空格实体，则层次加一层
 */
'use strict';
var allTopicObject = [];
var result = [];

//解析数据的真实层级
function parseTopicElement(text, layer) {
	var SPACE = '&nbsp;';
	var SPACE_LENGTH = 6;
	layer = layer || 1;
	if (text.indexOf(SPACE) !== 0) {
		return {
			title: text,
			layer: layer
		};
	} else {
		layer++;
		text = text.substr(SPACE_LENGTH);
		return parseTopicElement(text, layer);
	}
}

//从DOM中解析出相关数据
function parseTopicObject(topicElement) {
	var topicObject = {
		title: '',
		layer: 0
	};
	if (topicElement.firstElementChild === null) { //如果没有子元素，则停止执行
		return;
	}
	var text = topicElement.firstElementChild.innerHTML;
	if (topicElement.nodeName === 'H1') {
		topicObject.layer = 0;
		topicObject.title = text;
	} else if (topicElement.nodeName === 'H2') {
		topicObject.layer = 1;
		topicObject.title = text;
	} else if (topicElement.nodeName === 'H3') {
		//结合实体空格的数量来确定层级
		topicObject = parseTopicElement(text);
	} else {
		console.log(topicElement.nodeName);
	}
	allTopicObject.push(topicObject);
}
//获取body下所有的第一级子元素
var rootTopicElement = document.getElementsByTagName('h1')[0];
var nextTopicElement = rootTopicElement;
//解析上面结果中每个元素的层级，通过标签和空格实体数量
do {
	parseTopicObject(nextTopicElement);
	nextTopicElement = nextTopicElement.nextElementSibling;
} while (nextTopicElement);

//解析下一个对象的位置
var curPosition = [-1];

//解析坐标
function resolveNextObjectPosition(layer) {
	var length = curPosition.length;
	if (layer + 1 === length) { //同级
		curPosition[length - 1] = curPosition[length - 1] + 1;
	} else if (layer + 1 > length) { //子级
		curPosition.push(0);
	} else { //其他
		curPosition.length = layer + 1;
		curPosition[layer] = curPosition[layer] + 1;
	}
	return curPosition.join(',').split(',');

}

/**
 * [搜索数据插座]
 * @param  {[Array]} node     [祖先节点]
 * @param  {[Array]} position [位置坐标]
 * @return {[Array]}          [目标插座]
 */
function fillItopicByPosition(node, position) {
	if (position.length === 1) {
		return node;
	} else {
		var newNode = node[position[0]];
		if (newNode.iTopic === undefined) {
			newNode.iTopic = [];
		}
		newNode = newNode.iTopic;
		position.shift();
		return fillItopicByPosition(newNode, position);
	}
}

//组装数据结构
var i, ii = allTopicObject.length;
for (i = 0; i < ii; i++) {
	//确定位置
	var position = resolveNextObjectPosition(allTopicObject[i].layer);
	var arr = fillItopicByPosition(result, position);
	arr.push({
		'topic': allTopicObject[i].title
	});
}
console.log(JSON.stringify(result));</code></pre>

### 解析angularJs思维导图结果如下：
<pre><code>[{
	"topic": "angularJs",
	"iTopic": [{
		"topic": "模块",
		"iTopic": [{
			"topic": "声明模块：angular.module('moduleName',[requires])",
			"iTopic": [{
				"topic": "参数一:模块名称"
			},
			{
				"topic": "参数二：依赖的模块名数组"
			}]
		},
		{
			"topic": "获取已定义的模块：angualr.moduel('moduleName')"
		}]
	},
	{
		"topic": "作用域",
		"iTopic": [{
			"topic": "$scope对象是定义应用业务逻辑、控制器方法和视图属\r\n性的地方。"
		},
		{
			"topic": "AngularJS启动并生成视图时，会将根ng-app元素同$rootScope进行绑定。$rootScope是所有$scope对象的最上层。"
		},
		{
			"topic": "$scope对象就是一个普通的JavaScript对象，我们可以在其上随意修改或添加属性。$scope的所有属性，都可以自动被视图访问到。"
		},
		{
			"topic": "基本功能",
			"iTopic": [{
				"topic": "提供观察者以监视数据模型的变化"
			},
			{
				"topic": "可以将数据模型的变化通知给整个应用，甚至是系统外的组件"
			},
			{
				"topic": "可以进行嵌套，隔离业务功能和数据"
			},
			{
				"topic": "给表达式提供运算时所需的执行环境"
			}]
		},
		{
			"topic": "生命周期",
			"iTopic": [{
				"topic": "创建：在创建控制器或指令时，AngularJS会用$injector创建一个新的作用域，并在这个新建的控制器或指令运行时将作用域传递进去。"
			},
			{
				"topic": "链接：当Angular开始运行时，所有的$scope对象都会附加或者链接到视图中"
			},
			{
				"topic": "更新：当事件循环运行时，它通常执行在顶层$scope对象上（被称作$rootScope），每个子作用域都执行自己的脏值检测。每个监控函数都会检\r\n查变化。如果检测到任意变化，$scope对象就会触发指定的回调函数。"
			},
			{
				"topic": "销毁：当一个$scope在视图中不再需要时，这个作用域将会清理和销毁自己。"
			}]
		},
		{
			"topic": "作用域嵌套",
			"iTopic": [{
				"topic": "除了孤立作用域外，所有的作用域都通过原型继承而来，也就是说它们都可以访问父级作用域。默认情况下，AngularJS在当前作用域中无法找到某个属性时，便会在父级作用域中进行查找。如果AngularJS找不到对应的属性，会顺着父级\r\n作用域一直向上寻找，直到抵达$rootScope为止。如果在$rootScope中也找不到，程序会继续运行，但视图无法更新。"
			}]
		}]
	},
	{
		"topic": "控制器",
		"iTopic": [{
			"topic": "AngularJS中的控制器是一个函数，用来向视图的作用域中添加额外的功能。我们用它来给作用域对象设置初始状态，并添加自定义行为。"
		},
		{
			"topic": "控制器可以将与一个独立视图相关的业务逻辑封装在一个独立的容器中。"
		},
		{
			"topic": "控制器并不适合用来执行DOM操作、格式化或数据操作，以及除存储数据模型之外\r\n的状态维护操作。它只是视图和$scope之间的桥梁。"
		}]
	},
	{
		"topic": "表达式",
		"iTopic": [{
			"topic": "表达式和eval(javascript)非常相似，但是由于表达式由AngularJS来处理，它们有以下显著不同的特性：",
			"iTopic": [{
				"topic": "所有的表达式都在其所属的作用域内部执行，并有访问本地$scope的权限；"
			},
			{
				"topic": "如果表达式发生了TypeError和ReferenceError并不会抛出异常；"
			},
			{
				"topic": "不允许使用任何流程控制功能（条件控制，例如if/eles）；"
			},
			{
				"topic": "可以接受过滤器和过滤器链。"
			}]
		},
		{
			"topic": "插值字符串",
			"iTopic": [{
				"topic": "$interpolate服务返回一个函数，用来在特定的上下文中运算表达式。在控制器中注入$interpolate服务，可以在特定的上下文中运算表达式。",
				"iTopic": [{
					"topic": "参数",
					"iTopic": [{
						"topic": "text（字符串）： 一个包含字符插值标记的字符串。"
					},
					{
						"topic": "mustHaveExpression（布尔型）： 如果将这个参数设为true，当传入的字符串中不含有表达式时会返回null。"
					},
					{
						"topic": "trustedContext（字符串）：AngularJS会对已经进行过字符插值操作的字符串通过$sce.getTrusted()方法进行严格的上下文转义。"
					}]
				}]
			},
			{
				"topic": "修改{{ }}表达式的开始和结束的开始结束符号",
				"iTopic": [{
					"topic": "在$interpolateProvider中配置\r\n用startSymbol()方法可以修改标识开始的符号。这个方法接受一个参数。\r\nvalue（字符型）——开始符号的值。\r\n用endSymbol()方法可以修改标识结束的符号。这个方法也接受一个参数：\r\nvalue（字符型）—— 结束符号的值。"
				}]
			}]
		}]
	},
	{
		"topic": "表单验证"
	},
	{
		"topic": "指令",
		"iTopic": [{
			"topic": "指令本质上就是AngularJS扩展具有自定义功能的HTML元素的途径"
		},
		{
			"topic": "内置指令",
			"iTopic": [{
				"topic": "布尔属性",
				"iTopic": [{
					"topic": "ng-disabled"
				},
				{
					"topic": "ng-readonly"
				},
				{
					"topic": "ng-checked"
				},
				{
					"topic": "ng-selected"
				}]
			},
			{
				"topic": "类布尔属性",
				"iTopic": [{
					"topic": "ng-href"
				},
				{
					"topic": "ng-src"
				}]
			},
			{
				"topic": "会创建新作用域",
				"iTopic": [{
					"topic": "ng-app"
				},
				{
					"topic": "ng-controller"
				},
				{
					"topic": "ng-include"
				},
				{
					"topic": "ng-switch"
				},
				{
					"topic": "ng-view"
				},
				{
					"topic": "ng-if"
				},
				{
					"topic": "ng-repeat"
				}]
			},
			{
				"topic": "其他",
				"iTopic": [{
					"topic": "ng-init"
				},
				{
					"topic": "ng-bind/{{ }}"
				},
				{
					"topic": "ng-cloak"
				},
				{
					"topic": "ng-bind-template"
				},
				{
					"topic": "ng-model"
				},
				{
					"topic": "ng-model-options"
				},
				{
					"topic": "ng-show/ng-hide"
				},
				{
					"topic": "ng-form"
				},
				{
					"topic": "ng-class"
				},
				{
					"topic": "ng-attr-(suffix)"
				},
				{
					"topic": "ng-submit"
				},
				{
					"topic": "ng-change"
				},
				{
					"topic": "ng-select"
				},
				{
					"topic": "ng-click"
				},
				{
					"topic": "……其他事件绑定指令"
				}]
			}]
		},
		{
			"topic": "自定义指令directive()",
			"iTopic": [{
				"topic": "name：指令的名字"
			},
			{
				"topic": "factory_function：函数",
				"iTopic": [{
					"topic": "返回对象",
					"iTopic": [{
						"topic": "restrict:",
						"iTopic": [{
							"topic": "E（元素）"
						},
						{
							"topic": "A（属性，默认值）"
						},
						{
							"topic": "C（类名）"
						},
						{
							"topic": "M（注释）"
						}]
					},
					{
						"topic": "priority:",
						"iTopic": [{
							"topic": "优先级（数值型），默认值0"
						}]
					},
					{
						"topic": "terminal:",
						"iTopic": [{
							"topic": "true：运行当前元素上比本指令优先级低的指令"
						},
						{
							"topic": "false：停止运行当前元素上比本指令优先级低的指令"
						}]
					},
					{
						"topic": "template:",
						"iTopic": [{
							"topic": "字符串：一段HTML文本"
						},
						{
							"topic": "函数function(tElement, tAttrs) (...}：参数为tElement和tAttrs，并返回一个代表模板的字符串"
						}]
					},
					{
						"topic": "templateUrl:",
						"iTopic": [{
							"topic": "一个代表外部HTML文件路径的字符串"
						},
						{
							"topic": "一个可以接受两个参数的函数，参数为tElement和tAttrs，并返回一个外部HTML文件路径的字符串。"
						}]
					},
					{
						"topic": "replace:",
						"iTopic": [{
							"topic": "true：替换指令元素"
						},
						{
							"topic": "false：默认，模板会被当做子元素插入到指令元素的内部"
						}]
					},
					{
						"topic": "scope:",
						"iTopic": [{
							"topic": "false：默认，使用父元素的作用域"
						},
						{
							"topic": "true：从父作用域继承并创建一个新的作用域"
						},
						{
							"topic": "{}：隔离作用域",
							"iTopic": [{
								"topic": "绑定策略",
								"iTopic": [{
									"topic": "本地作用域属性：使用@符号将本地作用域同DOM属性的值进行绑定。指令内部作用域可以使用外部作用域的变量"
								},
								{
									"topic": "双向绑定：通过=可以将本地作用域上的属性同父级作用域上的属性进行双向的数据绑定。"
								},
								{
									"topic": "父级作用域绑定：通过&amp;符号可以对父级作用域进行绑定，以便在其中运行函数。"
								}]
							}]
						}]
					},
					{
						"topic": "transclude:",
						"iTopic": [{
							"topic": "false：默认，不允许指令中嵌入指令"
						},
						{
							"topic": "true：允许指令中嵌入指令，可以将任意内容和作用域传递给指令\r\n嵌入的内容放到保护ng-transclude指令的元素里面"
						}]
					},
					{
						"topic": "controller:",
						"iTopic": [{
							"topic": "字符串：控制器名称"
						},
						{
							"topic": "函数：通过匿名方式注册一个内联的控制器\r\n控制器可以注入一下服务：\r\n$scope：与指令元素相关联的当前作用域。\r\n$element：当前指令对应的元素。\r\n$attrs：由当前元素的属性组成的对象\r\n$transclude：嵌入链接函数会与对应的嵌入作用域进行预绑定。transclude链接函数是实际被执行用来克隆元素和操作DOM的函数。"
						}]
					},
					{
						"topic": "controllerAs:",
						"iTopic": [{
							"topic": "设置控制器的别名，可以以此为名来发布控制器，并且作用域可以访问controllerAs。\r\n可以在视图中引用控制器，无需注入$scope。"
						}]
					},
					{
						"topic": "require:",
						"iTopic": [{
							"topic": "参数可以被设置为字符串或数组，字符串或数组元素的值是会在当前指令的作用域中使用的指令名称"
						},
						{
							"topic": "依赖指令的控制器会被注入到指令链接函数中（第四个参数）"
						},
						{
							"topic": "通过前缀修改来改变查找控制器时的行为",
							"iTopic": [{
								"topic": "?：如果在当前指令中没有找到所需要的控制器，会将null作为传给link函数的第四个参数。"
							},
							{
								"topic": "^：指令会在上游的指令链中查找require参数所指定的控制器。"
							},
							{
								"topic": "?^：如果在当前指令中没有找到，则在上游指令链中查找依赖的指令的控制器"
							},
							{
								"topic": "没有前缀：指令将会在自身所提供的控制器中进行查找，如果没有找到任何控制器（或具有指定名字的指令）就抛出一个错误。"
							}]
						}]
					},
					{
						"topic": "link:",
						"iTopic": [{
							"topic": "用link函数创建可以操作DOM的指令\r\n链接函数是可选的。如果定义了编译函数，它会返回链接函数，因此当两个函数都定义了时，编译函数会重载链接函数。"
						}]
					},
					{
						"topic": "compile:"
					},
					{
						"topic": "指令的控制器和link函数可以互换",
						"iTopic": [{
							"topic": "控制器主要是用来提供可在指令间复用的行为，但链接函数只能在当前内部指令中定义行为，且无法在指令间复用。"
						},
						{
							"topic": "link函数可以将指令互相隔离开来，而controller则定义可复用的行为。"
						}]
					}]
				},
				{
					"topic": "返回函数：链接传递（postLink）函数，定义指令的链接（link）功能"
				}]
			}]
		}]
	},
	{
		"topic": "服务"
	},
	{
		"topic": "过滤器",
		"iTopic": [{
			"topic": "过滤器用来格式化需要展示给用户的数据。过滤器本质上是一个会把我们输入的内容当作参数传入进去的函数。"
		},
		{
			"topic": "在HTML中的模板绑定符号{{ }}内通过|符号来调用过滤器，如果需要传递参数给过滤器，只要在过滤器名字后面加冒号即可。如果有多个参数，可以在每个参数后面都加\r\n入冒号。"
		},
		{
			"topic": "在JavaScript代码中可以通过$filter服务来调用过滤器。"
		},
		{
			"topic": "内置过滤器",
			"iTopic": [{
				"topic": "currency"
			},
			{
				"topic": "date"
			},
			{
				"topic": "filter"
			},
			{
				"topic": "json"
			},
			{
				"topic": "limitTo"
			},
			{
				"topic": "lowercase"
			},
			{
				"topic": "uppercase"
			},
			{
				"topic": "number"
			},
			{
				"topic": "orderBy"
			}]
		},
		{
			"topic": "自定义过滤器",
			"iTopic": [{
				"topic": "在Angular的module中调用filter函数"
			}]
		}]
	},
	{
		"topic": "数据绑定"
	},
	{
		"topic": "路由"
	},
	{
		"topic": "依赖注入"
	},
	{
		"topic": "HTTP"
	},
	{
		"topic": "动画"
	},
	{
		"topic": "脏检查"
	}]
}]</code></pre>

### 解析angular思维导图结果如下：
<pre><code>[{
	"topic": "angular",
	"iTopic": [{
		"topic": "模块",
		"iTopic": [{
			"topic": "模块化",
			"iTopic": [{
				"topic": "模块把组件、指令和管道打包成内聚的功能块，每个模块聚焦于一个特性区域、业务领域、工作流或通用工具。"
			}]
		},
		{
			"topic": "根模块",
			"iTopic": [{
				"topic": "每个应用都至少有一个 Angular 模块，也就是根模块，用来引导并运行应用。"
			}]
		},
		{
			"topic": "特性模块",
			"iTopic": [{
				"topic": "一个内聚的代码块专注于某个应用领域、工作流或紧密相关的功能。"
			}]
		},
		{
			"topic": "ngModule与JavaScript模块的对比",
			"iTopic": [{
				"topic": "完全不同且完全无关"
			},
			{
				"topic": "这两个模块化系统是互补的"
			}]
		}]
	},
	{
		"topic": "装饰器",
		"iTopic": [{
			"topic": "装饰器是一个函数，它将元数据添加到类、类成员（属性、方法）和函数参数。"
		},
		{
			"topic": "@NgModule",
			"iTopic": [{
				"topic": "declarations ：声明本模块中拥有的视图类。 Angular 有三种视图类：组件、指令和管道。"
			},
			{
				"topic": "exports ： declarations 的子集，可用于其它模块的组件模板。"
			},
			{
				"topic": "imports ： 本模块声明的组件模板需要的类所在的其它模块。"
			},
			{
				"topic": "providers ： 服务的创建者，并加入到全局服务列表中，可用于应用任何部分。"
			},
			{
				"topic": "bootstrap ： 指定应用的主视图（称为根组件），它是所有其它视图的宿主。只有根模块才能设置bootstrap属性。"
			}]
		},
		{
			"topic": "@Directive",
			"iTopic": [{
				"topic": "声明当前类是一个指令，并提供关于该指令的元数据"
			},
			{
				"topic": "moduleId: module.id。如果设置了，templateUrl和styleUrl会被解析成相对于组件的。"
			},
			{
				"topic": "viewProviders ： 依赖注入provider的数组，局限于当前组件的视图中。"
			},
			{
				"topic": "template : 当前组件视图的内联模板"
			},
			{
				"topic": "templateUrl ： 当前组件视图的外部模板地址"
			},
			{
				"topic": "styles : 内联CSS样式，用于给组件的视图添加样式。"
			},
			{
				"topic": "styleUrls : 外部样式表URL的列表，用于给组件的视图添加样式。"
			}]
		},
		{
			"topic": "@Component",
			"iTopic": [{
				"topic": "moduleId: 为与模块相关的 URL（例如templateUrl）提供基地址。"
			},
			{
				"topic": "selector： CSS 选择器"
			},
			{
				"topic": "templateUrl：组件 HTML 模板的模块相对地址"
			},
			{
				"topic": "providers - 组件所需服务的依赖注入提供商数组。"
			}]
		},
		{
			"topic": "@Injectable",
			"iTopic": [{
				"topic": "声明当前类有一些依赖，当依赖注入器创建该类的实例时，这些依赖应该被注入到构造函数中。"
			}]
		},
		{
			"topic": "@Pipe",
			"iTopic": [{
				"topic": "声明当前类是一个管道，并且提供关于该管道的元数据"
			}]
		},
		{
			"topic": "@Input()",
			"iTopic": [{
				"topic": "声明一个输入属性，以便我们可以通过属性绑定更新它。"
			}]
		},
		{
			"topic": "@Output()",
			"iTopic": [{
				"topic": "声明一个输出属性，以便我们可以通过事件绑定进行订阅。"
			}]
		},
		{
			"topic": "@HostBinding",
			"iTopic": [{
				"topic": "把宿主元素的属性(比如CSS类：valid)绑定到指令/组件的属性(比如：isValid)。"
			}]
		},
		{
			"topic": "@HostListener",
			"iTopic": [{
				"topic": "通过指令/组件的方法(例如onClick)订阅宿主元素的事件(例如click)，可选传入一个参数($event)。"
			}]
		},
		{
			"topic": "@ContentChild",
			"iTopic": [{
				"topic": "把组件内容查询(myPredicate)的第一个结果绑定到类的myChildComponent属性。"
			}]
		},
		{
			"topic": "@ContentChildren",
			"iTopic": [{
				"topic": "把组件内容查询(myPredicate)的全部结果，绑定到类的myChildComponents属性。"
			}]
		},
		{
			"topic": "@ViewChild",
			"iTopic": [{
				"topic": "把组件视图查询(myPredicate)的第一个结果绑定到类的myChildComponent属性。对指令无效。"
			}]
		},
		{
			"topic": "@ViewChildren",
			"iTopic": [{
				"topic": "把组件视图查询(myPredicate)的全部结果绑定到类的myChildComponents属性。对指令无效"
			}]
		},
		{
			"topic": "@SkipSelf",
			"iTopic": [{
				"topic": "在注入器injector层级中的祖先injector中查找 CoreModule,如果没有祖先injector则会提供一个CoreModule实例"
			}]
		}]
	},
	{
		"topic": "模板",
		"iTopic": [{
			"topic": "内联模板还是模板文件",
			"iTopic": [{
				"topic": "两种方式均可以，如果HTML模板比较大，使用模板文件更合适，否则可以使用内联模板"
			}]
		},
		{
			"topic": "模板中的HTML",
			"iTopic": [{
				"topic": "几乎所有的HTML语法都是有效的模板语法。例外是&lt;script&gt;元素。"
			},
			{
				"topic": "有些合法的 HTML 被用在模板中是没有意义的，如：&lt;html&gt;、&lt;body&gt;和&lt;base&gt;"
			},
			{
				"topic": "可以通过组件和指令来扩展模板中的 HTML 词汇。它们看上去就是新元素和属性。"
			}]
		},
		{
			"topic": "插值表达式",
			"iTopic": [{
				"topic": "插值表达式可以把计算后的字符串插入到 HTML"
			}]
		},
		{
			"topic": "模板表达式",
			"iTopic": [{
				"topic": "插值表达式可以把计算后的字符串插入到 HTML 元素标签内的文本或对标签的属性进行赋值。"
			}]
		},
		{
			"topic": "表达式上下文"
		},
		{
			"topic": "表达式指南"
		},
		{
			"topic": "模板语句"
		},
		{
			"topic": "语句上下文"
		},
		{
			"topic": "语句指南"
		},
		{
			"topic": "绑定语法",
			"iTopic": [{
				"topic": "概览"
			},
			{
				"topic": "绑定目标"
			},
			{
				"topic": "属性绑定",
				"iTopic": [{
					"topic": "属性绑定还是插值表达式"
				}]
			},
			{
				"topic": "attribute绑定"
			},
			{
				"topic": "css类绑定"
			},
			{
				"topic": "样式绑定"
			},
			{
				"topic": "事件绑定"
			},
			{
				"topic": "双向数据绑定"
			},
			{
				"topic": "模板引用变量"
			},
			{
				"topic": "输入输出属性"
			},
			{
				"topic": "模板表达式操作符",
				"iTopic": [{
					"topic": "管道操作符"
				},
				{
					"topic": "安全导航操作符和空属性路径"
				},
				{
					"topic": "非空断言操作符"
				}]
			}]
		}]
	},
	{
		"topic": "指令",
		"iTopic": [{
			"topic": "概述"
		},
		{
			"topic": "属性型指令",
			"iTopic": [{
				"topic": "创建属性型指令"
			},
			{
				"topic": "使用属性型指令"
			},
			{
				"topic": "内置属性型指令",
				"iTopic": [{
					"topic": "NgClass指令"
				},
				{
					"topic": "NgStyle指令"
				},
				{
					"topic": "NgModule"
				}]
			}]
		},
		{
			"topic": "结构型指令",
			"iTopic": [{
				"topic": "内置结构型指令",
				"iTopic": [{
					"topic": "NgIf指令"
				},
				{
					"topic": "NgFor指令"
				},
				{
					"topic": "NgFor指令"
				},
				{
					"topic": "NgSwitch指令"
				},
				{
					"topic": "&lt;ng-template&gt;指令"
				},
				{
					"topic": "&lt;ng-container&gt;指令"
				}]
			},
			{
				"topic": "微语法"
			},
			{
				"topic": "星号（*）前缀"
			},
			{
				"topic": "模板输入变量"
			},
			{
				"topic": "创建结构型指令"
			}]
		},
		{
			"topic": "生命周期",
			"iTopic": [{
				"topic": "生命周期钩子"
			},
			{
				"topic": "组件生命周期概览"
			},
			{
				"topic": "生命周期的顺序",
				"iTopic": [{
					"topic": "ngOnChanges()"
				},
				{
					"topic": "ngOnInit()"
				},
				{
					"topic": "ngDoCheck()"
				},
				{
					"topic": "ngAfterContentInit()"
				},
				{
					"topic": "ngAfterContentChecked()"
				},
				{
					"topic": "ngAfterViewInit()"
				},
				{
					"topic": "ngAfterViewChecked()"
				},
				{
					"topic": "ngOndestory()"
				}]
			},
			{
				"topic": "OnInit()钩子"
			},
			{
				"topic": "OnDestory()钩子"
			},
			{
				"topic": "OnChange()钩子"
			},
			{
				"topic": "Docheck()钩子"
			},
			{
				"topic": "AfterView()钩子"
			},
			{
				"topic": "AfterContent()钩子"
			},
			{
				"topic": "AfterView与AfterContent比较"
			}]
		},
		{
			"topic": "组件",
			"iTopic": [{
				"topic": "组件间的交互"
			},
			{
				"topic": "组件样式"
			},
			{
				"topic": "动态组件"
			}]
		}]
	},
	{
		"topic": "Forms"
	},
	{
		"topic": "管道",
		"iTopic": [{
			"topic": "使用管道"
		},
		{
			"topic": "对管道进行参数化"
		},
		{
			"topic": "链式管道"
		},
		{
			"topic": "自定义管道"
		},
		{
			"topic": "管道与变更检测",
			"iTopic": [{
				"topic": "纯管道"
			},
			{
				"topic": "非纯管道"
			}]
		}]
	},
	{
		"topic": "动画",
		"iTopic": [{
			"topic": "引入动画特性模块"
		},
		{
			"topic": "状态"
		},
		{
			"topic": "转场"
		},
		{
			"topic": "*（通配符）状态"
		},
		{
			"topic": "void状态"
		},
		{
			"topic": "可动的（Animatable）属性与单位"
		},
		{
			"topic": "自动属性计算"
		},
		{
			"topic": "动画时间线"
		},
		{
			"topic": "基于关键帧（keyframes）的多阶段动画"
		},
		{
			"topic": "并行动画组"
		},
		{
			"topic": "动画回调"
		}]
	},
	{
		"topic": "依赖注入 "
	},
	{
		"topic": "http"
	},
	{
		"topic": "服务"
	},
	{
		"topic": "事件"
	},
	{
		"topic": "路由"
	}]
}]</code></pre>
