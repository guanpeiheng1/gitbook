---
title: Linux系统的安装与配置
categories: Linux
abbrlink: Linux-Installation
date: 2019-11-26 09:50:29
tags:
---

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g9b69on8vmj30p10fj74q.jpg)

安装Linux系统，可以完成许多在Windows下无法完成的操作。

<!-- more -->

# 版本迭代

|    版本   | Codename |      开发代号     |  发布日期  | 桌面版支持结束时间 | 服务器版支持结束时间 | 内核版本 |
|-----------|----------|-------------------|------------|--------------------|----------------------|----------|
| 4.10      | warty    | Warty Warthog     | 2004-10-20 | 2006-04-30         | 2006-04-30           | 2.6.8    |
| 5.04      | hoary    | Hoary Hedgehog    | 2005-04-08 | 2006-10-31         | 2006-10-31           | 2.6.10   |
| 5.10      | breezy   | Breezy Badger     | 2005-10-13 | 2007-04-13         | 2007-04-13           | 2.6.12   |
| 6.06 LTS  | dapper   | Dapper Drake      | 2006-06-01 | 2009-07-14         | 2011-06-01           | 2.6.15   |
| 6.10      | edgy     | Edgy Eft          | 2006-10-26 | 2008-04-25         | 2008-04-25           | 2.6.17   |
| 7.04      | feisty   | Feisty Fawn       | 2007-04-19 | 2008-10-19         | 2008-10-19           | 2.6.20   |
| 7.10      | gutsy    | Gutsy Gibbon      | 2007-10-18 | 2009-04-18         | 2009-04-18           | 2.6.22   |
| 8.04 LTS  | hardy    | Hardy Heron       | 2008-04-24 | 2011-05-12         | 2013-05-09           | 2.6.24   |
| 8.10      | intrepid | Intrepid Ibex     | 2008-10-30 | 2010-04-30         | 2010-04-30           | 2.6.27   |
| 9.04      | jaunty   | Jaunty Jackalope  | 2009-04-23 | 2010-10-23         | 2010-10-23           | 2.6.28   |
| 9.10      | karmic   | Karmic Koala      | 2009-10-29 | 2011-04-30         | 2011-04-30           | 2.6.31   |
| 10.04 LTS | lucid    | Lucid Lynx        | 2010-04-29 | 2013-05-09         | 2015-04-30           | 2.6.32   |
| 10.10     | maverick | Maverick Meerkat  | 2010-10-10 | 2012-04-10         | 2012-04-10           | 2.6.35   |
| 11.04     | natty    | Natty Narwhal     | 2011-04-28 | 2012-10-28         | 2012-10-28           | 2.6.38   |
| 11.10     | oneiric  | Oneiric Ocelot    | 2011-10-13 | 2013-05-09         | 2013-05-09           | 3.0      |
| 12.04 LTS | precise  | Precise Pangolin  | 2012-04-26 | 2017-04-28         | 2017-04-28           | 3.2      |
| 12.10     | quantal  | Quantal Quetzal   | 2012-10-18 | 2014-05-16         | 2014-05-16           | 3.5      |
| 13.04     | raring   | Raring Ringtail   | 2013-04-25 | 2014-01-27         | 2014-01-27           | 3.8      |
| 13.10     | saucy    | Saucy Salamander  | 2013-10-17 | 2014-07-17         | 2014-07-17           | 3.11     |
| 14.04 LTS | trusty   | Trusty Tahr       | 2014-04-17 | 2019-04-25         | 2019-04-25           | 3.13     |
| 14.10     | utopic   | Utopic Unicorn    | 2014-10-23 | 2015-07-23         | 2015-07-23           | 3.16     |
| 15.04     | vivid    | Vivid Vervet      | 2015-04-23 | 2016-02-04         | 2016-02-04           | 3.19     |
| 15.10     | wily     | Wily Werewolf     | 2015-10-22 | 2016-07-28         | 2016-07-28           | 4.2      |
| 16.04 LTS | xenial   | Xenial Xerus      | 2016-04-21 | 2021-04            | 2021-04              | 4.4      |
| 16.10     | yakkety  | Yakkety Yak       | 2016-10-13 | 2017-07-20         | 2017-07-20           | 4.8      |
| 17.04     | zesty    | Zesty Zapus       | 2017-04-13 | 2018-01-13         | 2018-01-13           | 4.10     |
| 17.10     | artful   | Artful Aardvark   | 2017-10-19 | 2018-07-19         | 2018-07-19           | 4.13     |
| 18.04 LTS | bionic   | Bionic Beaver     | 2018-04-26 | 2023-04            | 2023-04              | 4.15     |
| 18.10     | cosmic   | Cosmic Cuttlefish | 2018-10-18 | 2019-07-18         | 2019-07-18           | 4.18     |
| 19.04     | disco    | Disco Dingo       | 2019-04-18 | 2020-01-23         | 2020-01-23           | 5.0      |
| 19.10     | eoan     | Eoan Ermine       | 2019-10-17 | 2020-07-17         | 2020-07-17           | 5.3      |
| 20.04 LTS | focal    | Focal Fossa       | 2020-04-23 | 2025-04            | 2025-04              | 5.4      |
| 20.10     | groovy   | Groovy Gorilla    | 2020-10-22 | 2021-07            | 2021-07              | 5.8      |
| 21.04     | hirsute  | Hirsute Hippo     | 2021-04-22 | 2022-01            | 2022-01              | 未定     |


