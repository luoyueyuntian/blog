#### 使用`@keyframes`定义补间动画
<pre><code>@keyframes &lt;animation-name&gt; {
    &lt;position-1&gt;: {}
    &lt;position-2&gt;: {}
    ……
    ……
    &lt;position-n&gt;: {}
}</code></pre>

+ `animation-name`: 表示动画的名称，当一个元素要使用动画时，需要指定动画名称，说的就是这个地方的动画名称
+ `position`：表示关键帧应用的位置，可以使用百分比（如：0%， 50%等），也可以使用`from`、`to`，它们分别等效于`0%`、`100%`

#### 使用`animation`应用动画
当需要对某个元素应用某个动画时，需要在选择器中通过`animation`指定动画名称时动画时长。

<pre><code>&lt;selector&gt; {
    animation: animation-name animation-duration;
    -moz-animation: animation-name animation-duration;	/* Firefox */
    -webkit-animation: animation-name animation-duration;	/* Safari 和 Chrome */
    -o-animation: animation-name animation-duration;	/* Opera */
}</code></pre>

#### `animation`的所有配置项
+ `@keyframes`	规定动画。
+ `animation`	所有动画属性的简写属性，除了 animation-play-state 属性。
+ `animation-name`	规定 @keyframes 动画的名称。
+ `animation-duration`	规定动画完成一个周期所花费的秒或毫秒。默认是 0。
+ `animation-timing-function`	规定动画的速度曲线。默认是 "ease"。
+ `animation-delay`	规定动画何时开始。默认是 0。
+ `animation-iteration-count`	规定动画被播放的次数。默认是 1。
+ `animation-direction`	规定动画是否在下一周期逆向地播放。默认是 "normal"。
+ `animation-play-state`	规定动画是否正在运行或暂停。默认是 "running"。
+ `animation-fill-mode`	规定对象动画时间之外的状态。

#### 可以使用哪些属性做动画?
只要可以用数值描述且可以线性变换的属性，才可以用来制作动画，所以[可动画属性列表](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_animated_properties)是一个有限集合，且集合是变动的。

下面是一部分可以用来制动动画的属性：
+ 宽高：width height
+ 内边距：padding
+ 外边距：margin
+ 位置：left right top bottom
+ 边框：border border-radius
+ 颜色：color background-color
+ 字体大小：font-size
+ 变换：transform
+ 不透明度：opacity
+ 层叠序号：z-index

#### 动画样例

#### 水平移动
<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
@keyframes move {
    form {
        left: 10px;
    }
    to {
        left: 300px
    }
}
#el {
    animation: move 5s;
    position: absolute;
    top: 10px;
    left: 10px;
    width: 100px;
    height: 100px;
    background: red;
}</code></pre>
或者
<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
@keyframes move {
    form {
        transform: translateX(0px)
    }
    to {
        transform: translateX(300px)
    }
}
#el {
    animation: move 5s;
    width: 100px;
    height: 100px;
    background: red;
}</code></pre>

#### 水平翻转
<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
@keyframes reversal {
    form {
        transform: scaleX(1)
    }
    to {
        transform: scaleX(-1)
    }
}
#el {
    animation: reversal 5s;
    width: 100px;
    height: 100px;
    background: red;
    transform-origin: 100% 50%;
}</code></pre>

#### 旋转
<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
@keyframes rotate-animation {
    form {
        transform: rotate(0deg)
    }
    to {
        transform: rotate(180deg)
    }
}
#el {
    animation: rotate-animation 5s;
    width: 100px;
    height: 200px;
    background: red;
    border-radius: 50%;
    transform-origin: 200% 100%;
}</code></pre>

#### 水平缩放变形动画
<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
@keyframes scale-animation {
    0% {
        transform: scaleX(1);
    }
    50% {
        transform: scaleX(0);
    }
    100% {
        transform: scaleX(1);
    }
}
#el {
    animation: scale-animation 5s;
    width: 200px;
    height: 100px;
    background: red;
}</code></pre>

#### 背景色过渡
<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
@keyframes color-animation {
    0% {
        background: white;
    }
    50% {
        background: green;
    }
    100% {
        background: purple;
    }
}
#el {
    animation: color-animation 5s;
    width: 100px;
    height: 100px;
}</code></pre>

#### 文字大小变换
<pre><code>// HTML
&lt;div id="el"&gt;&lt;helloWord/div&gt;

// style
@keyframes size-animation {
    0% {
        font-size: 12px;
    }
    50% {
        font-size: 60px;
    }
    100% {
        font-size: 20px;
    }
}
#el {
    animation: size-animation 5s;
    width: 100px;
    height: 100px;
    font-size: 20px;
    text-align: center;
    line-height: 100px;
}</code></pre>
