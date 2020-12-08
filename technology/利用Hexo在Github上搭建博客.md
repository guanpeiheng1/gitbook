---
title: 利用Hexo在Github上搭建博客
categories: Technology
abbrlink: Building-Blog-With-Hexo
date: 2019-06-04 09:50:29
tags:
---

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g9ari29zqgj31cq0u0gva.jpg)

利用Github个人主页和Hexo，可轻松搭建出精美的个人博客。

<!-- more -->

# 搭建博客

```
// 利用gitbook搭建博客
通过github账号登录gitbook，新建workspace
https://www.gitbook.com/
在github账号新建一个repository，假设为gitbook
在工作区中Integrations-github，连接github账号，选择上面的repo
为实现本地push一个repo时同步更新gitbook，选择Github to Gitbook

连接后按照基本的github操作，本地连接远程的repo:
cd /Volumes/Payton\'s
git clone git@github.com:guanpeiheng1/gitbook.git
cd /Volumes/Payton\'s/gitbook
git init
git remote add origin git@github.com:guanpeiheng1/gitbook.git
git pull
git add .
git commit -m "..."
git push origin master

目录结构：
文件夹为group
文件夹内每一个md为一个页面


```

## 搭建前的准备

需已安装`node`和`git`，且git已连接到github。

## 创建Github个人主页

登录网页版Github，点击`New`以新建仓库，仓库名必须为`用户名.github.io`。点击`Create new file`，文件为`index.html`，内容为`Hello Github Pages`。复制仓库名，在浏览器中访问，若看到刚才输入的内容，则创建成功。

## 搭建Hexo环境

命令行/终端切换到想安装的目录（此处为E盘），执行以下安装命令。

```
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo s
```

在网页输入`localhost:4000`预览，若出现博客样式，则搭建成功。

## 将Hexo博客部署到Github个人主页上

在建好的Github仓库中点击`Clone or download`，复制仓库地址。打开`blog/_config.yml`，在末尾添加（或修改）如下信息，注意空格和缩进。

```
deploy:
  type: git
  repo: // 你的Github个人主页地址
  branch: master
```

命令行/终端路径切换到刚才的blog目录，输入以下命令完成部署。

```
npm install hexo-deployer-git –save
hexo g
hexo d
```

## 备份Hexo源文件到Github

输入以下命令安装备份插件。

```
npm install hexo-git-backup --save
```

在`_config.yml`文件末尾添加以下内容。

```
# Backup
backup:
  type: git     
  message: backup for hexo blog
  repository:  
    github: git@github.com:paytonguan/paytonguan.github.io.git,source
```

保存后输入以下命令以备份。

```
hexo b
```

如果连接不上仓库，则删除`.git`文件夹，重试即可。

## Hexo使用

### 常用命令

| 命令             | 操作                                 |
| ---------------- | ------------------------------------ |
| hexo s           | 启动服务预览                         |
| hexo g           | 生成静态页面                         |
| hexo d           | 部署                                 |
| hexo new a       | 新建文章a（位于blog/source/_posts）  |
| hexo new draft b | 新建草稿b（位于blog/source/_drafts） |
| hexo publish b   | 发布草稿b                            |
| hexo new page c  | 新建页面c（位于blog/source）         |

### 全局配置

Hexo的配置文件位于`blog/_config.yml`中，修改其中配置即可实现Hexo的不同功能。

#### 杂项

```
title: Payton's blog
subtitle: Somewhere leisure.
description:
keywords:
author: Payton
language: zh-CN
timezone: Asia/Shanghai
```

#### 默认新建页面

调整`hexo new a`所新建的页面a的位置。

```
default_layout: post //默认页面新建成post格式
default_layout: draft //默认页面新建成draft格式
```

### 给文章添加分类

终端进入blog目录，输入以下命令新建页面。

```
hexo new page categories
```

编辑刚新建的页面，如下。

```
title: Categories
date: 2014-12-22 12:39:04
type: "categories"
---
```

编辑主题的_config.yml，将menu中的`categories: /categories`注释去掉，然后在文章md文件头部新增`categories:`即可。

### 文章设置多标签

在文章md文件头部新增`tags: [标签1,标签2,···]`即可。

# Next主题安装与配置

## 安装

### theme安装

命令行/终端路径切换到blog目录，输入以下命令下载Next主题。

```
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

打开`blog/_config.yml`，将`theme:`更改为`next`。再次输入`hexo s`，若主题更改，则安装成功。

### theme依赖安装

#### hexo-symbols-count-time

在终端输入以下命令。

```
npm install hexo-symbols-count-time
```

在根目录的`_config.yml`文件末尾添加以下内容即可。若无效，则执行`hexo clean`，然后再生成。

```
symbols_count_time:
  symbols: true
  time: true
  total_symbols: true
  total_time: true
  exclude_codeblock: false
  awl: 4
  wpm: 275
  suffix: "mins."