# 安装与引导

## 安装

用rufus将ubuntu镜像拷录到U盘。备份硬盘的EFI，用Diskgenius为ubuntu安装分配空间，并格式化为NTFS。

重启电脑并从U盘启动，进入安装程序。在安装选项中选其他选项，进入分区界面。选择刚才的分区并点击`分区`，格式选择`Ex4`，挂载点选择`/`，在`安装启动引导器的设备`下拉菜单，选择刚才格式化好的分区，点击安装。

安装完成后，若Windows引导丢失，可用Diskgenius将备份好的EFI/BOOT中的Microsoft文件夹复制到EFI分区的EFI/BOOT下，与ubuntu文件夹同级即可。

## 引导器配置

在grub2引导器下（若无引导菜单则在开机时按Shift），按`C`进入控制台模式并输入以下命令，以得到grub所支持的显示器的分辨率。记下后按`Esc`退出控制台模式并进入Ubuntu。

```
videoinfo
```

打开终端输入以下命令，以获取文件管理器的超级权限。

```
sudo nautilus
```

编辑系统盘的`/etc/default/grub`并修改以下位置，内容为刚才记录的分辨率。修改完成后保存并退出。

```
- #GRUB_GFXMODE=640x480
+ GRUB_GFXMODE=1920x1080,1366x768,1280x1024,1024x768,800x600,640x480
```

打开以下链接，下载Grub-theme-vimix主题。

```
https://www.gnome-look.org/p/1009236/
```

解压后终端切换到解压目录并输入以下命令，完成grub主题安装并更新grub引导器。

```
./Install
sudo update-grub
```

# 配置

## 安装Deepin容器

Deepin容器可用于安装Windows软件。打开终端并输入以下命令，安装git并下载deepin-wine容器到工作目录。

```
sudo apt-get install git
git clone https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu.git
```

到工作目录下解压压缩包后，将终端切换到解压目录下并输入以下命令，完成deepin-wine容器安装。

```
./install.sh
```

若出现错误，则执行以下命令。

```
./KDE-install.sh
```

## 系统美化

打开终端并输入以下命令，安装主题管理器及部分扩展。

```
sudo apt-get install gnome-tweak-tool
sudo apt-get install gnome-shell-extensions
```

重启电脑后，在应用程序菜单打开`优化`，此时`外观`选项卡下，`Shell`处于禁用状态。

