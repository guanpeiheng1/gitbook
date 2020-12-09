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

# 安装与引导

## 安装Ubuntu

用`rufus`将ubuntu镜像拷录到U盘。备份硬盘的`EFI`，用Diskgenius为ubuntu安装分配空间，并格式化为NTFS。

重启电脑并从U盘启动，进入安装程序。在安装选项中选`其他选项`，进入分区界面。选择刚才的分区并点击`分区`，格式选择`Ex4`，挂载点选择`/`，在`安装启动引导器的设备`下拉菜单，选择刚才格式化好的分区，点击安装。

安装完成后，若Windows引导丢失，可用Diskgenius将备份好的EFI/BOOT中的Microsoft文件夹复制到EFI分区的EFI/BOOT下，与ubuntu文件夹同级即可。

## grub2引导器配置

在grub2引导器下（若无引导菜单则在开机时按Shift），按`C`进入控制台模式并输入以下命令，以得到grub所支持的显示器的分辨率。记下后按`Esc`退出控制台模式并进入Ubuntu。

```
videoinfo
```

打开终端输入以下命令，以获取文件管理器的超级权限。

```
sudo nautilus
```

编辑系统盘的`/etc/default/grub`并修改以下位置，然后在终端点击`Ctrl+C`结束程序。

```
- #GRUB_GFXMODE=640x480
+ GRUB_GFXMODE=1920x1080,1366x768,1280x1024,1024x768,800x600,640x480
```

打开以下链接，下载`Grub-theme-vimix`主题。

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

Deepin容器可用于安装Windows软件。打开终端并输入以下命令，安装`git`并下载`deepin-wine`容器到工作目录。

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

## 常用软件

### 软件合集

```
https://github.com/luong-komorebi/Awesome-Linux-Software
```

### Deepin-wine仓库

```
https://mirrors.aliyun.com/deepin/pool/non-free/d
```

### TIM

```
https://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.office/
```

### 微信

```
https://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.wechat/
```

### Google Chrome

```
https://www.google.cn/chrome/
```

### WPS Office

```
https://www.wps.cn/product/wpslinux/
```

### 网易云音乐

```
https://music.163.com/#/download
```

### Smplayer

在终端利用`apt-get`命令安装。

```
sudo apt-get install smplayer
```

### Spotify

在终端输入以下命令。

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys BBEBDCB318AD50EC6865090613B00F1FD2C19886
echo deb http://repository.spotify.com stable non-free | sudo tee /etc/apt/sources.list.d/spotify.list
sudo apt-get update
sudo apt-get install spotify-client
```

### Musixmatch

```
https://adv.musixmatch.com/r/wwwmxm
```

## 系统美化

打开终端并输入以下命令，安装主题管理器及部分扩展。

```
sudo apt-get install gnome-tweak-tool
sudo apt-get install gnome-shell-extensions
```

重启电脑后，在应用程序菜单（即Dashboard）打开`优化`，此时`外观`选项卡下，`Shell`处于禁用状态。

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

```
应用程序为Vimix-dark-laptop-ruby
光标为DMZ-Black
图表为Vimix-Paper
Shell为Vimix-dark-laptop-ruby
背景/锁屏自定义
```

在`桌面`选项卡中打开`回收站`，在`扩展`选项卡中打开`Dash to panel`并点击设置图标，进行个性化设置。

## 显示此电脑图标

打开文件浏览器并转到`/usr/share/applications`，找到`Files`文件并拖到桌面。右键点击`属性`，切换到`权限`选项卡，勾选`允许作为程序执行文件`，双击打开后文件变成图标。右键点击`属性`，选择自定义图标，路径如下。

```
/home/用户名/.local/share/icons/Vimix-Paper/scalable/devices/computer.png
```

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

# 使用技巧

## 显示Ubuntu的隐藏文件

按`Ctrl+H`即可，文件（夹）名称前带有`.`的为隐藏文件。

## 快速打开终端

任意位置按`Ctrl+Alt+T`即可。

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

其它版本只需将utopic或precise更换为对应的Codename即可。

```
breezy
dapper
edgy
feisty
gutsy
hardy
hoary
intrepid
jaunty
karmic
lucid
maverick
natty
oneiric
quantal
raring
saucy
utopic
warty
```

也可通过以下命令查看Codename。

```
lsb_release -a
```

保存文件后运行以下命令以生效。

```
sudo apt-get update
sudo apt-get upgrade
```

## 常见问题

### WPS for Linux显示系统缺失字体

打开以下链接下载字体库。

```
网站 / https://pan.baidu.com/s/1xil5_i9M53fM7EQNIt3Mcw
密码 / jqnu
```

在终端输入以下命令，重启WPS即可。

```
sudo unzip wps_symbol_fonts.zip -d /usr/share/fonts/wps-office
```

### Shift打不出特殊符号

将键盘布局设置成英语（美国）即可。

### 输入管理员密码后提示「认证失败」

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

## Ubuntu的引导

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

## Linux切换终端工作目录的方法

可利用`cd`命令，或文件管理器打开需切换到的工作目录，右键点击`在终端打开`。

## Linux的软件安装目录

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

# 相关下载

## Ubuntu镜像

```
https://www.ubuntu.com/download
```

## rufus

```
https://rufus.ie/
```

# 参考教程

## 国内老版本ubuntu更新源地址以及sources.list的配置方法

```
https://blog.csdn.net/snaking616/article/details/52966634
```