```

项目链接如下。

```
https://github.com/theme-next/hexo-symbols-count-time
```

#### pangu

在终端输入以下命令即可。需在配置文件中设置`pangu: true`。

```
npm install pangu --save
```

项目链接如下。

```
https://github.com/vinta/pangu.js
```

## 配置

Next主题的配置文件位于`blog/themes/next/config.yml`中，自定义样式文件位于`blog/themes/next/source/css/custom/custom.styl`中。修改其中配置即可实现不同的页面效果。

### 更改底部ICON

将icon的name改为`fa fa-comments`，color改为`#808080`即可。

### 更改主题样式

将Scheme更改为Pisces即可。

### 关闭脚注

将footer下的powered改为false即可。

### 添加社交链接

在social下取消注释要添加的链接，然后将social_icons的设置都设为true即可。

### 左侧菜单添加图标

配置文件中有关的配置如下。

```
avatar:
  url: /images/avatar.gif
  rounded: true
  rotated: false
```

### 行内代码块自定义样式

在`blog/themes/next/source/css/_custom/custom.styl`添加以下样式。

```
code {
    color: #DC143C;
    margin: 2px;
    border: 1px solid #d6d6d6;
}
```

### 添加背景图片

在`blog/themes/next/source/images`下放入背景图片并重命名为`background.jpg`，在`blog/themes/next/source/css/_custom/custom.styl`添加以下样式。

```
@media screen and (min-width:1200px) {

    body {
    background-image:url(/images/background.jpg);
    background-repeat: no-repeat;
    background-attachment:fixed;
    background-position:50% 50%; 
    }

    #footer a {
        color:#eee;
    }
}
```

### 在文章末尾添加「本文结束」标记

在路径`/themes/next/layout\_macro`中新建`passage-end-tag.swig`文件，并添加以下内容。

```
<div>
  {% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:14px;">-------------本文结束<i class="fa fa-paw"></i>感谢您的阅读-------------</div>
  {% endif %}
</div>
```

接着打开`/themes/next/layout\_macro/post.swig`，在`post-body`之后、`post-footer`之前的`</div>`添加如下代码。

```
<div>
 {% if not is_index %}
  {% include 'passage-end-tag.swig' %}
 {% endif %}
</div>
```

然后打开主题配置文件`_config.yml`，在末尾添加如下代码即可。

```
// 文章末尾添加「本文结束」标记
passage_end_tag:
	enabled: true
```

### 添加Mathjax公式支持

在主题配置文件`_config.yml`中更改以下设置，打开math中的Mathjax功能（注意不能与Katex一起使用）。

```
math:
  mathjax:
    enable: true
    mhchem: true
```

在需要使用Mathjax的文章在头部新增`mathjax: true`即可。如果没有效果，则在终端输入以下代码。

```
// 卸载自带markdown渲染器
npm un hexo-renderer-marked --save

// 安装支持mathjax的渲染器
npm i hexo-renderer-kramed --save
```

### 文章链接添加html后缀

在终端输入以下命令安装hexo-abbrlink插件。

```
npm install hexo-abbrlink --save
```

在Hexo根目录的配置文件`_config.yml`中修改/添加以下信息。

```
permalink: posts/:abbrlink.html

#abbrlinks
abbrink:
  alg: crc32
  rep: dec
```

文章md文件头部新增`abbrlink: xxx`，即可在渲染时得到xxx.html，否则将按照算法随机生成数字。

除使用`:abbrlink`变量外，也可以使用以下属性。

| 变量      | 描述                                                   |
| :-------- | :----------------------------------------------------- |
| :year     | 文章的发表年份（4位数）                                |
| :month    | 文章的发表月份（2位数）                                |
| :i_month  | 文章的发表月份（去掉开头的零）                         |
| :day      | 文章的发表日期（2位数）                                |
| :i_day    | 文章的发表日期（去掉开头的零）                         |
| :title    | 文件名称                                               |
| :id       | 文章ID                                                 |
| :category | 分类（如果文章没有分类，则是default_category配置信息） |

可在 `permalink_defaults` 参数下调整永久链接中各变量的默认值。

```
permalink_defaults:
  lang: en
```

### 添加宠物/动漫人物插件

打开终端，切换到博客根目录并输入以下命令以安装。

```bash
npm install --save hexo-helper-live2d
```

打开主题文件夹的_config.yml文件并添加以下内容到文件末尾。

```
# Live2D
## https://github.com/EYHN/hexo-helper-live2d
live2d:
  enable: true
  # enable: false
  scriptFrom: local # 默认
  pluginRootPath: live2dw/ # 插件在站点上的根目录(相对路径)
  pluginJsPath: lib/ # 脚本文件相对与插件根目录路径
  pluginModelPath: assets/ # 模型文件相对与插件根目录路径
  # scriptFrom: jsdelivr # jsdelivr CDN
  # scriptFrom: unpkg # unpkg CDN
  # scriptFrom: https://cdn.jsdelivr.net/npm/live2d-widget@3.x/lib/L2Dwidget.min.js # 你的自定义 url
  tagMode: false # 标签模式, 是否仅替换 live2d tag标签而非插入到所有页面中
  debug: false # 调试, 是否在控制台输出日志
  model:
    use: live2d-widget-model-wanko # npm-module package name
    # use: wanko # 博客根目录/live2d_models/ 下的目录名
    # use: ./wives/wanko # 相对于博客根目录的路径
    # use: https://cdn.jsdelivr.net/npm/live2d-widget-model-wanko@1.0.5/assets/wanko.model.json # 你的自定义 url
  react:
    opacity: 0.7
```
若需卸载，则在_config.yml和package.json中删除相关信息，终端切换到博客目录并运行以下命令。

