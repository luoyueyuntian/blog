## 原生js修改元素样式

#### 直接修改元素style的属性  某些情况用这个设置 `!important`值无效

如果属性有'-'号，就写成驼峰的形式（如textAlign）  如果想保留 - 号，就中括号的形式  `element.style['text-align'] = 'center'`;
<pre><code>element.style.height = '100px';</code></pre>

#### 直接设置元素属性（只能用于某些属性，相关样式会自动识别）
<pre><code>element.setAttribute('height', 100);</code></pre>

#### 使用`setAttribute`设置元素的`style`属性
<pre><code>element.setAttribute('style', 'height: 100px !important');</code></pre>

#### 使用`setProperty`设置元素`style`的属性，如果要设置!important，推荐用这种方法设置第三个参数
<pre><code>element.style.setProperty('height', '300px', 'important');</code></pre>

#### 修改元素`class`属性，让元素应用不同的规则

因JS获取不到css的伪元素，所以可以通过改变伪元素父级的class来动态更改伪元素的样式
<pre><code>element.className = 'blue';
element.className += 'blue fb';</code></pre>

#### 设置元素的`style`的`cssText`内容，此方法可以批量修改多个样式，避免多次回流
<pre><code>element.style.cssText = 'height: 100px !important';
element.style.cssText += 'height: 100px !important';</code></pre>

#### 创建新的css样式文件（styleSheet）并插入到文档中
<pre><code>var styleElement = document.createElement('style');
styleElement.type = 'text/css';
styleElement.appendChild(document.createTextNode(styleText));
document.head.appendChild(styleElement);
</code></pre>

#### 使用addRule、insertRule添加、删除规则
+ `insertRule(rule,index)`： 	向样式表中插入一条新规则的 DOM 标准方法。
+ `deleteRule(index)`： 	从指定位置删除规则的 DOM 标准方法。
+ `addRule(selector,style,index)`： 	为一个样式表添加一条规则的特定于 IE 的方法。
+ `removeRule(index)`：	删除一条规则的特定于 IE 的方法。
<pre><code>document.styleSheets[0].addRule('.box', 'height: 100px');
document.styleSheets[0].insertRule('.box {height: 100px}', 0);
</code></pre>

#### 直接修改`CSSStyleRule`对象
使用 styleSheets 对象可以访问页面中所有样式表，包括用 `<style>` 标签定义的内部样式表，以及用 `<link>` 标签或 @import 命令导入的外部样式表。

`CSSStyleRule` 对象表示 CSS 样式表中一个单独的规则集（rule sets）。

通过document.styleSheets[n] // n表示期望修改的样式表序号，从0开始计数来获取到期望修改的样式表，通过cssRules获取到该样式表的所有规则，通过style修改特定规则的样式；

cssRules（或 rules）的 style 对象在访问 CSS 属性时，使用的是 CSS 脚本属性名，因此所有属性名称中不能使用连字符。

<pre><code>document.styleSheets[0].cssRules[0].style.height = '120px'
</code></pre>

#### 通过`classList`操作元素`class`属性
classList 属性返回元素的类名，该属性用于在元素中添加，移除及切换 CSS 类。classList 属性是只读的，但你可以使用 add() 和 remove() 方法修改它。
+ `length`属性：只读，返回类列表中类的数量
+ `add(class1, class2, ...)`：在元素中添加一个或多个类名。如果指定的类名已存在，则不会添加
+ `contains(class)`：判断指定的类名是否存在。
+ `item(index)`：返回元素中索引值对应的类名。索引值从 0 开始。如果索引值在区间范围外则返回 null
+ `remove(class1, class2, ...)`：移除元素中一个或多个类名。移除不存在的类名，不会报错。
+ `toggle(class, true|false)`：在元素中切换类名。第一个参数为要在元素中移除的类名，并返回 false。如果该类名不存在则会在元素中添加类名，并返回 true。第二个是可选参数，是个布尔值用于设置元素是否强制添加或移除类，不管该类名是否存在。
<pre><code>// 添加元素多个类:
document.getElementById("myDIV").classList.add("mystyle", "anotherClass", "thirdClass");

// 移除元素多个类:
document.getElementById("myDIV").classList.remove("mystyle", "anotherClass", "thirdClass");

// 切换元素类:
document.getElementById("myDIV").classList.toggle("newClassName");
</code></pre>
