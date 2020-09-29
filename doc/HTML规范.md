### DOCTYPE申明

为每个 HTML 页面的第一行添加 standards mode（标准模式） 声明，这样能够确保在每个浏览器中拥有一致的展现。
```
<!doctype html>
<html>
  <head>
  </head>
</html>
```

### 语言属性（Language attribute）
根据 HTML5 规范：
+ 强烈建议为 html 根元素指定 lang 属性，从而为文档设置正确的语言。这将有助于语音合成工具确定其所应该采用的发音，有助于翻译工具确定其翻译时所应遵守的规则等等。
```
<html lang="en">
  <!-- ... -->
</html>
```

### IE 兼容模式
IE 支持通过特定的 `<meta>` 标签来确定绘制当前页面所应该采用的 IE 版本。除非有强烈的特殊需求，否则最好是设置为 edge mode，从而通知 IE 采用其所支持的最新的绘制模式。
```
<meta http-equiv="x-ua-compatible" content="ie=edge">
```

### 字符编码
通过明确声明字符编码，能够确保浏览器快速并容易的判断页面内容的渲染方式。这样做的好处是，可以避免在 HTML 中使用字符实体标记（character entity），从而全部与文档编码一致（一般采用 UTF-8 编码）。
```
<head>
  <meta charset="UTF-8">
</head>
```

### title
页面必须包含 title 标签声明标题。
title 必须作为 head 的直接子元素，并紧随 charset 声明之后。
解释：
+ title 中如果包含 ASCII 之外的字符，浏览器需要知道字符编码类型才能进行解码，否则可能导致乱码。

```
<head>
    <meta charset="UTF-8">
    <title>页面标题</title>
</head>
```

### favicon
保证 favicon 可访问。
解释：
+ 在未指定 favicon 时，大多数浏览器会请求 Web Server 根目录下的 favicon.ico 。为了保证 favicon 可访问，避免 404，必须遵循以下两种方法之一：
    + 在 Web Server 根目录放置 favicon.ico 文件。
    + 使用 link 指定 favicon。

```
<link rel="shortcut icon" href="path/to/favicon.ico">
```
### viewport
viewport: 一般指的是浏览器窗口内容区的大小，不包含工具条、选项卡等内容；
+ width: 浏览器宽度，输出设备中的页面可见区域宽度；
+ device-width: 设备分辨率宽度，输出设备的屏幕可见宽度；
+ initial-scale: 初始缩放比例；
+ maximum-scale: 最大缩放比例；

viewport meta tag 可以设置可视区域的宽度和初始缩放大小，避免在移动设备上出现页面展示不正常。

比如，在页面宽度小于 980px 时，若需 iOS 设备友好，应当设置 viewport 的 width 值来适应你的页面宽度。同时因为不同移动设备分辨率不同，在设置时，应当使用 device-width 和 device-height 变量。

### CSS 和 JavaScript 引入
引入 CSS 时必须指明 rel="stylesheet"

引入 CSS 和 JavaScript 时无须指明 type 属性。

根据 HTML5 规范，在引入 CSS 和 JavaScript 文件时一般不需要指定 type 属性，因为 text/css 和 text/javascript 分别是它们的默认值。

表现定义放置于外部 CSS 中，行为定义放置于外部 JavaScript 中(结构-样式-行为的代码分离，对于提高代码的可阅读性和维护性都有好处)

在 head 中引入页面需要的所有 CSS 资源

JavaScript 应当放在页面末尾，或采用异步加载

移动环境或只针对现代浏览器设计的 Web 应用，如果引用外部资源的 URL 协议部分与页面相同，建议省略协议前缀

使用 protocol-relative URL 引入 CSS，在 IE7/8 下，会发两次请求。是否使用 protocol-relative URL 应充分考虑页面针对的环境。


### 大小写
标签、标签属性全部小写

### 缩进与换行
统一两个(或四个)空格缩进（总之缩进统一即可），不要使用 Tab 或者 Tab、空格混搭。

### 命名
class 必须单词全字母小写，单词间以 - 分隔

class 必须代表相应模块或部件的内容或功能，不得以样式信息进行命名

元素 id 必须保证页面唯一

id 建议单词全字母小写，单词间以 - 分隔。同项目必须保持风格一致

id、class 命名，在避免冲突并描述清楚的前提下尽可能短

如果ID用于js获取dom，则使用小驼峰命名方式

### 标签
+ 自闭合（self-closing）标签，无需闭合 ( 例如： img input br hr 等 )（HTML5 规范中明确说明这是可选的）；
+ 可选的闭合标签（closing tag），需闭合 ( 例如：`</li>` 或`</body>` )；
+ 尽量减少标签数量；

### 嵌套
a 不允许嵌套 div这种约束属于语义嵌套约束，与之区别的约束还有严格嵌套约束，比如a 不允许嵌套 a。

严格嵌套约束在所有的浏览器下都不被允许；而语义嵌套约束，浏览器大多会容错处理，生成的文档树可能相互不太一样。

#### 语义嵌套约束

`<li>` 用于 `<ul>` 或 `<ol>` 下；

`<dd>`, `<dt>` 用于 `<dl>` 下；

`<thead>`, `<tbody>`, `<tfoot>`, `<tr>`, `<td>` 用于 `<table>` 下；

