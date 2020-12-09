---
title: Mac使用技巧总结
categories: Mac
abbrlink: Mac-Skills
date: 2019-11-25 14:12:29
tags:
---

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g9adg1f7usj31c00u0u15.jpg)

总结了一些MacOS的使用技巧，不断更新。

<!-- more -->

# 常见问题

```
._文件删除：
dot_clean [路径]

.DS_Store：
https://qastack.cn/apple/14980/why-are-dot-underscore-_-files-created-and-how-can-i-avoid-them
https://www.jianshu.com/p/fdaa8be7f6c3
```



## 构建Xcode时提示「xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory」

在终端输入以下命令即可。

```
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer/ 
xcode-select --print-path
// 输出/Applications/Xcode.app/Contents/Developer
```

## App在Catalina下提示已损坏

在终端输入以下命令即可。

```
// /Applications/xxxx.app换为所需App路径，推荐直接将app文件拖入终端
sudo xattr -d com.apple.quarantine /Applications/xxxx.app
```

## 因AppleGFXHDADriver重启

将SLE下的AppleGFXHDA.kext删除或更改后缀即可。

# 系统技巧

## 旧款Mac安装最新Catalina

使用`macOS Catalina Patcher`即可。

```
http://dosdude1.com/catalina/
```

## 不使用Boot Camp助手下载Boot Camp驱动程序

使用`Brigadier`即可。

```
https://github.com/timsutton/brigadier/releases
```

## 在安全模式下读取EFI分区

注意，此法不能读取内置Windows分区和外接驱动器（非APFS/HFS）分区。

在正常模式的系统下执行以下命令以获取msdosfs.kext，默认在根目录的kexts文件夹下。

```
sudo mkdir /kexts
sudo cp -RX /System/Library/Extensions/msdosfs.kext /kexts
sudo /usr/libexec/PlistBuddy -c "Add :OSBundleRequired string" /kexts/msdosfs.kext/Contents/Info.plist
sudo /usr/libexec/PlistBuddy -c "Set :OSBundleRequired \"Safe Boot\"" /kexts/msdosfs.kext/Contents/Info.plist
sudo chmod -Rf 755 /kexts/msdosfs.kext
sudo chown -Rf 0:0 /kexts/msdosfs.kext
```

在安全模式下通过以下命令加载此kext，再挂载EFI分区即可。

```
sudo kextload /kexts/msdosfs.kext
```

## 利用系统VPN连接学校有线网络

打开`系统偏好设置`-`网络`，点击`+`，接口选择`vpn`，类型选`L2TP/IPSec`，完成后填写下列信息。

```
服务器地址 / 10.5.1.7
账户名称 / 学号@a10（20/50元包月则为@a20/@a50）
```

点击`鉴定设置-密码`并输入，保存后点击`高级`。会话选项下三个选项均勾选，保存。打开终端输入以下代码。 

```
sudo vim /etc/ppp/options
```

按`i`键后输入下列语句。

```
plugin L2TP.ppp
l2tpnoipsec（第一个字母为小写L） 
```

按`Esc`后输入`:wq`并回车，回到网络设置界面。连接上述设置好的VPN，正常情况下连接成功。

## 删除蓝牙配置文件以重置设置

进入系统盘的`/Library/Preference`，删除`com.apple.Bluetooth.plist`文件后重启即可。

## 备份EFI分区脚本

脚本地址如下。

```
https://github.com/corpnewt/EFI-Backup-Restore
```

## zsh导入bash环境变量

向`~/.zshrc`添加以下代码。

```
source ~/.bash_profile
```

在终端执行以下命令即可。

```
source .zshrc
```

# 操作技巧

## Menu Bar操作

按住Command键并拖动菜单栏图标可调整其顺序，拖出菜单栏则清除此图标。

若菜单栏上没有图标，则转至`/System/Library/CoreServices/Menu Extras`，直接用鼠标将Eject.menu拖放至菜单栏即可。

## 常用快捷键

Mac键盘与普通键盘键位对应如下表。 

| Mac     | Windows             |
| ------- | ------------------- |
| Command | Windows徽标键       |
| Option  | Alt键               |
| Delete  | 空格（Backspace）键 |
| Return  | 回车（Enter）键     |

