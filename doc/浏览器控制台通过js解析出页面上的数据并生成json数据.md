最近在做面试作品，目标是用最热的angular/vue/react做小米商城，小米的导航内容有点多，我需要把里面的数据一个一个拷出来，写到json文件里面，然后用模板来生成导航的内容，这样就可以利用框架的优势来提高开发效率。刚开始觉得手动拷没多大问题，也没想过能不能用前端的知识来更快更准确的把数据拿出来。

平时有需要，也会在浏览器控制台用js去做一些逻辑的处理，比如在外包公司的时候，收藏了很多网址，因为公司网络限制，这些网址没法通过邮箱等方式发回家里，后来发现公司里面可以登GitHub，在GitHub上有可以在线生成文件的功能，我就想把浏览器收藏夹导出来，然后把里面的网址解析出来，保存的GitHub上，回家就可以下载下来，然后在通过收藏的自己的电脑上。在和公司确认把非保密信息传到外网不算违规后，我就决定这样做。浏览器导出的收藏夹是一个HTML文件，里面记录了所有收藏的内容，所有的网址都保存在了a标签中，我通过浏览器控制台，可以很轻松的获取所有的a标签，然后通过for循环，把a标签的text和href属性提取出来，以一个包含name和href属性的对象保存到一个数组里。然后适用JSON.stringify，就可以把数据以json的形式打印到控制台上，然后把数据拷出来，放到一个新建的json文件里面就OK了。后来我一想，我在GitHub上一般倒是保存的md文件，那我为什么不直接生成md文件格式的字符串呢？于是我又做了下处理，对取到的json数组做了一个遍历，md文件一行的内容大概是
<pre><code>[${name}](${url})\n\n</code></pre>
通过定义一个字符串，把所有的内容串起来，打印到控制台，就可以得到一个换好行，直接可以保存到md文件里面的字符串了。

对于获取小米导航里面的数据，我觉得也可以采用这种方式，于是就写了如下代码，在控制台运行，结果确实是我想要的。
<pre><code>
var menuContainer = $("#J_categoryList>li");
var data = [];
menuContainer.each(function(i, dom) {
    var name = $(dom).children("a.title").text();
    var productArr = getChildrenData($(dom).find("div.children"));
    data.push({
        productName: name,
        productArr: productArr
    });
});

function getChildrenData(containerEle) {
    var result = [];
    containerEle.find("ul").each(function(i, dom) {
        var $liArr = $(dom).find("li");
        var length = $liArr.length;
        var i;
        for (i = 0; i < length; i++) {
            var idom = $liArr[i];
            var name = $(idom).find(".link .text").text();
            var isVarity = $(idom).hasClass("star-goods");
            var imgUrl = $(idom).find("img.thumb").attr("src");
            result.push({
                productName: name,
                imgSrc: "./assert/img/" + getImgName(imgUrl),
                isVarity: isVarity
            });
        }
    });
    return result;
}

function getImgName(imgUrl) {
    var startPos = imgUrl.lastIndexOf("/") + 1;
    var lastPos = imgUrl.lastIndexOf("?");
    if (lastPos === -1) {
        lastPos = imgUrl.length;
    }
    return imgUrl.substring(startPos, lastPos);
}
console.log(JSON.stringify(data));
</code></pre>
上面这段代码当然也不是一次就写好的，中间也调试了几次，毕竟也不经常这样做，不是太熟。不过作为使用行业知识提高做事情的效率，这个还是值得推广的。

接着上面，上面我实现了从网站上解析出网站上的数据，更进一步，网站上的图片我也是需要下载到本地的，js也是可以实现这个功能的，代码如下：
<pre><code>var imgs = [];
var menuContainer = $("#J_categoryList>li");
menuContainer.each(function(i, dom) {
    resolveImgSrc($(dom).find("div.children"));
})

function resolveImgSrc(containerEle) {
    containerEle.find("ul").each(function(i, dom) {
        var $liArr = $(dom).find("li");
        var length = $liArr.length;
        var i;
        for (i = 0; i < length; i++) {
            var idom = $liArr[i];
            var imgUrl = $(idom).find("img.thumb").attr("src");
            imgs.push(imgUrl);
        }
    });
}

function unique4(array) {
    array.sort();
    var re = [array[0]];
    for (var i = 1; i < array.length; i++) {
        if (array[i] !== re[re.length - 1]) {
            re.push(array[i]);
        }
    }
    return re;
}
unique4(imgs).map(function(i) {
    var a = document.createElement('a');
    a.setAttribute('download', '');
    a.href = i;
    document.body.appendChild(a);
    a.click();
})
</code></pre>

运行上述代码之前，要先把浏览器的默认下载目录改到你想下载到的目录，并确定不是每次下载都询问你保存位置。

运行上面代码时浏览器由于安全策略，会让你确认是否允许同时下载多个文件，点击确认即可。

如果遇到下载文件数量不对的情况，可以先在网页上把导航全部内容浏览一遍，这样确保页面上的内容是完整的，避免部分懒加载的内容还没有加载，页面数据不全。