#### 严格嵌套约束

inline-Level 元素，仅可以包含文本或其它 inline-Level 元素;

`<a>`里不可以嵌套交互式元素`<a>`、`<button>`、`<select>`等;

`<p>`里不可以嵌套块级元素`<div>`、`<h1>`~`<h6>`、`<p>`、`<ul>`/`<ol>`/`<li>`、`<dl>`/`<dt>`/`<dd>`、`<form>`等。


### HTML 标签的使用应该遵循标签的语义
+ p - 段落
+ h1,h2,h3,h4,h5,h6 - 层级标题
+ strong,em - 强调
+ ins - 插入
+ del - 删除
+ abbr - 缩写
+ code - 代码标识
+ cite - 引述来源作品的标题
+ q - 引用
+ blockquote - 一段或长篇引用
+ ul - 无序列表
+ ol - 有序列表
+ dl,dt,dd - 定义列表

### 引号
属性的定义，统一使用双引号。

### 属性顺序
+ id
+ class
+ name
+ data-*
+ src, for, type, href, value
+ title, alt
+ role, aria-*

### 布尔（boolean）型属性
布尔型属性可以在声明时不赋值。XHTML 规范要求为其赋值，但是 HTML5 规范不需要。
+ 元素的布尔型属性如果有值，就是 true，如果没有值，就是 false（存疑）。
+ 如果一定要为其赋值的话，其值必须是空字符串或 [ASCII case-insensitive](https://infra.spec.whatwg.org/#ascii-case-insensitive) 属性的规范名称，并且不要再收尾添加空白符。
```
<input type="text" disabled>

<input type="checkbox" value="1" checked>

<select>
  <option value="1" selected>1</option>
</select>
```

### 自定义属性建议以 `xxx-`为 前缀，推荐使用 `data-`
使用前缀有助于区分自定义属性和标准定义的属性。

### 图片
禁止 img 的 src 取值为空。延迟加载的图片也要增加默认的 src。
+ 解释：src 取值为空，会导致部分浏览器重新加载一次当前页面

避免为 img 添加不必要的 title 属性。
+ 解释：多余的 title 影响看图体验，并且增加了页面尺寸

为重要图片添加 alt 属性。
+ 解释：可以提高图片加载失败时的用户体验。

添加 width 和 height 属性，以避免页面抖动。

有下载需求的图片采用 img 标签实现，无下载需求的图片采用 CSS 背景图实现。

解释：

产品 logo、用户头像、用户产生的图片等有潜在下载需求的图片，以 img 形式实现，能方便用户下载。
无下载需求的图片，比如：icon、背景、代码使用的图片等，尽可能采用 CSS 背景图实现


### 表单标题

有文本标题的控件必须使用 label 标签将其与其标题相关联。

有两种方式：

将控件置于 label 内。
+ label 的 for 属性指向控件的 id。
+ 推荐使用第一种，减少不必要的 id。如果 DOM 结构不允许直接嵌套，则应使用第二种。

示例：
```
<label><input type="checkbox" name="confirm" value="on"> 我已确认上述条款</label>

<label for="username">用户名：</label> <input type="textbox" name="username" id="username">
```

### 按钮
使用 button 元素时必须指明 type 属性值。

button 元素的默认 type 为 submit，如果被置于 form 元素中，点击后将导致表单提交。为显示区分其作用方便理解，必须给出 type 属性。

尽量不要使用按钮类元素的 name 属性
由于浏览器兼容性问题，使用按钮的 name 属性会带来许多难以发现的问题。


### 多媒体
当在现代浏览器中使用 audio 以及 video 标签来播放音频、视频时，应当注意格式。

音频应尽可能覆盖到如下格式：
+ MP3
+ WAV
+ Ogg

视频应尽可能覆盖到如下格式：
+ MP4
+ WebM
+ Ogg

在支持 HTML5 的浏览器中优先使用 audio 和 video 标签来定义音视频元素。

使用退化到插件的方式来对多浏览器进行支持。

```
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    <object width="100" height="50" data="audio.mp3">
        <embed width="100" height="50" src="audio.swf">
    </object>
</audio>

<video width="100" height="50" controls>
    <source src="video.mp4" type="video/mp4">
    <source src="video.ogg" type="video/ogg">
    <object width="100" height="50" data="video.mp4">
        <embed width="100" height="50" src="video.swf">
    </object>
</video>
```

只在必要的时候开启音视频的自动播放。

在 object 标签内部提供指示浏览器不支持该标签的说明
```
<object width="100" height="50" data="something.swf">DO NOT SUPPORT THIS TAG</object>
```

### 参考文档
+ [前端HTML-CSS规范-阿里云开发者社区](https://developer.aliyun.com/article/51487)
+ [前端开发规范(一、HTML篇)-社区博客-网易数帆](https://sq.163yun.com/blog/article/229321197788643328)
+ [html-style-guide](https://github.com/ecomfe/spec/blob/master/html-style-guide.md)
+ [编码规范 by @mdo](https://codeguide.bootcss.com/)
+ [HTML代码规范](https://github.com/mgbq/front-end-Doc/blob/master/html.md)
+ [前端开发规范文档-前端开发博客](http://caibaojian.com/web-fontend-doc.html)