替换Command和Control键后，常用快捷键对应如下（Space键即空格键，Win键即Windows徽标键）。

| Mac                           | Windows                | 作用                       |
| ----------------------------- | ---------------------- | -------------------------- |
| **常用**                      |                        |                            |
| Option                        |                        | 显示菜单隐藏项             |
| Command+C                     | Ctrl+C                 | 复制                       |
| Command+V‍                     | Ctrl+V                 | 粘贴                       |
| Command+Option+V              | Ctrl+Alt+V             | （Ctrl+C后）剪切           |
| Command+Delete                | Ctrl+Backspace         | 删除                       |
| Command+Z                     | Ctrl+Z                 | 撤销                       |
| Command+A                     | Ctrl+A                 | 全选                       |
| Command+F                     | Ctrl+F                 | 全选                       |
| Command+S                     | Ctrl+S                 | 保存                       |
| **截屏**                      |                        |                            |
| Option                        |                        | 维持比例                   |
| Control                       |                        | 保存截图到剪切板           |
| Command+Shift+3               | Ctrl+Shift+3           | 截取全部屏幕到文件         |
| Command+Shift+Control+3       | Ctrl+Shift+Win+3       | 截取全部屏幕到剪切板       |
| Command+Shift+4               | Ctrl+Shift+4           | 截取所选屏幕区域到一个文件 |
| Command+Shift+Control+4       | Ctrl+Shift+Win+4       | 截取所选屏幕区域到剪贴板   |
| Command+Shift+Control+Space+4 | Ctrl+Shift+Win+Space+4 | 仅捕捉一个窗到剪贴板       |
| **文件**                      |                        |                            |
| Control+Command+I             |                        | 查看多个文件简介           |
| Command+I                     |                        | 查看单个文件简介           |
| Command+Shift+N               | Ctrl+Shift+N           | 新建文件夹                 |
| Command+Shift+G               | Ctrl+Shift+G           | 输入文件路径               |
| Command+Up                    | Ctrl+Up                | 回到上级目录               |
| Command+Down                  | Ctrl+Down              | 进入下级目录（打开文件）   |
| Space                         | Space                  | 预览                       |
| Command+O                     | Ctrl+O                 | 打开所选文件               |
| Command+Shift+Delete          | Ctrl+Shift+Backspace   | 清倒废纸篓                 |

## 临时禁止通知中心弹出通知

按住`Option`后点击右上角通知中心图标即可。

## 把PNG转成JPG

直接修改后缀名即可。

## 触控板设置三指拖移

进入系统偏好设置-辅助功能-鼠标与触控板-`触控板选项…`，勾选`启用拖移`并选择`三指拖移`即可。

## 归类文件

选中多个文件，右键选择`使用多个文件新建文件夹`。

## 显示隐藏文件

利用`command（⌘）+shift+.`即可。

# 软件技巧

## 破解百度网盘客户端限速（新版已失效）

打开终端，输入下列命令以执行破解脚本。注意，只适用于2.2.2及以下的版本，2.2.3以后均加入了监测机制。

```
cd ~/Downloads && git clone https://github.com/CodeTips/BaiduNetdiskPlugin-macOS.git && ./BaiduNetdiskPlugin-macOS/Other/Install.sh
```

完成后打开百度云盘，此时要输入大概十次系统密码，进入云盘界面发现限速已经破解。下载时点击`免费试用`，倒计时将会保持在8秒。

## Mac版QQ功能扩展

下载以下仓库，解压后在终端运行Other文件夹中的`install.sh`即可。

```
https://github.com/TKkk-iOSer/QQPlugin-macOS/tree/master
```

## Mac版微信功能扩展

在终端输入以下命令即可。

```
curl -o- -L https://raw.githubusercontent.com/lmk123/oh-my-wechat/master/install.sh | bash -s
```

项目地址如下。

```
https://github.com/MustangYM/WeChatExtension-ForMac
```

安装扩展后可以多开微信，在命令行输入以下命令即可。

```
open -n /Applications/WeChat.app
```

## Quick Look插件

