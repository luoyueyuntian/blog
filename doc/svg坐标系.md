SVG坐标系的最重要的三个属性：viewport， viewBox， 和 preserveAspectRatio。

## SVG画布
画布是绘制SVG内容的一块空间或区域。


## viewport
SVG通过有限区域展现在屏幕上，这个区域叫做viewport。SVG中超出视窗边界的区域会被裁切并且隐藏。

整个SVG画布可见还是部分可见取决于这个canvas的尺寸以及preserveAspectRatio属性值

在SVG中，值可以带单位也不可以不带。一个不带单位的值可以在用户空间中通过用户单位声明。如果值通过用户单位声明，那么这个值的数值被认为和px单位的数值一样。
也可以使用单位来声明值。SVG支持的长度单位有：em，ex，px，pt，pc，cm，mm，in和百分比。

一旦你设定最外层SVG元素的宽高，浏览器会建立初始视窗坐标系和初始用户坐标系。

## 初始坐标系
### 初始视窗坐标系
初始视窗坐标系是一个建立在视窗上的坐标系。原点(0,0)在视窗的左上角，X轴正向指向右，Y轴正向指向下，初始坐标系中的一个单位等于视窗中的一个"像素"。

### 初始用户坐标系
初始用户坐标系是建立在SVG画布上的坐标系。这个坐标系一开始和视窗坐标系完全一样-它自己的原点位于视窗左上角，x轴正向指向右，y轴正向指向下。使用viewBox属性，初始用户坐标系统-也称当前坐标系，或使用中的用户空间-可以变成与视窗坐标系不一样的坐标系。

## viewBox
viewBox用来把SVG图形绘制到画布上的坐标系，这个坐标系可以大于视窗也可以小于视窗，在视窗中可以整体可见或部分可见。

一旦创建视窗坐标系（使用width和height），浏览器默认创建一个完全相同的用户坐标系。

如果选择的用户坐标系统和视窗坐标系统宽高比（高比宽）相同，它会延伸来适应整个视窗区域。如果用户坐标系宽高比不同，你可以用preserveAspectRatio属性来声明整个系统在视窗内是否可见，你也可以用它来声明在视窗中如何定位。

### viewBox语法
viewBox属性接收四个参数值，包括：&lt;min-x&gt;, &lt;min-y&gt;, width 和 height。
<pre><code>viewBox = &lt;min-x&gt; &lt;min-y&gt; &lt;width&gt; &lt;height&gt;</code></pre>
`<min-x>` 和 `<min-y>` 值决定viewBox的左上角，width和height决定视窗的宽高。这里要注意视窗的宽高不一定和父&lt;svg&gt;元素的宽高一样。&lt;width&gt;和&lt;height&gt;值为负数是不合法的。值为0的话会禁止元素的渲染。

注意视窗的宽度也可以在CSS中设置为任何值。例如：设置width:100%会让SVG视窗在文档中自适应。无论viewBox的值是多少，它会映射为外层SVG元素计算出的宽度值。
例如：
<pre><code>&lt;!-- The viewBox in this example is equal to the viewport, but it can be different --&gt; 
&lt;svg width="800" height="600" viewBox="0 0 800 600"&gt; 
	&lt;!-- SVG content drawn onto the SVG canvas --&gt; 
&lt;/svg&gt;
</code></pre>

### 用viewBox属性通过缩放或者变化使SVG图形变换
#### 缩放
&lt;min-x&gt;和&lt;min-y&gt;都设置成0。viewBox的宽高是viewport宽高的倍数（大于1是缩小并裁剪画布，小于一是放大）。
<pre><code>&lt;svg width="800" height="600" viewBox="0 0 400 300"&gt; 
	&lt;!-- SVG content drawn onto the SVG canvas --&gt; 
&lt;/svg&gt;</code></pre>
viewBox="0 0 400 300"表示:

+ 它声明了一个特定的区域，canvas横跨左上角的点(0,0)到点(400,300)。
+ SVG图像被这个区域裁切。
+ 区域被拉伸（类似缩放效果）来充满整个视窗。
+ 用户坐标系被映射到视窗坐标系-在这种情况下-一个用户单位等于两个视窗单位。

