## 目录

  - 页面单词
  - **搜索查询语法**
    - 单个单词single word
    - 多个单词multiple word
    - 短语phrase
    - 前缀prefix
    - 转换为基本搜索switch to basic search
    - 通配符搜索wildcard search
  - **搜索过滤器：**
    - 反向搜索
    - 条件搜索any
    - 标题搜索title
    - 主体搜索body
    - 标签搜索tag
    - 笔记本搜索notebook
    - 创建时间搜索created
    - 更新时间搜索updated
    - 期限搜索due
    - 种类搜索type
    - 状态搜索iscompleted
    - 位置(纬度)搜索latitude
    - 位置(经度)搜索longitude
    - 位置(纬度)搜索altitude
    - 附件类型搜索resource
    - 网址(URL)搜索sourceurl
    - 笔记ID搜索
  - 快捷键
    - 页面内搜索
    - 全局搜索
  - 问题解决
    - 重启后，笔记存储路径变回默认导致设置和笔记全部消失
    - 代码块内，编辑页面和渲染页面缩进不同，回车键自动加缩进的解决方法
    - 使用OneDrive导致两个PC之间无法同步目录的问题
    - 可以使用OneDrive
    - joplin冲突
    - 重置OneDrive

## 页面单词

sidebar   s.工具栏；边栏

note list   s.笔记列表

note editor   s.笔记编辑器

rich text editor   s.富文本编辑器

markdown editor   s.markdown文本编辑器

toggle editor   s.对页面的编辑器进行切换

external editor   s.外部编辑器

tags   s.标签

synchronising   s.同步

web clipper   s.网页剪藏

attachments   s.附件，在joplin中表现为链接到指定文件的链接

queries   s.查询

wildcard search   s.通配符搜索

todos   s.待办事项（在joplin中是与"笔记"同级的一种文体）

filter   s.过滤器

confirm   s.确定；确认

## **搜索查询语法**

### 单个单词single word

例如，搜索："cat"会显示包含"this is a cat"的笔记。不会显示包含"cataclysmic"的笔记。

### 多个单词multiple word

例如，搜索："dog cat"会显示所有同时包含"dog"和"cat"的笔记，且无论这两个单词存在的位置。不会显示只包含"dog"或者"cat"的笔记

### 短语phrase

在一个短语的两边加上双引号。例如，搜索：" "dog cat list" "只会显示包含"dog cat list"的笔记。不会显示同时拥有"dog""cat""list"但顺序不是依照短语的要求的笔记。

### 前缀prefix

在一个单词的前缀部分后加上符号" * "。例如，搜索："swim*"会显示所有包含"swim""swimming""swims"的笔记。注意符号"*"在单词开头将会被忽略，而在单词中间则会被认为是单词的组成部分。

### 转换为基本搜索switch to basic search

在需要进行搜索的特定语句前加上符号"/"，然后在特定语句两边加上双引号。这种搜索方法一般会比较慢，但是能搜索特殊的符号。例如，搜索："/"文本内容" "，会显示所有包含"文本内容"的笔记。

### 通配符搜索wildcard search

通配符是一种特殊语句，主要有星号(*)和问号(?)，用来模糊搜索文件。 当查找文件夹时，可以使用它来代替一个或多个真正字符。当不知道真正字符或者懒得输入完整名字时，常常使用通配符代替一个或多个真正的字符。例如搜索："be*ful"，会显示包含"belful"或者"beautiful"的笔记。

## **搜索过滤器：**

### 反向搜索

在一个单词，或者在一个区域搜索限定词前（例如tag:）加上符号"-"。会显示不包含此单词的笔记，可在多个单词搜索下增加限制（带A不带B）。例如，搜索："-dog"会显示所有不包含"dog"的笔记，但是会显示包含"dogcat"的笔记。

### 条件搜索any

例如，搜索："any:0 dog cat"，将会显示包含了"dog"和"cat"的笔记。搜索："any:1 dog cat"，将会显示包含了"dog"或者"cat"的笔记

### 标题搜索title

例如，搜索："title:dog"，将会显示仅在标题(title)部分中包含了"dog"的笔记。

### 主体搜索body

例如，搜索："body:dog"，将会显示仅在正文(body)部分中包含了"dog"的笔记。

### 标签搜索tag

例如，搜索："tag:dog"，将会显示标签(tag)包含了"dog"的笔记。

### 笔记本搜索notebook

例如，搜索："notebook:dog"，将会只能显示"dog"笔记本下的所有子笔记本。

### 创建时间搜索created

例如搜索："created:20201218"，将会只能显示在2020年12月18日当天及以后创建的笔记。具体的月和日可以省略。可以使用"-"来转变为"当天及以前"。

### 更新时间搜索updated

例如搜索："updated:20201218"，将会只能显示在2020年12月18日当天及以前得到更新的笔记。具体的月和日可以省略。可以使用"-"来转变为"当天及以后"。例如搜索"updated:day-2"，将会只能显示当天和前两天之间的时间内得到更新的笔记。

### 期限搜索due

例如搜索："due:day+7"，将会显示所有将会在当天后的7日期间截止的待办事项。例如搜索"due:day-7"，将会显示所有到当天为止已经截止超过7天的待办事项。

### 种类搜索type

例如搜索"type:note"，将会只能显示所有笔记。例如搜索："type:todo"，将会只能显示所有待办事项。

### 状态搜索iscompleted

例如搜索："iscompleted:1"，将会只能显示所有已完成待办事项。例如搜索"iscompleted:0"，将会只能显示所有未完成待办事项。

### 位置(纬度)搜索latitude

例如搜索："latitude:40"，将会只能显示latitude大于等于40的笔记。