通过安装Quick Look插件，可以在点击空格预览的时候有更多功能。以下链接为Quick Look插件库，在终端通过`brew cask install [插件名]`进行安装。常用插件有qlcolorcode、qlstephen、qlmarkdown、quicklook-json、qlimagesize、suspicious-package等。

```
https://github.com/sindresorhus/quick-look-plugins
```

## Xcode SDK文件

用于为Xcode提供旧版SDK文件。

```
https://github.com/phracker/MacOSX-SDKs
```

# 终端命令

macOS中用`\40`代表空格。

## 获取开机日志

```
log show --predicate "processID == 0" --start $(date "+%Y-%m-%d") --debug > ~/Desktop/bootlog.txt
```

## 应用多开

```
open -n /Applications/xxxx.app
```

## 测量系统放电瓦数

若想保留此函数的功能，则需将此函数写入`~/.profile`中。

```
// 将以下函数直接复制到终端运行
watts() {
  FORMULA=$(system_profiler SPPowerDataType | awk '/Amperage\ \(mA\):/ {printf $3" * "}; /Voltage\ \(mV\):/ {print $3}')
  WATTS=$(echo "scale=3; ${FORMULA} / 1000000" | bc 2>/dev/null)
  if ! [[ ${WATTS} =~ [0-9] ]];
  then
    WATTS=0
  fi
  echo "${WATTS} mW"
}

// 使用
watts
```

## 终端的电源活动监视器

若想保留此函数的功能，则需将此函数写入`~/.profile`中。

```
// 创建函数
powertop() {
  top -stats pid,command,power -o power
}

// 调用
powertop
```

## 查询CPU运行情况

```
sudo powermetrics --show-process-energy
```

## 把安装应用调成任意来源

```
sudo spctl --master-disable
```

## 获取某磁盘的UUID

```
diskutil info diskXsY | grep -i "Partition UUID" | rev | cut -d' ' -f 1 | rev
```

## 取消4位数密码限制

```
pwpolicy -clearaccountpolicies
passwd
```

## Catalina挂载系统分区

```
sudo -S mount -uw / && killall Finder
```

## 查看睡眠时间

```
sysctl -a | grep sleeptime
```

## 查看睡眠唤醒时间

```
sysctl -a | grep waketime
```

## 查看最近一天睡眠唤醒原因

```
log show --last 1d | grep "Wake reason"
```

## 查看睡眠唤醒原因

```
pmset -g log | grep -Ei 'wake.*due'
```

## 检查AppleALC和Lilu是否工作正常

```
log show --predicate 'process == "kernel" AND (eventMessage CONTAINS "AppleALC" OR eventMessage CONTAINS "Lilu")' --style syslog –source
```

## 关闭系统更新提醒红点

```
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0
```

## 删除权限不足的文件

```
sudo rm -r -f [你要删除的文件（可以从Finder拖进来，支持多个）]
```

## 关机

```
sudo shutdown -h now
```

## 开启原生SSD Trim功能

先在Clover的启动参数添加rootless=0，然后终端输入以下命令并重启，重启后取消rootless参数。

```
sudo trimforce enable
```

## 关闭硬盘摔落保护功能

```
sudo pmset -a sms 0
```

## 一键查询硬件信息

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/daliansky/dell7000/master/Tools/archey)"
```

## 笔记本插电源出提示音

```
// 开启
defaults write com.apple.PowerChime ChimeOnAllHardware -bool true; open /System/Library/CoreServices/PowerChime.app &

// 关闭
defaults write com.apple.PowerChime ChimeOnAllHardware -bool false; killall PowerChime
```

## 手动安装kexts

```
// 删除重名/错误的kexts
sudo rm -Rf /Library/Extensions/KextToInstall.kext
sudo rm -Rf /System/Library/Extensions/KextToInstall.kext

// 把新kext移动到LE或SLE
sudo cp -R KextToInstall.kext /Library/Extensions
sudo cp -R KextToInstall.kext /System/Library/Extensions
```

## 手动修复权限和重建缓存

重建缓存可用以下终端命令，也可在磁盘工具中选择macOS磁盘并点击修复。

```
// 修复权限
sudo chmod -Rf 755 /S*/L*/E*
sudo chown -Rf 0:0 /S*/L*/E*
sudo chmod -Rf 755 /L*/E*
sudo chown -Rf 0:0 /L*/E*
sudo rm -Rf /S*/L*/PrelinkedKernels/*
sudo rm -Rf /S*/L*/Caches/com.apple.kext.caches/*

// 重建缓存（法一）
sudo touch -f /S*/L*/E*
sudo touch -f /L*/E*
sudo kextcache -Boot -U /