#### 裁剪
&lt;min-x&gt;和&lt;min-y&gt;都设置成为需要裁剪区域的起点，viewBox的width和height设置为要裁剪区域的大小

#### 移动变换
&lt;min-x&gt;和&lt;min-y&gt;都设置非0时有移动变换的效果，可以设置正值，也可以设置负值

#### 拉伸变换
viewBox的宽高与viewport宽高比不同会导致图形在某些方向上扭曲

## preserveAspectRatio
preserveAspectRatio可以改变视窗中viewBox的位置
### preserveAspectRatio属性
preserveAspectRatio属性强制统一缩放比来保持图形的宽高比。
preserveAspectRatio属性可以在保持宽高比的情况下强制统一viewBox的缩放比，并且如果不想用默认居中你可以声明viewBox在视窗中的位置。

#### preserveAspectRatio语法
<pre><code>preserveAspectRatio = defer? &lt;align&gt; &lt;meetOrSlice&gt;?</code></pre>

+ defer声明是可选的，并且只有当你在<image>上添加preserveAspectRatio才被用到。用在任何其他元素上时它都会被忽略。
+ align参数声明是否强制统一放缩，如果是，对齐方法会在viewBox的宽高比不符合viewport的宽高比的情况下生效。

如果align值设为none,图形不在保持宽高比而会缩放来适应视窗，其他所有preserveAspectRatio值都在保持viewBox的宽高比的情况下强制拉伸，并且指定在视窗内如何对齐viewBox。

> align参数使用9个值中的一个或者为none。任何除none之外的值都用来保持宽高比缩放图片，并且还用来在视窗中对齐viewBox。<br/>
当使用百分比值时，align值类似于background-position。你可以把viewBox当做背景图像。通过align定位和background-position的不同在于，不同于通过一个与视窗相关的点来声明一个特定的viewBox值，它把具体的viewBox“轴”和对应的视窗的“轴”对齐。<br/>
##### viewBox中的"min-x"、"min-y"、"max-x"和"max-y"、"mid-x"和"mid-y"轴
> viewBox的&lt;min-x&gt;和&lt;min-y&gt;定义viewBox中的"min-x"和"min-y"轴<br/>
viewBox的&lt;min-x&gt; + (&lt;width&gt;/2) 和 &lt;min-y&gt; + (&lt;height&gt;/2)定义"mid-x"和"mid-y"轴<br/>
viewBox的&lt;min-x&gt; + &lt;width&gt; 和 &lt;min-y&gt; + &lt;height&gt;定义"max-x"和"max-y"轴

+ meetOrSlice也是可选的，默认值为meet。这个属性声明整个viewBox在视窗中是否可见。如果是，它和align参数通过一个或多个空格分隔。meetOrSlice被用来声明viewBox是否会被完全包含在视窗中，或者它是否应该尽可能缩放来覆盖整个视窗，甚至意味着部分的viewBox会被“slice”。

meetOrSlice有两个可选值
> meet（默认值）<br/>
基于以下两条准侧尽可能缩放元素：<br/>
保持宽高比<br/>
整个viewBox在视窗中可见<br/>
在这个情况下，如果图形的宽高比不符合视窗，一些视窗会超出viewBox的边界（即viewBox绘制的区域会小于视窗）。在这个情况下，viewBox的边界被包含在viewport中使得边界满足。

> slice<br/>
在保持宽高比的情况下，缩放图形直到viewBox覆盖了整个视窗区域。viewBox被缩放到正好覆盖视窗区域（在两个维度上），但是它不会缩放任何超出这个范围的部分。换而言之，它缩放到viewBox的宽高可以正好完全覆盖视窗。<br/>
在这种情况下，如果viewBox的宽高比不适合视窗，一部分viewBox会扩展超过视窗边界（即，viewBox绘制的区域会比视窗大）。这会导致部分viewBox被切片。


参考文档：[理解SVG坐标系和变换：视窗,viewBox和preserveAspectRatio](https://www.w3cplus.com/html5/svg-coordinate-systems.html)
