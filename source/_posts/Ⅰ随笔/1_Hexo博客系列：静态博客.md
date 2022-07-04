---
title: Hexo博客系列：静态博客
categories: Ⅰ随笔
sort: 1
tags:
  - 博客
  - Hexo
date: 2022-06-28 09:47:10
updated: 2022-06-28 09:47:15
---

> 静态博客生成器，相较于其他生成器来说速度是最快的，文章对应使用Hexo版本为6.2.0

# Hexo安装

[Hexo](https://hexo.io/zh-cn/)静态博客生成器需要[Node.js](https://nodejs.org/en/)环境，官网建议使用`Node.js 12.0`及以上版本，同时需要安装[Git](https://git-scm.com/download/win)来进行代码管理

```
// 默认安装最新的版本
npm install hexo-cli -g
// 指定版本进行安装
npm install -g hexo@6.2.0
```

这里的`-g`表示全局安装（系统任何地方可使用），不建议局部安装（仅项目文件夹中可用），局部安装无法直接使用`hexo`命令，需要配置环境变量，较为复杂

<!-- more -->

# 相关命令

* 项目生成：`hexo init [name]`
* 本地启动：`hexo s`
* 本地打包：`hexo g`
* 清除打包：`hexo clean`
* 部署网站：`hexo d`
* 创建导航：`hexo n page xxx`

使用`hexo init`初始化文档项目时可能因为网络原因较为慢（国外地址），建议多尝试几次

# npm相关命令

* 查看本地Hexo版本：`npm ls hexo`
* 查看已发布的Hexo版本：`npm info hexo versions`
* 卸载Hexo：`npm uninstall -g hexo-cli`

# 网站信息配置

配置文件为目录下`_config.yml`，该文件的所有改动需要重新启动`hexo s`才会生效

```yml
# Site
title: Fan' Blog                  # 网站名称
subtitle: ''                      # 副标题
description: '因为热爱，所以追求'   # 描述
keywords: '编程'                   # 网站关键字
author: FanJun                     # 作者
language: zh-CN                    # 语言
timezone: ''
```

# 个人网址

```yml
url: https://fanmr.top
```

用于版权声明等情况

# 永久链接

```yml
permalink: /article/:year:month:day:hour:minute:second.html
```

默认的文章路径为`:year/:month/:day/:title/`，即文章发布时间加文章名字，如果文章的名字有所调整，那么文章原有的链接就无法访问该文章，对于收藏来说是不友好的

以上方式将文章的链接改为了文章发布时间，只要不对发布时间做调整，文章就可以被永久访问到

# 文章创建

文章放在`source/_posts`下，采用Markdown语法格式，可以随意创建文件夹，文章的完整格式如下：

```
---
title: 文章名
date: 2013-05-29 07:56:29     # 发表日期
updated: 2016-04-06 14:58:03  # 更新日期
categories: 随笔              # 文章分类
tags:                         # 标签，单个可直接指定
  - tag1
  - tag2
photos:                       # 文章头部展示图片，单个可直接指定
  - URL1
  - URL2
---

# 正文从这里开始
# 使用markdown语法

<!--more-->
以下内容不会展示在首页
```

# 本地图片

默认情况下发现本地图片是无法显示的，网上的解决方案都是使用`hexo-asset-image`插件，但是我发现`source`下非`_posts`的目录是被打包的，利用这个特性，即可实现本地图片的使用

在`source`下创建图片目录，如`images`，将图片放入其中，在引用的文章中直接使用Markdown的图片语法即可，如：

```
// 绝对路径（仅网站可用）
![](/imgs/20211227093201.png)
// 相对路径（不影响编辑器直接查询文章），VsCode会提示
![](../../imgs/20211227144740.png)
```

# 部署

需要先安装部署插件

```
npm install hexo-deployer-git
```

然后配置

```yml
# Deployment
deploy:
  type: git
  repo: https://github.com/fanmore/fanmore.github.io
  branch: main
```

最后使用`hexo d`就能将`public`中的文件推送到仓库了

# VsCode终端执行hexo报错

在VsCode的控制台执行`hexo`命令报错，出现以下信息：

```
hexo : 无法加载文件 C:\Users\Administrator\AppData\Roaming\npm\hexo.ps1，因为在此系统上禁止运行脚本有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_P 
olicies
所在位置 行:1 字符: 1
+ hexo -v
+ ~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

系统搜索`power shell`，以管理员身份运行，执行`set-ExecutionPolicy RemoteSigned`，输入`A`回车即可

# 分类文章排序方式

个人梳理了一下各个页面的排序方式，发现分类中的文章是按照时间倒序展示，有点别扭，感觉调整为时间正序排序比较好

调整方式，编辑`node_modules/hexo-generator-category/lib/generator.js`，将`-date`改为`date`即可

同理，如果在文章的头部设置了`sort: 99`，那么将`-date`改为`-sort`即为自定义的sort字段倒序

# 自动分类

文章多了发现手动维护分类是一个麻烦的事情，一旦碰到更改分类名的情况更是头痛，偶然发现一个自动分类插件，根据文件的目录进行分类名的维护

地址：https://github.com/xu-song/hexo-auto-category

```
npm install hexo-auto-category
```

在站点根目录下的`_config.yml`添加

```yml
auto_category:
 enable: true
 # 可指定目录深度
 depth:
```

**注意**：该插件在`hexo s`运行的情况下直接重命名文件夹偶尔会出问题，严重可能文章内容丢失！

# 自动分类的修复及升级

基于以上问题进行了修复和升级，实现了根据文件名自动维护文章名和排序号

例如这是我的结构目录，有顺序的文章管理，有逻辑和层次性

<img src="/images/1656470785705.png"/> 

然后需要实现的是文章中的标题根据文件名自动更改即可，而分类中的排序不再根据时间排序，而是我指定的排序方式，这样文章的排序更加有逻辑性

<img style="width:520px" src="/images/1656641427444.png"/>

在安装的插件`hexo-auto-category`中进行更改，目录为：`node_modules/hexo-auto-category/`

将`lib/logic.js`的内容更改为以下内容

```js
'use strict'

var front = require('hexo-front-matter')
var fs = require('hexo-fs')
debugger
let logic = function (data) {
    var log = this.log

    if (data.layout != 'post')
        return data
    if (!this.config.render_drafts && data.source.startsWith("_drafts/"))
        return data
    // 修复更改文件夹后没有内容会空写入情况
    if (data.content.length == 0)
        return data

    // 开始
    let postStr
    // 文章所有内容格式化
    var tmpPost = front.parse(data.raw)
    // 获取文章所在路径
    // _posts/Ⅰ随笔1/3_Hexo博客系列：高级配置.md
    var categories = data.source.split('/')
    // 分类名称
    var newCategories = categories[1]

    // 标题和排序号
    var titleStr = categories[2]
    var titleAndSort = titleStr.split('_')
    var title = titleAndSort[1].replace('.md', '')
    var sort = parseInt(titleAndSort[0])

    // 没有修改
    if (tmpPost.categories == newCategories && tmpPost.title == title && tmpPost.sort == sort)
        return data

    // 设置标题
    tmpPost.title = title
    // 设置排序号
    tmpPost.sort = sort
    // 设置分类
    tmpPost.categories = newCategories

    // 4. process post
    postStr = front.stringify(tmpPost)
    postStr = '---\n' + postStr
    fs.writeFile(data.full_source, postStr, 'utf-8')
    log.i("分类标题排序整理: categories [%s] for post [%s]", tmpPost.categories, categories[categories.length - 1])
    return data
}

module.exports = logic

```

更改后，配置文件不用再指定开启也不会报错，但是文章的目录结构需要跟上面一致才可以

然后更改分类中的文章排序方式，编辑`node_modules/hexo-generator-category/lib/generator.js`，将`-date`改为`sort`即可

这样更改后就能更加专注于文章，而不是网站的配置等问题，减少文章配置的维护

# VsCode写Mardown

使用`VsCode`进行`markdown`记录，其中使用了一些必要的插件：

* Markdown All in One：VsCode对`markdown`的语法支持
* Insert Date String：插入时间戳字符串，用在文章命名等（`Ctrl + Shift + i`）
* Markdown Image：直接粘贴图片到本地（或Githud图床），建议改为时间戳命名、html格式，便于定义图片宽度
* :emojisense:：使用`CTRL I`快捷添加emoji表情🤠
* Image preview：能直接预览图片，鼠标移动到图片上即可，还能定位图片位置和打开文件夹

同时使用`Snipaste`进行截图

# 关于图片

`Markdown Image`配置`Github`图床示例：

<img style="width:550px" src="/images/1656380130415.png"/>

`Markdown Image`会自动转换为CDN地址👍

Token在Github的`Settings` -> `Developer settings` -> `Personal access tokens`创建

<img style="width:600px" src="/images/1656310932634.png"/>

记得重启VsCode

# Github Pages自定义域名

域名的自定义在开启Pages的仓库中即可绑定，但是前提是添加进了Github的域名管理，在`Settings` -> `Pages`中根据提示添加domains并校验成功，即可使用

同时注意，绑定自定义的域名会在部署仓库下多一个`CNAME`文件，别忘了放入博客中