// 重建缓存（法二）
sudo kextcache -i /
```

## 让AirDrop支持有线传输

```
defaults write com.apple.NetworkBrowser BrowseAllInterfaces 1 && killall Finder
```

## 清空iCloud、iMassage和FaceTime缓存

```
cd ~/Library/Caches/
rm -R com.apple.Messages*
rm -R com.apple.imfoundation*
cd ~/Library/Preferences/

rm com.apple.iChat*
rm com.apple.icloud*
rm com.apple.ids.service*
rm com.apple.imagent*
rm com.apple.imessage*
 
rm com.apple.imservice*
rm -R ~/Library/Messages/
```

## 终端ftp工具

```
// 安装
brew install inetutils

// 使用
ftp
```

## 设置macOS电源相关

```
// 查看系统范围内的电源状态
pmset -g
pmset -g live

// 查看电源状态
pmset -g assertions

// 从日志查看电源状态
pmset -g log

// -a针对所有电源，-b针对电池电源，-c针对壁式充电器电源，-u以UPS电源为目标
// 休眠到内存（台式机默认设置）
sudo pmset -a hibernatemodes 0

// 休眠到硬盘
sudo pmset -a hibernatemodes 1

// 混合休眠（笔记本电脑默认配置）
sudo pmset -a hibernatemodes 3

// 省电休眠
sudo pmset -a hibernatemode 25

// 关闭powernap
sudo pmset -a powernap 0

// 关闭休眠
sudo pmset -a autopoweroff 0
sudo pmset -a standby 0

// 重置设置
pmset -a restoredefaults
```

下表为参数含义对照。

| 参数                               | 含义                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| proximitywake                      | 相同iCloud ID的设备靠近唤醒（1开启，0关闭）                  |
| disksleep                          | 设置不活动后磁盘停止旋转之前的时间，值以分钟为单位（默认10） |
| standby                            | 在系统已休眠一段时间后，此功能将从休眠转变为休眠状态（1开启，0关闭） |
| standbydelayhigh/standbydelaylow   | 将睡眠会话写入磁盘并关闭内存电源之前的延迟（以秒为单位）。当电池电量超过50%时，使用standbydelaylow，当电池电量低于50%时，使用standbydelaylow。对于台式机，这些值应相同 |
| highstandbythreshold               | 待机延迟的电池阈值（默认50%）                                |
| autopoweroff                       | 从休眠状态转换为关闭状态                                     |
| autopoweroffdelay                  | 计算机关闭之前的延迟（以秒为单位）                           |

## 清空bootercfg存储在NVRAM的变量

```
sudo nvram bootercfg="log=0 debug=0"
```

## 将逻辑卷转换为物理卷

```
sudo diskutil corestorage revert /
```

## 查看加载驱动

```
// 查看所有加载的驱动
kextstat

// 查看除苹果以外加载的驱动
kextstat | grep -v "com.apple"

// 查看加载的非官方驱动
kextstat | grep -v "com.apple" | grep -v "Energy"

// 查看特定名称的驱动（以ACPIplat为例）
kextstat | grep -y acpiplat
kextstat | grep -i acpiplat
```

## Finder显示完整文件路径

```
// 显示
defaults write com.apple.finder _FXShowPosixPathInTitle -bool TRUE && killall Finder

// 取消显示
defaults write com.apple.finder _FXShowPosixPathInTitle -bool FALSE && killall Finder
```

## 用Finder打开终端当前路径

```
open .
```

## 终端调用Chrome

```
// 打开Chrome浏览器
open -a Google\ Chrome --args -disable-web-security

