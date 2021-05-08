---
title: Sublime 3的使用
categories: Skill
abbrlink: Sublime-Installation
date: 2019-12-24 08:16:29
tags:
---

![](https://tva1.sinaimg.cn/large/006tNbRwly1ga8x7npp86j30v50u0h1g.jpg)

Sublime 3是一款好用的文本编辑软件。

<!-- more -->

# 安装

## 直接安装破解版本

Mac版本可从以下链接获得。

```
https://xclient.info/s/sublime-text.html
```

安装后选择菜单Preferences->settings，在`{}`之间添加以下行以阻止自动更新。

```
"update_check":false
```

## 安装最新版后破解

本方法针对Windows系统。打开系统hosts文件并添加以下行。

```
#sublimetext　
127.0.0.1 www.sublimetext.com
127.0.0.1 sublimetext.com
127.0.0.1 sublimehq.com
127.0.0.1 telemetry.sublimehq.com
127.0.0.1 license.sublimehq.com
127.0.0.1 45.55.255.55
127.0.0.1 45.55.41.223
```

关闭sublime text3，打开安装目录，找到sublime_text.exe，备份一份，然后用二进制编辑器打开exe文件，搜索十六进制，输入97940D，替换为000000。将完成后的文件保存，覆盖原来的sublime_text.exe。

打开程序，点击菜单Help-Enter Lisence，输入以下注册码。若显示Thanks for Purchase，则成功。

```
----- BEGIN LICENSE -----
TwitterInc
200 User License
EA7E-890007
1D77F72E 390CDD93 4DCBA022 FAF60790
61AA12C0 A37081C5 D0316412 4584D136
94D7F7D4 95BC8C1C 527DA828 560BB037
D1EDDD8C AE7B379F 50C9D69D B35179EF
2FE898C4 8E4277A8 555CE714 E1FB0E43
D5D52613 C3D12E98 BC49967F 7652EED2
9D2D2E61 67610860 6D338B72 5CF95C69
E36B85CC 84991F19 7575D828 470A92AB
------ END LICENSE ------
```

# Package Control

Package Control用于在Sublime 3平台管理插件，项目地址如下。

```
https://packagecontrol.io/installation
```

## 安装

### 最新方法

新版本在菜单上直接有Install Package Control的选项，点击即可。注意可能要翻墙环境。

### 过时方法

<details>
<summary></summary>

在Sublime 3主界面用Ctrl+`或者通过View-Show Console菜单打开命令行，粘贴以下代码即可。

```
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

也可手动安装。点击Preferences-Browse Packages菜单，进入打开的目录的上层目录，然后再进入Installed Packages目录。从以下链接下载Package Control.sublime-package并复制到Installed Packages目录，重启Sublime Text。

```
https://sublime.wbond.net/Package Control.sublime-package
```
</details>

## 使用

在Sublime 3主界面点击Command/Ctrl+Shift+P，输入install后选中Install Package，回车即可安装插件。

## 常用插件

### ConvertToUTF8

在Mac下打开乱码的GBK编码文件，通常由Windows创建。需要先安装Codecs33插件。

### x86 and x86_64 Assembly

80×86汇编.asm文件的语法高亮插件。

### ChineseLocalizations

Sublime的语言包。

### VBScript

添加VB语言高亮支持。

### FindKeyConflicts

查询快捷键的冲突。安装后按`Ctrl+Shift+P`，输入`FindKeyConflicts`即可查看相关内容。

### HexViewer

十六进制显示。安装后需要点击Tools-Packages-Hex Viewer-Toggle Hex View以在本文件启用。

### Markdown相关

#### 使用优化

##### MarkDown Editing

Markdown语法高亮。打开Preferences-Package Settings-Markdown Editing-Markdown GFM Setting——User，添加以下代码以更改颜色样式。

```
{
    "color_scheme": "Packages/MarkdownEditing/MarkdownEditor.tmTheme",
    // "color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Dark.tmTheme",
    // "color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Yellow.tmTheme",
}
```

##### TableEditor

Markdown表格。需先按`Ctrl+Shift+P`，然后打开`Table Editor: Enable for current syntax`以在md格式中应用该插件，打开`Table Editor: Enable for current view`和`Table Editor: Set table syntax ... for current view`以在本文件中应用该插件。注意打开后Tab键将被该插件抢占。

##### SmartMarkdown

智能标题折叠，在标题上按`Tab`以折叠/展开本层，在任意位置按`Shift+Tab`以折叠/展开全文。

若Tab无法使用，表示快捷键被占用。进入Sublime的Preferences-Key Bindings，复制以下代码后保存，以后可用`Command+Shift+M`代替Tab。

```
{ "keys": ["shift+super+m"], "command": "smart_folding"}
```

#### 实时预览

##### markmon

暂时不可用。

##### MarkdownLivePreview

使用时按Ctrl+Shift+P，选择`MarkdownLivePreview: Open Preview`。

##### OmniMarkupPreviewer

安装后打开Preferences–Package Settings–OmniMarkupPreviewer–Setting——User，添加以下内容。

```
{
    "renderer_options-MarkdownRenderer":
    {
        "extensions": ["tables", "fenced_code", "codehilite"],
        "parser": "markdown",
        "enabled_parsers": ["markdown"]
    }
}
```

打开Preferences–Package Settings–OmniMarkupPreviewer–Setting——Default，修改以下内容以支持latex语法。

```
"mathjax_enabled": true,
```

使用时按Ctrl+Alt+O即可。

##### Markdown Preview+LiveReload

安装Markdown Preview和LiveReload，然后打开Preferences–Package Settings–Markdown Preview–Setting，在user设置中添加以下内容。

```
{
    "enable_autoreload": true,
}
```

按Ctrl+Shift+P，输入`LiveReload: Enable/disable plug-ins`，选择`Enable: Simple Reload`。然后选择Preferences—Key Bindings-User，添加以下内容。

```
[
    { "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"} }
]
```

使用时按Alt+M即可。

## 常见问题

### 无法显示插件目录

需要翻墙。或者将以下链接的文件下载到本地，然后在Sublime Text中点击Preference-Package Setting-Package Control-User Setting。

```
https://packagecontrol.io/channel_v3.json
```

修改以下代码即可。

```
"channels": [
    "/Users/..." // 以上文件的路径
]
```

### 下载报错

Github采用https协议，而sublime使用urllib，故需对Package Control进行配置。点击Preference-Package Setting-Package Control-User Setting，添加以下代码即可。

```
"downloader_precedence":
{
    "linux":
    [
        "curl",
        "urllib",
        "wget"
    ],
    "osx":
    [
        "curl",
        "wget"
    ],
    "windows":
    [
        "wininet"
    ]
},
```

# 编译功能配置

Sublime自带编译器，能够对C、C++、Java、Python等语言源代码进行编译。在菜单工具-编译系统中可以看到当前选择的编译器，一般程序会根据文件后缀自动做出正确的选择。

使用`Ctrl/Command+Shift+B`即跳出编译选项，按照提示进行即可。

# 语法高亮

## Quantumult X配置文件

在Sublime中点击Preferences-Browse Packages...，进入User文件夹。在该文件夹下新建Quantumult.sublime-syntax文件，复制以下代码并保存即可。

```
%YAML 1.2
---
name: Quantumult Conf
file_extensions: [conf]
scope: source.c

contexts:
  main:
    - match: '([\w|-]+(|\s)+(?=([\=][\-]?)))'
      scope: keyword.other
    - match: '^\[.+\]'
      scope: entity.name.section
    - match: '\\/|\^|\?'
      scope: entity.name
    - match: '((https|http)?:\/\/)[^(\s)|(,)]+'
      scope: constant.character.escape
    - match: '(\#|;)(.+|$)'
      scope: comment.number-sign
```

# 分屏显示

点击菜单栏上的视图-布局，即可选择不同的布局模式。

若希望两边同时显示同一个文档，可选择该文档，点击文件-克隆文件，然后把标签页拉到另一边即可。

# 参考教程

## Sublime text3 Version 3.2.2, Build 3211破解

```
https://www.cnblogs.com/Fluorescence-tjy/p/11910345.html
```

## Sublime Text3 Markdown 编辑+实时预览

```
https://juejin.cn/post/6844903741183754253
```

## Why it doesn't work? · Issue #44 · demon386/SmartMarkdown

```
https://github.com/demon386/SmartMarkdown/issues/44
```

## Sublime Text新增gbk编码支持

```
https://blog.csdn.net/a7442358/article/details/84063491
```

## 分享一个Sublime Text的Quantumult X语法高亮

```
https://blog.butanediol.me/2020/05/17/%E5%88%86%E4%BA%AB%E4%B8%80%E4%B8%AA-Sublime-Text-%E7%9A%84-Quantumult-X-%E8%AF%AD%E6%B3%95%E9%AB%98%E4%BA%AE/
```

## Sublime Text3插件 --- HexViewer的使用（16进制显示）

```
https://blog.csdn.net/zldz14/article/details/82183530
```

## Sublime Text 如何一个代码双屏显示代码上下部分？

```
https://www.zhihu.com/question/32381975
```