### 位置(经度)搜索longitude

例如搜索："longitude:40"，将会只能显示longitude大于等于40的笔记。

### 位置(纬度)搜索altitude

例如搜索："altitude:40"，将会只能显示altitude大于等于40的笔记。

### 附件类型搜索resource

例如搜索："resource:image/jpeg"，将会显示包含"jpeg"格式的附件的笔记。例如搜索："-resource:application/pdf"，将会显示不包含"pdf"格式的附件的笔记。例如搜索："resource:image"，将会显示包含任意格式的图像(image)的附件的笔记。*

### 网址(URL)搜索sourceurl

例如搜索："sourceurl:https://www.google.com"，将会显示包含跳转到此网址超链接的笔记。

### 笔记ID搜索

例如搜索："id:9cbc1b4f242043a9b8a50627508bccd5"，将会显示笔记ID为9cbc1b4f242043a9b8a50627508bccd5的笔记，每个笔记有自己的ID号。

## 快捷键

### 页面内搜索

**ctrl+F**

页面内搜索关键词，同windows的ctrl+F的效果。

### 全局搜索

**ctrl+P**

开启全局搜索窗口。(简单搜索关键词时，比普通搜索栏更值得推荐)

## 问题解决

### 重启后，笔记存储路径变回默认导致设置和笔记全部消失

把先前的快捷方式删除，新建快捷方式，在"目标"一栏更换为：```D:\Joplin\Joplin.exe --profile "E:\个人\奥丁\材料和文件\Joplin笔记存储"```（此处第一个路径为Joplin安装的路径，第二个路径为自定义的Joplin笔记存储的路径）然后使用任务管理器彻底关闭Joplin。然后使用新的快捷方式打开Joplin即可。

注意语法为：```Joplin可执行文件绝对路径 --profile "设置和笔记存储位置根目录绝对路径"```

### 代码块内，编辑页面和渲染页面缩进不同，回车键自动加缩进的解决方法

可以直接鼠标点击下一个空行的开头来输入新内容，来消除上一行代码缩进的影响。如果下一行没有空行则在```` ``` ````处按回车键建立新的空行。

在代码块内输入两次`Tab`，可以达成类似`\t`的统一缩进间隔的效果。常用于代码注释的排版。

### 使用OneDrive导致两个PC之间无法同步目录的问题

不要使用OneDrive，同步会有一定的问题。使用第三方网盘或者自己搭建NAS来支持WebDAV，使用WebDAV来进行网盘同步可以正常同步目录结构和内容。

### 可以使用OneDrive

最稳妥的方法，开启Windows10自带的OneDrive服务，然后在文件管理器页面创建一个用来进行笔记同步的文件夹，joplin里使用file system填写文件管理器的OneDrive下此文件夹的绝对路径，那么经过OneDrive自动同步后，两台登录同一个OneDrive账号的电脑文件管理器下同时可以看到此文件夹，然后待同步的电脑的joplin同样填上这台电脑OneDrive下此文件夹的绝对路径，然后删除本地数据同步云端数据即可。

file system 同步的本质：将joplin笔记上传到本机OneDrive盘（实际上是C盘本地空间），OneDrive将本机内容哦同步到OneDrive云盘，然后另一台电脑OneDrive盘将OneDrive云盘的内容同步到C盘本地空间，然后在同步到另一台电脑的joplin上。

注意因为需要使用VPN登录OneDrive官网，可能会使得Windows自动打开代理服务器的选项，在"设置"-"代理服务器设置"-"手动设置代理"，关掉代理才能正常使用正常网络上网，不然即使连接上wifi没网的。

登录OneDrive仅需要有网，打开OneDrive在线官网需要有VPN且Windows手动代理设置已打开。同步OneDrive可以仅有网，但更推荐VPN且打开Windows手动代理设置。

完全卸载joplin除了使用geek卸载之外，因为joplin的本地笔记文件和配置文件都不会删除，所以需要提前找到路径手动删除。

OneDrive同步会不断自动删除云端OneDrive的json日志文件，此时需要通过修改注册表禁止OneDrive同步某种文件类型（因为其他文件类型也是有可能的）（但目前疑似没用，未知原因），网页SharePoint需要内部管理员权限。

可以在OneDrive某个文件直接查看版本历史记录，推测同步到哪些东西了。

“删除本地数据并从同步目标导入数据”，如果是file system的同步方式的话仅仅几分钟就可以了（因为是本地）。

最终还是不行，能够同步大部分笔记，但新改动的同步非常缓慢然后由于各种原因卡住。

最后决定，在轻薄本仅安装joplin软件并不设置同步，需要去上课的情况才拷贝需要的特定的笔记（U盘）到轻薄本上，而平常宿舍和图书馆等地使用游戏本的本体笔记。

### joplin冲突

joplin冲突：在两台不同的设备上，对内容进行了不同且矛盾的更改（如不矛盾则是合并更改）并且同时进行同步，使得一份相同的内容同时进行了矛盾的更改而没有先后顺序，因此joplin会将本地更改后的内容划入"冲突"文件夹，同时另一台设备也会保留自己的本地更改后的内容副本。

### 重置OneDrive

使用 \reset 命令对两台电脑上的OneDrive均进行重置，可以脱离"正在更新x个改动"然后数字不断跳动卡住的情况。然后先把两个电脑的本地的OneDrive文件转移到其他地方进行备份，然后把两个电脑本的OneDrive文件全部删除，然后同步到云端OneDrive全部文件也同步删除，然后joplin同步file system找不到文件夹会自己创建一个新的，然后自动开始上传，同时OneDrive也自动开始同步上传的joplin内容。