// 直接打开网站
open www.baidu.com
```

## 屏蔽系统更新

```
sudo softwareupdate --ignore "macOS Catalina"
```

## 去掉应用的小红点提示

```
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0 && killall Dock
```

## 配置Safari的开发功能

或打开Safari后选择`偏好设置`，勾选`高级`选项卡下的`在菜单栏显示开发菜单`，也可达到相同效果。

```
// 开启
defaults write com.apple.Safari IncludeInternalDebugMenu 1

// 关闭
defaults write com.apple.Safari IncludeInternalDebugMenu 0
```

## 制作Mac安装盘

以Sierra安装包为例。

```
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/Sierra --applicationpath /Applications/Install\ macOS\ Sierra.app --nointeraction
```

# 常用APP

## 破解APP库

```
https://github.com/catofmrlu/MacApps
https://github.com/hemanth/awesome-pwa
http://mac-torrent-download.net
https://cmacapps.com
https://xclient.info/
https://www.waitsun.com/
https://www.macbl.com/
```

## 划词翻译

### Bob

```
https://github.com/ripperhe/Bob
```

## CaptuocrToy

OCR识别软件。

```
https://github.com/gragrance/CaptuocrToy/releases
```

## iStat Menus 6

用于性能监控。

```
// 下载地址
https://bjango.com/mac/istatmenus/

// 注册信息
Email: 982092332@qq.com
SN: GAWAE-FCWQ3-P8NYB-C7GF7-NEDRT-Q5DTB-MFZG6-6NEQC-CRMUD-8MZ2K-66SRB-SU8EW-EDLZ9-TGH3S-8SGA
```

## Simple Display Rotation Wrapper

屏幕旋转。

```
https://github.com/fewtarius/displayrotation
```

## Scroll Reverser

只改变鼠标滚轮方向而不改变触控板滑动方向，在终端输入以下命令安装。此APP会让三指点击失效。

```
brew cask install scroll-reverser
```

## DPCIManager

查看硬件信息。

```
https://sourceforge.net/projects/dpcimanager/files/latest/download
```

## DarwinDumper

输出硬件信息。

```
https://bitbucket.org/blackosx/darwindumper/downloads/
```

## Geekbench

跑分软件。

```
https://www.geekbench.com/
```

## Intel Power Gadget

查看CPU状态。

```
https://software.intel.com/en-us/articles/intel-power-gadget
```

## Hex Fiend

Hex编辑器。

```
http://ridiculousfish.com/hexfiend/
```

## IOJones

IORegistryExplorer的替代品。

```
https://sourceforge.net/projects/iojones/
```

## Heaven

显卡性能测试软件。

```
https://benchmark.unigine.com/heaven
```

## Novabench

跑分软件。

```
https://novabench.com/download#personal
```

## ShowHiddenFiles

显示/隐藏文件。

```
https://gotoes.org/sales/ShowHiddenFilesMacOSX/How_To_Show_Hidden_Files.php
```

## Trim Enabler

为SSD打开Trim功能。

```
https://cindori.org/trimenabler/
```

## OnyX

功能全面的系统清理工具。

```
https://www.titanium-software.fr/en/onyx.html
```

## Pacifist

安装包文件提取工具。

```
https://www.zhinin.com/pacifist-mac.html
```

注册码如下。

```
Name / Tom Smith
Serial number / 4Ip8izH03A-0K48zlp3zw-1Sp
```

## VOX

音乐播放软件。

```
https://vox.rocks/mac-music-player/download
```

## EFI MountianShow

挂载EFI分区。

```
https://www.tonymacx86.com/threads/updated-efi-mounter-v3-renamed-to-efi-mountianshow.210413/
```

## RAMDisk Utility

生成内存缓存盘，程序在其中运行速度可明显提升。

```
https://www.firewolf.science/2015/06/ramdisk-utility-for-os-x/
```

## CUDA-Z

查看Nvidia驱动。

```
https://sourceforge.net/projects/cuda-z/
```

## TotalFinder

Finder的增强。

```
http://www.pc6.com/mac/113433.html
```

## AJA System Test

测试磁盘速度。

```
https://macdownload.informer.com/aja-system-test/download/
```

## Touchbar Pet

Touchbar养宠物。

```
https://www.cr173.com/mac/1100456.html
```

## App Store自带APP

### MachineProfile

查询电脑信息。

### FileMerge

安装Xcode后即可搜索到，用于比较代码差异，常用于编写SSDT时比较打补丁前后的DSDT差异。

# python 3

Mac 11.0已自带python3。

Mac 10.15以下自带python 2，然而编程通常使用python 3。安装`python 3`和`pycharm`，以配置python环境。

## 安装python 3

### 通过pyenv

```
brew install pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
echo 'export LDFLAGS="${LDFLAGS} -L/usr/local/opt/zlib/lib"' >> ~/.zshrc
echo 'export CPPFLAGS="${CPPFLAGS} -I/usr/local/opt/zlib/include"' >> ~/.zshrc
echo 'export PKG_CONFIG_PATH="${PKG_CONFIG_PATH} /usr/local/opt/zlib/lib/pkgconfig"' >> ~/.zshrc
source ~/.zshrc
exec "$SHELL"
brew install openssl readline sqlite3 xz zlib
pyenv install -l
pyenv install [版本号]
```

### 旧方法（不推荐）

查看系统自带的python版本。

```
python -V
```

用Homebrew安装python 3。

```
brew install python3
```

查看python 3位置，并记录。

```
which python3
```

打开配置文件。

```
// 以下二选一
（终端shell为zsh）
vi ~/.zshrc