```
npm uninstall -g hexo-helper-live2d
```

运行后搜索live2d并删除文件夹即可。

### 字体

配置文件中有关字体的配置如下。

```
font:
  enable: true

  # Uri of fonts host, e.g. https://fonts.googleapis.com (Default).
  host: https://fonts.loli.net

  # Font options:
  # `external: true` will load this font family from `host` above.
  # `family: Times New Roman`. Without any quotes.
  # `size: x.x`. Use `em` as unit. Default: 1 (16px)

  # Global font settings used for all elements inside <body>.
  global:
    external: true
    family: Noto Serif SC
    size: 0.9

  # Font settings for site title (.site-title).
  title:
    external: true
    family:
    size:

  # Font settings for headlines (<h1> to <h6>).
  headings:
    external: true
    family: Roboto Slab
    size: 1.7

  # Font settings for posts (.post-body).
  posts:
    external: true
    family: Noto Serif SC

  # Font settings for <code> and code blocks.
  codes:
    external: true
    family:
```

### 代码块样式

配置文件中有关代码块样式的配置如下。

```
codeblock:
  # Code Highlight theme
  # Available values: normal | night | night eighties | night blue | night bright | solarized | solarized dark | galactic
  # See: https://github.com/chriskempson/tomorrow-theme
  highlight_theme: night eighties
  # Add copy button on codeblock
  copy_button:
    enable: true
    # Show text copy result.
    show_result: true
    # Available values: default | flat | mac
    style: mac
```

### 回到首页

配置文件中有关的配置如下。

```
back2top:
  enable: true
  # Back to top in sidebar.
  sidebar: true
  # Scroll percent label in b2t button.
  scrollpercent: true
```

### 去除图片边框（已失效）

在`themes/next/source/css/_common/components/post/post-expand.styl`中修改以下代码即可。

```
img {
  box-sizing: border-box;
  margin: 0 auto 25px;
  padding: 3px;
-  border: 1px solid $gray-lighter;
+  border: none;
}
```

# Markdown语法

## 标题

标题可以有六级。

```
# 标题1
## 标题2
### 标题3
#### 标题4
##### 标题5
###### 标题6
```

## 区块引用

区块引用可以嵌套，也可以使用其他的Markdown语法，包括标题、列表、代码区块等。

```
> 引用
> # 标题
>
> > 嵌套引用
>
> 回到原引用
```

## 列表

### 有序列表

```
1. Bird
2. McHale
3. Parish
```

### 无序列表

```
* Red
* Green
* Blue

// 或
+ Red
+ Green
+ Blue

// 或
- Red
- Green
- Blue
```

## 分隔线

以下任意一种均可。

```
* * *
***
*****
​- - -
​---------------------------------------
```

## 超链接

### 行内式

```
This is [an example](http://example.com/ "Title") inline link.
[This link](http://example.net/) has no title attribute.
```

### 参考式

#### 显性参考式链接

链接辨别标签可以有字母、数字、空白和标点符号，但是并不区分大小写。

```
I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].
​
  [1]: http://google.com/        "Google"
  [2]: http://search.yahoo.com/  "Yahoo Search"
  [3]: http://search.msn.com/    "MSN Search"

// 选其一即可
[1]: http://example.com/  "Optional Title Here"
[1]: http://example.com/  'Optional Title Here'
[1]: http://example.com/  (Optional Title Here)
[1]: <http://example.com/>  "Optional Title Here"
```

#### 隐性链接标记

```
I get 10 times more traffic from [Google][] than from
[Yahoo][] or [MSN][].
​
  [google]: http://google.com/        "Google"
  [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
  [msn]:    http://search.msn.com/    "MSN Search"
```

## 强调

### 强调一行

```
*single asterisks*
_single underscores_
**double asterisks**
​__double underscores__
```

如果要在文字前后直接插入普通的星号或底线，则可以用反斜线。

```
\*this text is surrounded by literal asterisks\*
```

### 行内强调

```
un*frigging*believable
```

## 代码

```
Use the `printf()` function.
```

如果要在代码区段内插入反引号，可以用多个反引号来开启和结束代码区段。

```
``There is a literal backtick (`) here.``
```

## 图片

### 行内式

```
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional title")
```

### 参考式

```
![Alt text][id]
[id]: url/to/image  "Optional title attribute"
```

# 参考教程

## Markdown 语法说明 (简体中文版)

```
https://www.appinn.com/markdown/
```

## hexo的next主题个性化教程:打造炫酷网站

```
https://www.jianshu.com/p/f054333ac9e6
```

## NexT使用文档

```
http://theme-next.iissnan.com/
```

