---
title: Mac和Linux下终端优化
categories: Computer
abbrlink: Terminal-Optimization
date: 2019-11-25 13:06:29
tags:
---

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g9ad0bvjl7j312q0rutjq.jpg)

对Mac和Linux下终端进行优化，可以大大提升用户体验。

<!-- more -->

# 设置Shell

## 设置默认Shell为zsh

使用`zsh`为默认shell，可应用插件和主题，实现默认的bash所不能实现的功能。

可在终端输入以下命令查看当前使用的shell和已安装的shell。

```
echo $SHELL
cat /etc/shells
```

最新的Mac Catalina中已内置zsh，Linux和未安装zsh的Mac可通过以下命令安装。

```
// Mac下用Homebrew安装
brew install zsh

// Linux下用apt-get安装
sudo apt-get install zsh
```

在终端输入以下命令，更换默认shell为zsh，重启终端生效。

```
chsh -s /bin/zsh
```

## 暂时在zsh中使用bash

```
exec /bin/bash
```

# oh-my-zsh安装及配置

## oh-my-zsh安装

采用下列命令安装oh-my-zsh。

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

在终端输入以下命令以打开配置文件，若提示权限不足，则在命令前加`sudo`。

```
vim ~/.zshrc
```

按i键以修改文件。修改`ZSH_THEME`，即可修改oh-my-zsh的主题。oh-my-zsh的GitHub Wiki页面提供了主题列表。

```
https://github.com/robbyrussell/oh-my-zsh/wiki/themes
```

推荐使用以下主题。

```
// Linux用户
ZSH_THEME="agnoster"

// Mac用户
ZSH_THEME="trapd00r"
```

在文件末尾加入以下代码后按`Esc`，然后输入`:wq`并回车，保存并退出。

```
DEFAULT_USER=$USER
```

修改完成后，需运行以下命令以更新zsh配置。

```
source ~/.zshrc
```

## 插件安装

### 语法高亮插件zsh-syntax-highlighting

在终端输入以下命令安装。

```
cd ~/.oh-my-zsh/custom/plugins &&\
git clone git://github.com/zsh-users/zsh-syntax-highlighting.git
```

修改~/.zshrc中的plugins，添加`zsh-syntax-highlighting`即可。

```
plugins=(
	zsh-syntax-highlighting
)
```

### 自动建议补全插件zsh-autosuggestions

在终端输入以下命令安装。

```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

修改~/.zshrc中的plugins，添加`zsh-autosuggestions`即可。

```
plugins=(
	zsh-autosuggestions
)
```

### 自动跳转插件auto-jump

在终端输入以下命令安装。

```
git clone git://github.com/wting/autojump.git
cd autojump
./install.py or ./uninstall.py
```

### 自动补全插件incr

打开下面网站下载incr插件，并放置于`/.oh-my-zsh/customs/plugins/incr`中（若无则新建）。

```
https://mimosa-pudica.net/zsh-incremental.html
```

修改~/.zshrc，在文件末尾添加以下代码即可。

```
source $ZSH/custom/plugins/incr/incr*.zsh
```

## 安装agnoster主题字体（Linux）

在终端输入下列命令以安装`powerline fonts`项目。

```
git clone https://github.com/powerline/fonts
./fonts/install.sh
```

在终端窗口点击右键，选择`配置文件`，点击`配置文件首选项`，应用以下设置并保存。

```
自定义字体更改为Ubuntu Mono derivative Powerline
背景透明度设为10%
样式采用Tango
```

# Homebrew

`Homebrew`为Mac下一款方便的软件包管理工具，在命令行输入一行命令即可完成安装、卸载、更新、查看、搜索等功能。

## 安装

### 官方安装

在终端输入以下命令即可。

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 国内安装方法（成功）

```
https://zhuanlan.zhihu.com/p/111014448


安装脚本：
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
卸载脚本：
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```

### 国内安装方法（失效）

#### 获取install文件

在终端输入以下命令。

```
curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install >> brew_install
```

#### 更改资源链接

打开所获取的install文件（一般在用户目录），更改脚本中`BREW_REPO`和`CORE_TAP_REPO`两处，两个镜像源任选一个。

```
// 中国科学技术大学源
BREW_REPO = "https://mirrors.ustc.edu.cn/brew.git".freeze
CORE_TAP_REPO = "https://mirrors.ustc.edu.cn/homebrew-core.git".freeze

// 清华大学源
BREW_REPO = "https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git".freeze
CORE_TAP_REPO = "https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git".freeze
```

#### 执行脚本

在终端输入以下命令。

```
/usr/bin/ruby brew_install
```

## 使用

### 更换默认源

```
// 中国科学技术大学源
// 替换核心软件仓库
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

// 替换cask软件仓库
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

// 替换Bottles源
// 以下二选一
（bash用户）
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile

（zsh用户）
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc


// 清华大学源
// 替换核心软件仓库
cd "$(brew --repo)"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git

// 替换cask软件仓库
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

// 替换Bottles源
// 以下二选一
（bash用户）
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile

（zsh用户）
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

完成后在终端输入以下命令更新Homebrew。

```
brew update
brew upgrade
```

### 常用命令

|          命令         |      操作      |
|-----------------------|----------------|
| brew help             | 帮助           |
| brew list             | 列出安装包     |
| brew update           | 更新Homebrew   |
| brew upgrade          | 更新包         |
| brew install [包名]   | 安装包         |
| brew uninstall [包名] | 卸载包         |
| brew -v               | Homebrew版本   |
| brew search [包名]    | 搜索可安装的包 |

## 卸载

在终端输入以下命令。

```
cd `brew --prefix`
rm -rf Cellar
brew prune
rm `git ls-files`
rm -r Library/Homebrew Library/Aliases Library/Formula Library/Contributions
rm -rf .git
rm -rf ~/Library/Caches/Homebrew
```

# 替代终端的软件

## iTerm2

```
https://iterm2.com/
```

## Hyper

```
https://hyper.is/#installation
```