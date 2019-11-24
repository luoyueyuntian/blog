事情起源于这么一个需求：当时我们地址编辑用到的省市区数据大概是如下这么一个树形结构：
<pre><code>[{
    provinceName: '广东省',
    provinceCode: '',
    cityList: [{
        cityName: '深圳市',
        cityCode: '',
        countyList: [{
            countyName: '南山区',
            countyCode: ''
        }]
    }]
}]</code></pre>
本来用着挺好的，突然发现有不同的市区countyCode是相同，这明显不符合我们对countyCode的定义。于是同时在群里统计目前已发现的重复区code，我看到这个后就想到了可以遍历整个数据，统计每个countyCode对应的countyName，然后把countyCode对应的countyName的数量大于1的筛选出来。

代码如下：
<pre><code>
</code></pre>

这个对于我们开发来说其实是一个很常规的需求，关键在于开发之外，大家有没有想过将自己编程技能应用到开发工作之外。