打开以下网站，网站提示需要安装扩展。进入Chrome扩展程序商店，搜索`GNOME Shell integration`并完成安装。

```
https://extensions.gnome.org/extension/19/user-themes/
```

再次进入网站，提示未检测到本机主机连接器。打开终端并输入以下命令以修复。

```
sudo apt install chrome-gnome-shell
```

完成安装后再次打开网站，正常情况下不再弹出错误信息。安装该网站上的扩展`User Themes`，再次打开`优化`，发现Shell主题已经可用。

到以下网站安装`dash-to-panel`插件，使桌面呈现任务栏风格。

```
https://extensions.gnome.org/extension/1160/dash-to-panel/
```

到以下网站下载最新版Vimix主题的zip包。

```
https://github.com/vinceliuice/vimix-gtk-themes/releases
```

解压后在终端打开解压目录位置，并输入以下命令完成Vimix主题的安装。

```
./Install
```

到以下网站下载最新版Vimix图标的zip包。

```
https://github.com/vinceliuice/vimix-icon-theme/releases
```

解压后在终端打开解压目录位置，并输入以下命令完成Vimix图标的安装。

```
./Install.sh
```

打开`优化`，在`外观`选项卡中更改下列选项。

|   选项   |          内容          |
|----------|------------------------|
| 应用程序 | Vimix-dark-laptop-ruby |
| 光标     | DMZ-Black              |
| 图表     | Vimix-Paper            |
| Shell    | Vimix-dark-laptop-ruby |

在`桌面`选项卡中打开`回收站`，在`扩展`选项卡中打开`Dash to panel`并点击设置图标，进行个性化设置。

## Dashboard自动分类

打开以下命令，下载压缩包后解压。

```
https://github.com/BenJetson/gnome-dash-fix/releases
```

终端切换到当前工作目录并输入以下命令执行脚本即可。

```
chmod +x interactive.py
./interactive.py
```

## 终端相关

### 终端设置

#### 设置默认Shell

使用`zsh`为默认shell，可应用插件和主题，实现默认的bash所不能实现的功能。

可在终端输入以下命令查看当前使用的shell和已安装的shell。

```
echo $SHELL
cat /etc/shells
```

若未安装zsh，可通过以下命令安装。

```
sudo apt-get install zsh
```

在终端输入以下命令，更换默认shell为zsh，重启终端生效。

```
chsh -s /bin/zsh
```

#### 安装agnoster主题字体

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

#### oh-my-zsh

oh-my-zsh可用于加强zsh终端。

##### 安装

终端输入以下命令安装。

```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

在终端输入以下命令以打开配置文件。

```
sudo vim ~/.zshrc
```

按i键以修改文件。修改`ZSH_THEME`，即可修改oh-my-zsh的主题。oh-my-zsh的GitHub Wiki页面提供了主题列表。

```
https://github.com/robbyrussell/oh-my-zsh/wiki/themes
```

推荐使用以下主题。

```
ZSH_THEME="agnoster"
```

在文件末尾加入以下代码后按`Esc`，然后输入`:wq`并回车，保存并退出。

```
DEFAULT_USER=$USER
```

修改完成后，需运行以下命令以更新zsh配置。

```
source ~/.zshrc
```

##### 插件

###### zsh-syntax-highlighting

语法高亮插件，在终端输入以下命令安装。

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

###### zsh-autosuggestions

自动建议补全插件，在终端输入以下命令安装。

```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

修改~/.zshrc中的plugins，添加`zsh-autosuggestions`即可。

```
plugins=(
    zsh-autosuggestions
)
```

###### auto-jump

自动跳转插件，在终端输入以下命令安装。

```
git clone git://github.com/wting/autojump.git
cd autojump
./install.py or ./uninstall.py
```

###### incr

自动补全插件。打开下面网站下载incr插件，并放置于`/.oh-my-zsh/customs/plugins/incr`中（若无则新建）。