（终端shell为bash）
vi ~/.bash_profile
```

在文件中添加下面语句，保存并退出。

```
// 路径改为上面记录的python 3路径
alias python="/usr/local/bin/python3"
```

编译系统配置文件。

```
// 以下二选一
（终端shell为zsh）
source ~/.zshrc

（终端shell为bash）
source ~/.bash_profile
```

查看系统python版本，应已改为python 3。

```
python -V
```

## pip

pip是python的安装包工具。

### 安装

在终端输入以下命令即可。

```
sudo easy_install pip
```

### 卸载

在终端输入以下命令即可。

```
sudo pip uninstall pip
```

## 下载加速

### 暂时加速

```
pip install [包名] -i [镜像源地址]
```

### 直接换源

```
pip install pip -U
pip config set global.index-url https://pypi.douban.com/simple/
```

### 源地址

```
https://pypi.tuna.tsinghua.edu.cn/simple/
http://pypi.mirrors.ustc.edu.cn/simple/
http://mirrors.aliyun.com/pypi/simple/
https://pypi.mirrors.ustc.edu.cn/simple/
http://pypi.douban.com/simple/
```

## 插件安装

### Frida

到以下网站下载Frida的安装包并放到用户目录，为egg格式。

```
https://pypi.org/project/frida/#files
```

打开终端并输入以下命令安装，其中路径为刚才下载的安装包。

```
easy_install [路径]
```

### ItChat

#### 【额外了解】

与微信连接，参考教程如下。

```
https://www.playpi.org/2019020701.html
```

# proxychains-ng

## 安装

Mac下使用Homebrew安装。

```
brew install proxychains-ng
```

Linux下编译安装。

```
./configure --prefix=/usr --sysconfdir=/etc
sudo make
sudo make install
sudo make install-config
```

## 配置

在终端输入以下命令。

```
// Mac用户
vim /usr/local/etc/proxychains.conf

// Linux用户
vim /etc/proxychains.conf
```

在`[ProxyList]`下注释掉原来的代理，并添加代理类型，如

```
socks5 127.0.0.1 9050
```

如果所在的网络很复杂，可能需要在配置文件中启用`dynamic_chain`，然后在[ProxyList]下添加多个代理。

```
// dynamic_chain
按照列表中出现的代理服务器的先后顺序组成一条链，如果有代理服务器失效，则自动将其排除，但至少要有一个是有效的

// strict_chain（默认）
按照后面列表中出现的代理服务器的先后顺序组成一条链，要求所有的代理服务器都是有效的
```

在`~/.zshrc`末尾添加如下内容并保存。

```
#   ---------------------------------------
#   proxychain-ng config
#   ---------------------------------------
alias pc='proxychains4'
```

## 使用

```
pc curl http://www.google.com
```

# 参考教程

## pyenv

```
https://github.com/pyenv/pyenv
https://github.com/pyenv/pyenv-installer
https://github.com/jiansoung/issues-list/issues/13
```
