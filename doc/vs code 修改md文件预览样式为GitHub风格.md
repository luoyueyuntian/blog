vs code 预览 Markdown 文件的快捷键为 Ctrl + Shift + V.


[VSCode预览markdown文件使用GitHub风格的样式表](https://github.com/raycon/vscode-markdown-css/blob/master/markdown-github.css)

将上面网址中的样式表文件拷出来，放到一个新建的githubStyle.css文件中

在VSC职工依次File --> Preferences --> Settings打开设置文件，子啊用户设置职工增加以下设置项，其中文件路径要根据实际路径修改。
<pre><code>{
    "markdown.styles": [
        "file:///F:/program films/Microsoft VS Code/resources/app/extensions/markdown/media/githubStyle.css"
    ]
}</code></pre>