```
https://mimosa-pudica.net/zsh-incremental.html
```

修改~/.zshrc，在文件末尾添加以下代码即可。

```
source $ZSH/custom/plugins/incr/incr*.zsh
```

#### proxychains-ng

proxychains-ng可用于终端翻墙。通过以下链接下载仓库压缩包并解压。

```
https://github.com/rofl0r/proxychains-ng
```

终端切换到解压目录后，运行以下命令以编译。

```
./configure --prefix=/usr --sysconfdir=/etc
sudo make
sudo make install
sudo make install-config
```

在终端输入以下命令。

```
vim /etc/proxychains.conf
```

在`[ProxyList]`下注释掉原来的代理，并添加代理类型，如

```
socks5 127.0.0.1 9050
```

如果所在的网络很复杂，可能需要在配置文件中启用`dynamic_chain`，然后在[ProxyList]下添加多个代理。两种类型的区别如下。

|         类型         |                                                     含义                                                     |
|----------------------|--------------------------------------------------------------------------------------------------------------|
| dynamic_chain        | 按照列表中出现的代理服务器的先后顺序组成一条链，如果有代理服务器失效，则自动将其排除，但至少要有一个是有效的 |
| strict_chain（默认） | 按照后面列表中出现的代理服务器的先后顺序组成一条链，要求所有的代理服务器都是有效的                           |

完成后即可使用，示例如下。

```
proxychains curl http://www.google.com
```

### 常用命令

#### 检查json文件

```
jq . config.json
```

# 使用技巧

## 显示隐藏文件

按`Ctrl+H`即可，文件（夹）名称前带有`.`的为隐藏文件。

## 快速打开终端

任意位置按`Ctrl+Alt+T`即可。

## 显示此电脑图标

打开文件浏览器并转到`/usr/share/applications`，找到`Files`文件并拖到桌面。右键点击`属性`，切换到`权限`选项卡，勾选`允许作为程序执行文件`，双击打开后文件变成图标。右键点击`属性`，选择自定义图标，路径如下。

```
/home/[用户名]/.local/share/icons/Vimix-Paper/scalable/devices/computer.png
```

## 安装rpm包

在终端输入以下命令即可。

```
sudo apt-get install alien
sudo alien xxxx.rpm // 将rpm转换为deb
sudo dpkg -i xxxx.deb
```

## 更换软件源

### 仍在维护的版本

对于仍在维护的版本，打开`软件与更新`，在`下载自`处下拉并选择其他站点，然后选择中国的站点即可。

若无法安装，则可以在`其他软件`选项卡中取消部分软件源的勾选。

### 已经停止维护的版本

对于已经停止维护的版本，需要手动指定软件源。

打开终端并执行以下命令以打开软件源列表文件。

```
sudo gedit /etc/apt/sources.list
```

将文件内容替换成以下格式。

```
deb 源地址/ubuntu/ 版本代号 main restricted universe multiverse
deb 源地址/ubuntu/ 版本代号-security main restricted universe multiverse
deb 源地址/ubuntu/ 版本代号-updates main restricted universe multiverse
deb 源地址/ubuntu/ 版本代号-proposed main restricted universe multiverse
deb 源地址/ubuntu/ 版本代号-backports main restricted universe multiverse
deb-src 源地址/ubuntu/ 版本代号 main restricted universe multiverse
deb-src 源地址/ubuntu/ 版本代号-security main restricted universe multiverse
deb-src 源地址/ubuntu/ 版本代号-updates main restricted universe multiverse
deb-src 源地址/ubuntu/ 版本代号-proposed main restricted universe multiverse
deb-src 源地址/ubuntu/ 版本代号-backports main restricted universe multiverse
```

以中科大的源为例，对于Ubuntu 14.10，内容如下。

```
deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-security main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-updates main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-proposed main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-backports main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-security main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-updates main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-proposed main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-backports main restricted universe multiverse
```

