推荐程 [廖雪峰的官方网站-Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137396287703354d8c6c01c904c7d9ff056ae23da865a000)
# 安装
从[https://git-for-windows.github.io/](https://git-for-windows.github.io/)下载安装包，按默认选项安装。
# 基本设置
开始菜单里找到“Git”->“Git Bash”，打开后弹出一个命令行输入工具，运行以下命令，设置用户名和邮箱
<pre><code>$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"</code></pre>
git config命令的--global参数，表示你这台机器上所有的Git仓库都会使用这个配置
# 创建版本库
找到一个空文件夹，路径最好不要带中文，执行命令
<pre><code>git init</code></pre>
当前目录下多了一个.git的目录（没有看到.git目录，那是因为这个目录默认是隐藏的），表示创建版本成功。
.git目录是Git来跟踪管理版本库的，不要手动修改里面的文件，以免破换git库。
# 添加要跟踪的文件
将文件添加到版本库，意味着git会跟踪文件的修改
<pre><code>git add filename</code></pre>
# 忽略文件
版本库里面有些文件是不需要跟踪的，需要配置忽略，以免每次都提示一大推文件已修改，影响工作效率
在仓库目录下新建一个名为.gitignore的文件，配置忽略规则
可以是具体的文件路径，也可以是配置一类目录、文件的规则
> 配置语法：
以斜杠“/”开头表示目录；
以星号“*”通配多个字符；
以问号“?”通配单个字符
以方括号“[]”包含单个字符的匹配列表；
以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；
# 提交修改
提交文件到仓库，提交时最好带上修改说明，方便后面查找修改记录
<pre><code>git commit -m '修改说明'</code></pre>
# 查看提交日志
<pre><code>git log</code></pre>
加上--pretty=oneline参数，可以设置显示的记录过长时不换行，方便查看
# 版本回退
HEAD表示当前版本，上一个版本是HEAD^，上上一个版本是HEAD^^，往上100个版本可以写HEAD~100
<pre><code>git reset --hard HEAD^</code></pre>
每次执行都可以得到版本号，知道commit id，可以直接回退到指定版本
<pre><code>git git reset --hard 3628164</code></pre>
# 怎样理解工作区和暂存区（重要概念）
+ 工作区（Working Directory）：我们正在编辑的文件目录
+ 版本库（Repository）：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。<br/>
![Alt 工作区与版本库](../assert/img/git.jpg)（注：从廖雪峰的网站上直接拿过来，感觉讲的比较清楚）<br/>
### 把文件往Git版本库里添加的时候，是分两步执行的：
第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；<br/>
![Alt 工作区与版本库](../assert/img/git-add.jpg)（注：从廖雪峰的网站上直接拿过来，感觉讲的比较清楚）<br/>
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。<br/>
![Alt 工作区与版本库](../assert/img/git-commit.jpg)（注：从廖雪峰的网站上直接拿过来，感觉讲的比较清楚）<br/>
*理解了暂存区，就要注意，git commit是将暂存区的修改提交到本地版本库，如果之前添加到版本跟踪文件，没有提交到暂存区的文件，是不会提价到本地版本库的*
