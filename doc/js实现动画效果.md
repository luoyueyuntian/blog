动画是快速循环播放一系列图像（其中每个图像与下一个图像略微不同）给人造成的一种幻觉。 大脑感觉这组图像是一个变化的场景。 
### 动画的原理
动画的原理是基于人眼的视觉暂留效果。医学证明人类具有“视觉暂留”的特性，人的眼睛看到一幅画或一个物体后，在0.34秒内不会消失，如果前后两个视像之间的时间间隔不超过0.34秒，那么前一个视像尚未消失，而后一个视像已经产生，并与前一个视像融合在一起，就会形成视觉暂留现象。利用这一原理，在一幅画还没有消失前播放下一幅画，就会给人造成一种流畅的视觉变化效果。

### 帧
帧就是影像动画中最小单位的单幅影像画面，一帧就是一副静止的画面。

帧里面又分为关键帧和过渡帧，这两概念是理解一些动画的基础。在一些场景中，我们可能不会给出一个动画的所有帧，所以将帧分成关键帧和过渡帧。关键帧可以理解为一个动画的起始状态或结束状态，而过渡帧则是系统自动完成插在关键帧之间的部分。过渡帧是一些状态可以线性变换的，比如左右位置、大小、透明度等，当我们设置了动画起始和结束状态的属性值，过渡帧可以计算出中间状态的属性值，并自动根据属性值的修改刷新页面，形成动画效果。

### 帧数与FPS
帧数，帧的数量。FPS（Frame per Second），即每秒显示帧数。

这两个概念，主要是FPS有什么作用呢？这是因为人眼生理构造的原因。人眼残留镜像的时间是有限的，如果过了这个时间，下一帧还没有变化，就会感觉不流畅。但也不是帧数越大越好，毕竟人眼也是有极限的，超过一定界限不仅不能提高观感，而且可能会带来性能压力。

### 缓动函数
缓动函数，顾名思义，就是决定动画效果运动快慢的函数。详细说就是定义不同曲线的数据公式，描述每一帧动画回调时需要改变的图形参数属性的幅度，从而达到均匀、先快后慢、先慢后快，甚至先超出起始值和结束值再慢慢恢复的各种动画特效。

### 动画类型
#### 逐帧动画
+ 在连续动画的相邻两帧中，画面一般仅有微小的变化
+ 每一帧都是关键帧

#### 渐变动画
+ 只在重要处定义关键帧
+ 渐变动画分“移动渐变”和“形状渐变”

### js创建动画
动画是一系列状态改变对应的视图改变而形成的动画。CSS3出来以前，主要是通过改变一个绝对定位元素的`left`和`top`来移动位置，，比如轮播图。通过修改一个元素的`display`来显示隐藏一个元素，比如弹窗。当然也可以通过定时去更好图片来创建逐帧动画。

##### 移动元素
<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
#el {
    position: absolute;
    top: 0;
    left: 0;
    width: 100px;
    height: 100px;
    background-color: #0099fff;
}

// script
var el = document.getElementById("el")
var moveLeft = function () {
    var left = parseInt(el.style.left || 0)
    if (left < 400) {
        el.style.left = (left + 20 < 400 ? left + 20 : 400) + 'px'
        setTimeout(moveLeft, 30)
    }
}
var moveRight = function () {
    var left = parseInt(el.style.left || 0)
    if (left > 10) {
        el.style.left = (left - 20 > 10 ? left - 20 : 10) + 'px'
        setTimeout(moveRight, 30)
    }
}
</code></pre>

##### 显示隐藏元素
<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
#el {
    width: 100px;
    height: 100px;
    background-color: #0099fff;
    display: block;
}

// script
var el = document.getElementById("el")
var show = function () {
    el.style.display = "block"
}
var hide = function () {
    el.style.display = "none"
}
</code></pre>

##### 元素展开收缩
<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
#el {
    width: 100px;
    height: 100px;
    background-color: #0099fff;
}

// script
var el = document.getElementById("el")
var maxHeight = 200
var show = function () {
    var animation = function () {
        var height = parseInt(el.style.height || maxHeight)
        if (height < maxHeight) {
            el.style.height = (height + 20 < maxHeight ? height + 20: maxHeight) + 'px'
            setTimeout(animation, 20)
        }
    }
    animation()
}
var hide = function () {
    var animation = function () {
        var height = parseInt(el.style.height || maxHeight)
        if (height > 0) {
            el.style.height = (height - 20 > 0 ? height - 20: 0) + 'px'
            setTimeout(animation, 20)
        }
    }
    animation()
}
</code></pre>

#### js修改CSS3属性创建动画

##### 元素渐隐和渐显（通过`opacity`）

<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
#el {
    width: 100px;
    height: 100px;
    background-color: #0099fff;
    opacity: 1;
}

// script
var el = document.getElementById("el")
var show = function () {
    var animation = function () {
        var opacity = el.style.opacity || 0
        if (opacity < 1) {
            el.style.opacity = opacity + 0.1 < 1 ? opacity + 0.1 : 1
            setTimeout(animation, 20)
        }
    }
    animation()
}
var hide = function () {
    var animation = function () {
        var opacity = el.style.opacity || 1
        if (opacity > 0) {
            el.style.opacity = opacity - 0.1 > 0 ? opacity - 0.1 : 0
            setTimeout(animation, 20)
        }
    }
    animation()
}
</code></pre>

##### 元素闪烁（通过`opacity`）

<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
#el {
    width: 100px;
    height: 100px;
    background-color: #0099fff;
    opacity: 1;
}

// script
var el = document.getElementById("el")
var flicker = function () {
    var opacity = el.style.opacity || 1
    el.style.opacity = opacity - 0.1 > 0 ? opacity - 0.1 : 1
    setTimeout(flicker, 20)
}
flicker()
</code></pre>

##### 翻页效果（通过`transform：translateY`）

<pre><code>// HTML
&lt;div id="el"&gt;&lt;/div&gt;

// style
#el {
    width: 100px;
    height: 100px;
    background-color: #0099fff;
    transform-origin: 100% 50%;
}

// script
var el = document.getElementById("el")
var reversal = function () {
    var status = 1
    var animation = function () {
        if (status > -1) {
            status = status - 0.1 > -1 ? status - 0.1 : -1
            el.style.transform = 'scaleX(' + status + ')';
            setTimeout(animation, 30)
        }
    }
    animation()
}
reversal()
</code></pre>