如果是长期更新源，即带有LTS后缀的版本，以Ububtu 12.04(LTS)为例，内容如下。

```
deb http://debian.ustc.edu.cn/ubuntu/ precise main restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ precise-backports restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ precise-proposed main restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ precise-security main restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ precise-updates main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ precise main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ precise-backports main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ precise-proposed main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ precise-security main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ precise-updates main restricted universe multiverse
```

其它版本只需将utopic或precise更换为对应的Codename，可通过以下命令查看。

```
lsb_release -a
```

保存文件后运行以下命令以生效。

```
sudo apt-get update
sudo apt-get upgrade
```

# 常用软件

## 软件合集

```
https://github.com/luong-komorebi/Awesome-Linux-Software
```

## Deepin-wine仓库

```
https://mirrors.aliyun.com/deepin/pool/non-free/d
```

## TIM

```
https://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.office/
```

## 微信

```
https://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.wechat/
```

## Google Chrome

```
https://www.google.cn/chrome/
```

## WPS Office

```
https://www.wps.cn/product/wpslinux/
```

## 网易云音乐

```
https://music.163.com/#/download
```

## Smplayer

在终端利用`apt-get`命令安装。

```
sudo apt-get install smplayer
```

## Spotify

在终端输入以下命令。

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys BBEBDCB318AD50EC6865090613B00F1FD2C19886
echo deb http://repository.spotify.com stable non-free | sudo tee /etc/apt/sources.list.d/spotify.list
sudo apt-get update
sudo apt-get install spotify-client
```

## Musixmatch

```
https://adv.musixmatch.com/r/wwwmxm
```

# 常见问题

## WPS for Linux显示系统缺失字体

打开以下链接下载字体库。

```
网站 / https://pan.baidu.com/s/1xil5_i9M53fM7EQNIt3Mcw
密码 / jqnu
```

在终端输入以下命令，重启WPS即可。

```
sudo unzip wps_symbol_fonts.zip -d /usr/share/fonts/wps-office
```

## Shift打不出特殊符号

将键盘布局设置成英语（美国）即可。

## 输入管理员密码后提示「认证失败」

打开终端并输入以下命令。

```
sudo vi /etc/pam.d/gdm-autologin
sudo vi /etc/pam.d/gdm-password
```

两者均注释以下行即可。

```
auth requied pam_succeed_if.so user != root quiet success
```

# 附录

## 引导器

### grub/grub2引导器

Ubuntu通过grub/grub2引导器引导系统，主要文件（夹）有三个。

#### /etc/grub.d

配置grub启动项，其中40_custom和41_custom供手动添加启动项。

#### /etc/default/grub

配置grub2引导器的各种选项，包括分辨率等。

#### /boot/grub/grub.cfg

根据前两个文件（夹）通过命令sudo update-grub生成，每次update会生成不同的grub.cfg，启动时根据grub.cfg生成引导菜单

### 安装Grub-Customizer管理引导项

在终端输入以下命令即可。

```
sudo add-apt-repository ppa:danielrichter2007/grub-customizer
sudo apt-get install grub-customizer
```

## 软件安装目录

### /usr

系统级的目录，可以理解为C:/Windows/。

### /usr/lib

理解为C:/Windows/System32。

### /usr/local

用户级的程序目录，可以理解为C:/Progrem Files/。用户自己编译的软件默认会安装到这个目录下。

### /opt

用户级的程序目录，可以理解为D:/Software，opt有可选的意思，这里可以用于放置第三方大型软件（或游戏），当你不需要时，直接rm -rf掉即可。在硬盘容量不够时，也可将/opt单独挂载到其他磁盘上使用。

### /usr/src

系统级的源码目录。

### /usr/local/src

用户级的源码目录。

# 参考教程

## 国内老版本ubuntu更新源地址以及sources.list的配置方法

```
https://blog.csdn.net/snaking616/article/details/52966634
```
