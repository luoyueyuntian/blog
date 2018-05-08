# GenerateSW 配置项说明
## 下面的配置影响webpack的汇编行为
### swDest
可选参数，字符串，默认值为 `'service-worker.js'`

该参数决定了的sw.js的输出目录

### importWorkboxFrom
可选参数，字符串，默认值为'cdn'

可选的值有'cdn', 'local', and 'disabled.

+ 'cdn'：使用谷歌的CDN

+ 'local'：使用本地的副本，构建时会自动在本地目录生成一份软件的副本，并引入该副本，不需要额外的操作

+ 'disabled'：插件不引入workbox库，需要自己在代码中手动引入，如果没用手动引入，则会报错

### chunks
可选参数，字符串组成的数组，默认[]

默认情况下，插件会预加载列表中插入所有的模块，该配置可以设置仅配置中模块输出到预加载列表中，类似于白名单

### excludeChunks
可选参数，字符串组成的数组，默认[]

与chunks配置项对应，类似于黑名单，配置中的模块会被跳过，不会添加到预加载模块

### include
可选参数，字符串或正则表达式组成的数组

提供一系列规则，符合该规则的模块则会添加到预加载列表

### exclude
可选参数，字符串或正则表达式组成的数组

提供一系列规则，符合该规则的模块则不会添加到预加载列表

### importsDirectory
可选参数，字符串，默认''

插件默认从根目录开始遍历，并将文件添加到预加载列表中，该配置项可设置为仅遍历子目录

### precacheManifestFilename
可选参数，字符串，默认为'precache-manifest.[manifestHash].js'

修改预加载文件列表文件的名称

## 下面的配置与webpack的汇编行为无关

### importWorkboxFrom
同上

### skipWaiting
可选参数，布尔值，默认false

是否跳过等待阶段，资源更新后不会立即激活，配合`clientsClaim: true`可以实现即时更新

### clientsClaim
可选参数，布尔值，默认false

### runtimeCaching
可选参数，数组或对象

设置运行中的请求如何缓存，通过一个或多个包含urlPatterns, handlers, 和潜在的options 来配置插件生成相应的sw.js来处理运行中的缓存文件。
通过globPatterns获取的预存URL的请求默认处理，并且不需要在runtimeCaching中容纳。

handler的值为workbox.strategies支持的策略名字的字符串

options可用于配置指定路由的缓存过期，可缓存响应和广播缓存更新插件的实例。

### navigateFallback
可选参数，字符串，默认undefined

这将用于创建一个路由导航，用来响应未预先缓存的URL的导航请求。

这是为了在单页面应用场景中使用，所有导航都使用公用的App Shell HTML。

它不打算用作浏览器脱机时显示的回退。

### navigateFallbackBlacklist
可选参数，由正则组成的数组，默认为[]

用来限制配置的navigateFallback行为适用于哪些URL。

如果网站的一部分网址应视为单页应用程序的一部分，则此功能非常有用。

如果配置了navigateFallbackBlacklist和navigateFallbackWhitelist，则navigateFallbackBlacklist会优先。

### navigateFallbackWhitelist
可选参数，由正则组成的数组，默认为[]

用来限制配置的navigateFallback行为适用于哪些URL。

如果网站的一部分网址应视为单页应用程序的一部分，则此功能非常有用。

如果配置了navigateFallbackBlacklist和navigateFallbackWhitelist，则navigateFallbackBlacklist会优先。

### importScripts
可选参数，字符串组成的数组

如果需要在项目之外额外引入并预加载一些代码，可通过该配置，在最终生成的sw.js中会预加载该文件

### ignoreUrlParametersMatching
可选参数，正则组成的数组，默认为 [/^utm_/]

在查找一个请求是否已经缓存时，可通过该配置忽略某些请求参数


### directoryIndex
可选的字符串，默认index.html

当一个导航请求'/'结尾，并且在预缓存中无匹配结果时，会将该参数的值追加到路由末尾然后在预缓存URL中重新匹配

### cacheId
可选的字符串，默认为null

如果配置了该参数，workbox在生成缓存时会在缓存名称中以该配置为前缀

该配置在本地开发中比较有用，因为本地开发环境的域名都是http://localhost:port 

### globDirectory
可选的字符串，默认为undefined

设置匹配globPatterns的基准目录，该值应为相对当前工作目录的相对路径。

设置该参数后，同时要配置globPatterns。

### globFollow
可选的布尔值，默认为true

确定在生成预先缓存清单时是否遵循符号链接。


### globIgnores
可选的字符串组成的数据, 默认为['node_modules/**/*']

一系列文件匹配模式，符合要求的文件会被跳过不放到预缓存文件清单中

### globPatterns
可选的数组或字符串, 默认值为 ['**/*.{js,css,html}'] (for workbox-build and workbox-cli) or [] (for workbox-webpack-plugin)

任何匹配这些模式的文件将包含在预缓存清单中。

注：使用workbox-webpack-plugin时，通常不需要设置globPatterns，默认情况下它会自动预先清除属于webpack生成管道一部分的文件。 使用webpack插件时，只需在需要缓存非webpack资源时进行设置。

### globStrict
可选的布尔值，默认为true

如果为true，在生成预加载文件清单时如果遇到读取文件目录失败,则整个编译过程会失败；如果为false，则跳过这些错误


### templatedUrls
可选的字符串组成的对象或数组,默认值为null

如果一个URL的生成是基于服务端逻辑，其内容可能取决于多个文件或某个其他唯一字符串值。

如果与字符串数组一起使用，它们将被解释为glob模式，并且与模式匹配的任何文件的内容将用于唯一地版本化URL。

如果使用单个字符串，它将被解释为您为给定URL带外生成的唯一版本信息。

### maximumFileSizeToCacheInBytes
可选的数字，默认值为2097152

此值可用于确定将被预缓存的文件的最大大小。 这样可以防止无意中预读非常大的文件，这些文件可能会意外地与某个模式相匹配。

### dontCacheBustUrlsMatching
可选的正则，默认为null

与此正则表达式匹配的资源将被假定为通过它们的URL进行唯一版本化，并免除正常填充缓存时执行的HTTP缓存破坏。

虽然不是必需的，但建议如果您的现有构建过程已经为每个文件名插入了一个[hash]值，则会提供一个RegExp来检测这些值，因为它将减少预缓存时消耗的带宽量。

### modifyUrlPrefix
可选的字符串组成的对象，默认为null

前缀映射（如果存在于预先缓存清单中的条目中）将替换为相应的值。

例如，如果您的网站托管设置与您的本地文件系统设置不匹配，则可以使用此选项从清单条目中删除或添加路径前缀。

作为具有更大灵活性的替代方案，您可以使用manifestTransforms选项并提供一个函数，使用您提供的任何逻辑修改清单中的条目。

### manifestTransforms
可选的ManifestTransform组成的数组

一个或多个ManifestTransform函数，它们将按顺序应用于生成的清单。

如果还指定了modifyUrlPrefix或dontCacheBustUrlsMatching，则将首先应用它们的相应转换。
