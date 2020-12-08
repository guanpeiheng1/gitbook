---
title: Windows使用技巧总结
categories: Technology
abbrlink: Windows-Skills
date: 2020-05-04 12:55:29
tags:
---

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gegayckavoj31be0qot9i.jpg)

Windows相关技巧。

<!-- more -->

# 常见问题

## 分区表错误导致硬盘无法识别

进入DOS版的Diskgenius并修复分区表即可。注意，WinPE的Diskgenius无效。

## 删除文件显示「访问被拒绝」

对于XP系统，需要先选择菜单栏的`工具`，点击`文件夹选项`，切换到`查看`选项卡，取消勾选`使用简单文件共享`后确定。在需要操作的文件上右键选择`属性`，切换到`安全`选项卡，点击`高级`，切换到`所有者`选项卡，选择`Administrators`并勾选`替换子容器及对象的所有者`，确定即可。

如果仍然不行，则在`安全`选项卡上点击`添加`，选择`高级`，点击`立即查找`，然后一路`确定`即可。

## Windows 10下忘记密码

准备Windows 10安装U盘，从U盘启动后点击下一步-修复计算机-疑难解答-命令提示符。用`dir`命令查看系统盘符（假设为C盘），然后输入以下命令。

```
C:
cd Windows/System32
ren sethc.exe AAA.exe
ren cmd.exe sethc.exe
```

重启到登录界面，按五次Shift进入命令行，输入以下命令以启用内置管理员账户并重置密码。

```
net user Administrator /active:yes 
net user Administrator [新密码]
```

进入系统后将Windows/System32下的sethc.exe重命名为cmd.exe，AAA.exe重命名为sethc.exe。

## XP系统无法访问/拒绝访问文件夹

以管理员身份登录，打开资源管理器，选择菜单工具-文件夹选项，单击`查看`选项卡，取消勾选`使用简单共享`。右键单击不能打开的文件夹，从弹出菜单中选择`属性`，单击安全-高级，选择`将所有者更改为下方的用户`，选中下方的`替换子容器及对象的所有者`，单击`应用`。单击权限-添加，在`选择用户或组`中单击高级-立即查找。选择要给予权限的用户，连续单击两次`确定`，直到出现`权限项目`对话框。在`权限`中的`完全控制`右侧选择`允许`，连续单击确定直到关闭所有对话框即可。

# 系统技巧

## 获取超级管理员权限

将以下内容保存为reg文件后运行，在右键菜单中即出现超级管理员的菜单。

```
Windows Registry Editor Version 5.00

[-HKEY_CLASSES_ROOT\*\shell\runas]

[HKEY_CLASSES_ROOT\*\shell\runas]
@="获取超级管理员权限"
"Icon"="C:\\Windows\\System32\\imageres.dll,-78"
"NoWorkingDirectory"=""

[HKEY_CLASSES_ROOT\*\shell\runas\command]
@="cmd.exe /c takeown /f \"%1\" && icacls \"%1\" /grant administrators:F"
"IsolatedCommand"="cmd.exe /c takeown /f \"%1\" && icacls \"%1\" /grant administrators:F"

[-HKEY_CLASSES_ROOT\Directory\shell\runas]

[HKEY_CLASSES_ROOT\Directory\shell\runas]
@="获取超级管理员权限"
"Icon"="C:\\Windows\\System32\\imageres.dll,-78"
"NoWorkingDirectory"=""

[HKEY_CLASSES_ROOT\Directory\shell\runas\command]
@="cmd.exe /c takeown /f \"%1\" /r /d y && icacls \"%1\" /grant administrators:F /t"
"IsolatedCommand"="cmd.exe /c takeown /f \"%1\" /r /d y && icacls \"%1\" /grant administrators:F /t"

[-HKEY_CLASSES_ROOT\dllfile\shell]

[HKEY_CLASSES_ROOT\dllfile\shell\runas]
@="获取超级管理员权限"
"HasLUAShield"=""
"NoWorkingDirectory"=""

[HKEY_CLASSES_ROOT\dllfile\shell\runas\command]
@="cmd.exe /c takeown /f \"%1\" && icacls \"%1\" /grant administrators:F"
"IsolatedCommand"="cmd.exe /c takeown /f \"%1\" && icacls \"%1\" /grant administrators:F"

[-HKEY_CLASSES_ROOT\Drive\shell\runas]

[HKEY_CLASSES_ROOT\Drive\shell\runas]
@="获取超级管理员权限"
"Icon"="C:\\Windows\\System32\\imageres.dll,-78"
"NoWorkingDirectory"=""

[HKEY_CLASSES_ROOT\Drive\shell\runas\command]
@="cmd.exe /c takeown /f \"%1\" /r /d y && icacls \"%1\" /grant administrators:F /t"
"IsolatedCommand"="cmd.exe /c takeown /f \"%1\" /r /d y && icacls \"%1\" /grant administrators:F /t"
```

若要取消该菜单，则用以下reg。

```
Windows Registry Editor Version 5.00

[-HKEY_CLASSES_ROOT\*\shell\runas]

[-HKEY_CLASSES_ROOT\Directory\shell\runas]

[-HKEY_CLASSES_ROOT\dllfile\shell]

[-HKEY_CLASSES_ROOT\Drive\shell\runas]

[-HKEY_CLASSES_ROOT\exefile\shell\runas]

[HKEY_CLASSES_ROOT\exefile\shell\runas]
"HasLUAShield"=""

[HKEY_CLASSES_ROOT\exefile\shell\runas\command]
@="\"%1\" %*"
"IsolatedCommand"="\"%1\" %*"
```

## 虚拟Wi-Fi功能

适用于Win7及以上系统。

```
https://hoochanlon.github.io/fq-book/#/append/win7-wifi
https://www.connectify.me/
```

## 设置Hyper-V

Hyper-V为Windows自带的虚拟机软件。打开控制面板，选择程序和功能-启用或关闭Windows功能，勾选`Hyper-V`，确定并安装即可。

## 设置Linux子系统

### 安装

进入系统设置，点击更新和安全-针对开发人员，选择`开发人员模式`。打开控制面板，选择程序和功能-启用或关闭Windows功能，勾选`适用于Linux的Windows子系统`，确定并安装。打开Windows应用商店，搜索`Linux`，安装并打开Ubuntu。注意需要经常在命令行界面敲回车键，以确认其安装情况。

### 基本配置

直接打开上述安装好的应用即可，也可打开命令行并输入`bash`。输入以下命令以重置root用户的密码。

```
sudo reboot
```

依次输入以下命令以备份并打开源配置文件。

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup
sudo vim /etc/apt/sources.list
```

在vim中输入以下命令以替换为阿里云的源地址，并保存退出。

```
:%s#deb http://archive.ubuntu.com/ubuntu/#deb http://mirrors.aliyun.com/ubuntu/#g
:%s#deb http://security.ubuntu.com/ubuntu/#deb http://mirrors.aliyun.com/ubuntu/#g
:wq
```

输入以下命令以更新配置。

```
apt-get update
```

### 更新为WSL2

Windows 10 19041及更高版本可进行此更新。

打开以下链接并点击Linux内核更新包，下载并安装。

```
https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-kernel
```

以管理员身份打开PowerShell，执行以下命令并重启即可。

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl.exe --set-version Ubuntu 2
wsl.exe --set-default-version 2
```

### 图形界面

#### 对于WSL1

##### 安装

打开终端并输入以下命令以安装。

```
sudo apt-get update
sudo apt-get install xorg
sudo apt-get install xfce4
sudo apt-get install xrdp
```

##### 使用

打开终端并输入以下命令。

```
sudo sed -i 's/port=3389/port=3390/g' /etc/xrdp/xrdp.ini
sudo echo xfce4-session >~/.xsession
sudo service xrdp restart
```

在Windows打开`远程桌面连接`，计算机名称为`[本机IP]:3390`，登录即可。注意图形界面运行时需保证终端运行，且`xrdp`服务处于开启状态。

#### 对于WSL2

##### 安装

打开终端并输入以下命令。

```
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install tasksel -y
sudo tasksel
```

选择`Ubuntu Desktop`，然后用Tab键选择OK，回车确定。如果突然退出，遇到`tasksel: apt-get failed (100)`，这时要重新运行上面最后一条命令，并且重新选择`Ubuntu Desktop`。安装完成会出现`xserver-xorg install`。

继续输入以下命令。

```
sudo apt-get install -y tigervnc-standalone-server
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install -y dotnet-runtime-3.1
sudo apt-get install -y lightdm
sudo dpkg-reconfigure lightdm
```

选择LightDM并保存。然后输入以下命令，不用设置view-only password。

```
vncpasswd
sudo vncpasswd
sudo -u lightdm vncpasswd
// 如果窗口管理器选择的是GDM，则输入sudo -u gdm vncpasswd
sudo mv /usr/bin/Xorg /usr/bin/Xorg_old
sudo vim /usr/bin/Xorg
```

将以下内容写入Xorg文件。

```
#!/bin/bash
for arg do
  shift
  case $arg in
    # Xvnc doesn't support vtxx argument. So we convert to ttyxx instead
    vt*)
      set -- "$@" "${arg//vt/tty}"
      ;;
    # -keeptty is not supported at all by Xvnc
    -keeptty)
      ;;
    # -novtswitch is not supported at all by Xvnc
    -novtswitch)
      ;;
    # other arguments are kept intact
    *)
      set -- "$@" "$arg"
      ;;
  esac
done

# Here you can change or add options to fit your needs
## command=("/usr/bin/Xvnc" "-geometry" "2736x1824" "-PasswordFile" "${HOME:-/root}/.vnc/passwd" "$@") 
# Because my host monitor resolution matches 1440x860
command=("/usr/bin/Xvnc" "-geometry" "1440x860" "-PasswordFile" "${HOME:-/root}/.vnc/passwd" "$@") 

systemd-cat -t /usr/bin/Xorg echo "Starting Xvnc:" "${command[@]}"

exec "${command[@]}"
```

保存并退出后继续输入以下命令。

```
sudo chmod 0755 /usr/bin/Xorg
curl -s https://packagecloud.io/install/repositories/arkane-systems/wsl-translinux/script.deb.sh | sudo bash
sudo apt-get install -y systemd-genie
sudo mkdir /usr/lib/genie/
sudo vim /usr/lib/genie/deviated-preverts.conf
```

将以下内容写入文件，保存并退出。

```
{
  "daemonize": "/usr/bin/daemonize"
}
```

继续执行以下命令。

```
genie -s
sudo vim /etc/lightdm/lightdm.conf
```

写入以下内容，保存并退出。

```
[LightDM]
start-default-seat=false

[XDMCPServer]
enabled=true
port=177
```

继续执行以下命令。

```
sudo service lightdm restart
```

##### 使用

打开以下链接以下载VcXsrv Windows X Server。

```
https://sourceforge.net/projects/vcxsrv/
```

连接前先在Linux的终端输入以下命令。

```
genie -s
hostname -I | awk '{print $1}'
```

记住输出的IP地址。打开VcXsrv，选择One window without titlebar后点击下一步，然后选择Open session via XDMCP后点击下一步。在Connect to host输入框中输入刚才的吧IP地址后点击下一步。选择Clipboard和Disable access control，取消选择Native opengl，然后点击下一步，完成即可。

## 共享

### 设置打印机共享

在连接有打印机的主机上设置打印机为共享。

在需要连接共享打印机的电脑上打开控制面板，选择`程序和功能`-`启用或关闭Windows功能`，勾选`打印与文件服务`、`SMB 1.0/CIFS文件共享支持`，确定并安装。然后打开设备管理器，对于较老的打印机，需点击`操作`-`添加过时硬件`，选择手动从列表选择，找到打印机并点击下一步。然后选择`创建新端口`，端口类型为`Local Port`，端口名称为`\\[IP地址]\[打印机名称]`。添加后选择`从磁盘安装`，选择驱动程序中的INF文件后，在列表中点击对应打印机的型号，完成安装即可。

### 访问Mac共享

在Mac打开设置，选择`共享`-`文件共享`，点击`选项`，勾选两个复选框，并勾选用于共享的用户。回到设置页面，打开`网络偏好设置`，选择活跃的连接，点击`高级`-`WINS`，输入Windows的工作组名称，一般为WORKGROUP。在Windows端打开运行，输入`\\[Mac的IP地址]`即可访问。

## Win 9.x内核进入DOS

### Windows 98

启动时按Ctrl。

### Windows 95 / ME

启动时按F8。

## 开启卓越模式电源计划

打开Powershell，执行以下命令即可。

```
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
```

## XP系统制作CAB压缩/自解压缩包

在运行输入IExpress，选择Create new Self Extraction Directive file，然后选择打包类型后添加文件即可。


## 删除「添加或删除程序」里的无用选项

Win+R调出运行框，输入regedit，定位到HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall，将要删除的软件对应的键删除即可。

## 取消按Ctrl+Alt+Del登录

进入控制面板，双击管理工具，选择本地安全策略，点击`安全设置`-`本地策略`-`安全选项`，在右侧选择`交互式登录：无须按Ctrl+Alt+Del`，启用即可。

## 开启自动登录

Win+R打开`运行`，输入`control userpasswords2`后回车，取消勾选`要使用本机，用户必须输入用户名和密码`即可。

## 批量安装补丁

在cmd下输入补丁的完整路径，并在其后加上`/q`参数即可。加`/?`参数能够查看补丁支持的参数选项，对于大部分补丁而言，也可使用`/passive /norestart`。

## 运行库安装

提示运行库缺失时，可通过AiO Runtimes一键安装。

```
// Windows 2000/XP
https://pan-yz.chaoxing.com/external/m/file/337329473334308864

// Windows Vista及以上
https://pan-yz.chaoxing.com/external/m/file/337350294375264256
```

## DOS常用命令

### doskey命令

为命令提供别名。

```
// 定义
doskey a=echo hello

// 使用
a // 显示hello
```

## 停止Windows 10自动更新

### 一般方法

卸载升级相关软件后，打开系统盘并删除以下文件。

```
C:\$*
C:\Windows10Update
C:\Windows\UpdateAssistant*
C:\Windows\SoftwareDistribution\Download\*
```

打开以下链接，下载Control Panel Plus后打开并取消Windows更新即可。

```
https://www.lanzoux.com/iUuXtez2fde
```

### 把网络设为按流量计费的连接

打开网络和Internet属性，选择更改连接属性，打开`设为按流量计费的连接`即可。

### 禁止系统自动下载更新程序

在此电脑上右键点击属性，选择高级系统设置-硬件吧-设备安装设置，在`是否要自动下载适合你设备的制造商应用和自定义图标`选择否即可。

# 操作技巧

## 快速打开网站

进入浏览器快捷方式属性，在目标后空一格并输入网址即可。

## 批量修改后缀名

复制以下代码到记事本，并另存为bat文件，使用时双击打开即可，注意，本bat文件对文件夹内的所有文件均适用。

```
// 修改gif为jpg
ren *.gif *.jpg

// 修改所有文件为jpg
ren *.* *.jpg
```

# 软件技巧

## Google Chrome

### 打开暗黑模式

在Chrome快捷方式上右键点击属性，在`目标`的后面添加`--force-dark-mode`即可。

### 打开实验功能

在Chrome的地址栏输入以下链接打开实验功能页。

```
chrome://flags
```

常用功能如下。

```
// Overlay Scrollbars
自动收起滚动条

// Flash Overlay Scrollbars When Mouse Enter
滚动条跟随鼠标移动出现

// Smooth Scrolling
平滑滚动

// Tab Hover Cards
悬停标签页显示网址

// Desktop PWAs
桌面版Chrome支持PWA应用

// Parallel downloading
提升下载速度
```

## 百度云盘加速

### KinhDown

```
http://kinhdown.kinh.cc
```

### bdmaster

```
https://www.lanzous.com/b825731/
```

### 网盘直链下载助手

```
https://www.baiduyun.wiki/
```

### PDown

```
https://wwa.lanzous.com/i8sIMdl40be
https://github.com/pdown2020/pdown
```

### Firefox+IDM

#### 直接安装

```
// 32位浏览器
https://lanzous.com/ibp84ud

// 64位浏览器
https://lanzous.com/ibp87vc

// idm直装版
https://lanzous.com/ibp884b
```

#### 手动安装

在Chrome中安装好Tempermonkey，然后安装以下脚本。

```
https://greasyfork.org/zh-CN/scripts/378301-%E7%BD%91%E7%9B%98%E5%8A%A9%E6%89%8B
```

### 度盘速下

需要在IDM中设置下载任务时使用的用户代理为`LogStatistic`。

```
http://byu5.cn/baiduwp/
```

### 雷鸟下载（暂时失效）

```
https://thunderbird.lanzous.com/b01bdspaj
```

## Scoop

类似Homebrew的软件管理器。打开PowerShell并运行以下命令以执行安装。

```
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
# 或
iwr -useb get.scoop.sh | iex
```

执行以下命令安装aria2以提升下载速度。

```
scoop install aria2
```

常用命令如下。

| 命令                                   | 说明                 |
| -------------------------------------- | -------------------- |
| scoop help                             | 帮助                 |
| scoop install [软件名]                 | 安装软件             |
| scoop install [软件名]@[版本号]        | 安装制定版本软件     |
| scoop install -g [软件名]              | global目录下安装软件 |
| scoop bucket add [软件库名] [源地址]   | 添加软件库           |
| scoop update                           | 更新scoop和软件库    |
| scoop update *                         | 更新所有已安装应用   |
| scoop cleanup *                        | 移除所有旧版本       |
| scoop uninstall [软件名]               | 卸载                 |
| scoop list > F:/scoop/ScoopAppList.txt | 导出软件列表         |
| scoop reset [软件名]                   | 版本切换             |
|                                        |                      |

软件库推荐如下。

```
scoop bucket add main # 默认
scoop bucket add extras # 推荐
scoop bucket add versions
scoop bucket add nightlies
scoop bucket add nirsoft
scoop bucket add php
scoop bucket add nerd-fonts
scoop bucket add nonportable
scoop bucket add java
scoop bucket add games
scoop bucket add jetbrains # 推荐
scoop bucket add echo https://github.com/echoiron/echo-scoop
scoop bucket add dorado https://github.com/chawyehsu/dorado
scoop bucket add dodorz https://github.com/dodorz/scoop-bucket
```

## Office

### 激活

#### 激活方法

主流的激活工具均为KMS激活，会安装KMS密钥。通过Kmspico或Microsoft Toolkit可以激活Office 2010。

由于Office安装完成后带有Retail密钥，因此KMS密钥安装完成后，如果无法激活，则需先删除Retail密钥。

#### 激活密钥

##### Office 97

```
4657-1931151
1921701-05354
8089-3069692
9990-1456507
9978-1234567
4156-0287065
415-0151383
2978-0029987
425-1921701
103-0145657
102-0128344
14080-000-4203561
28779-051-0101444
27716-102-0128344
111-1111111
167649998411
4657-1931151
1921701-05354
123-1111111
123-444444
```

##### Office 2003

```
GWH28-DGCMP-P6RC4-6J4MT-3HFDY
XVG79-Q2WK3-JRPMD-9H26V-7TBYT
BY3TG-48BRP-VTT2Y-YH84X-TYJ97
J2MV9-JYYQ6-JM44K-QMYTH-8RB2W
```

### 使用

#### Word分屏浏览

选择`视图`选项卡，点击`拆分窗口`即可。

#### 通配符

##### 选取包含特定字符的一行

将手动换行符变为换行符，即^l全部替换为^p。然后查找与替换中勾选`使用通配符`，查找内容填写`[特定字符](*)^13`（[特定字符]替换为对应内容），搜索范围为`全文档`即可。

# 常用工具

## 文件传输

### Dukto

可使同一局域网下的电脑传输文件，传输双方的系统类型可以不同。

```
https://www.52pojie.cn/thread-1086314-1-1.html
```

## 截图

### Snipaste

```
https://www.snipaste.com/
```

### ShareX

```
https://getsharex.com/
```

教程如下。

```
https://posts.careerengine.us/p/5ad5f9e21a02813ca670cce0
https://post.smzdm.com/p/698344/
https://www.xiaoz.me/archives/8132
```

### FSCapture

## 系统维护

### Dism++

需在Windows 7及以上系统使用。

```
http://www.chuyu.me/zh-Hans/
```

### 360系统急救箱独立版

```
http://www.xue51.com/soft/15383.html
```

### CGI备份还原

```
https://www.ranwenzw.com/xiazai/49240.html
```

### Revo Uninstaller

```
https://www.jb51.net/softs/654861.html
```

### CleanMyPC

```
https://macpaw.com/cleanmypc
https://www.lanzoux.com/ic9hf1e
https://www.lanzoux.com/ic9hhmh
```

### Wise Disk Cleaner

```
https://www.wisecleaner.com/wise-disk-cleaner.html
https://www.lanzoux.com/ic9oorg
```

### 一键系统优化加速工具

```
https://www.lanzoux.com/ic9onud
```

### 火绒弹窗拦截独立版

```
https://www.lanzoux.com/iqAkjgyw9vc
```

## 卸载工具

### Geek Uninstaller

```
https://www.lanzoux.com/ic9h9da
```

### Revo Uninstaller Pro

```
https://www.lanzoux.com/ic9h98f
```

### IObit Uninstaller

```
https://www.lanzoux.com/ic9ha6j
```

## 硬件配置

### AIDA64

```
https://www.aida64.com/downloads
```

### SiSoftware Sandra

```
https://www.sisoftware.co.uk/download-buy/
```

### everest

可用于配置较低的主机，在AIDA64打不开时可使用本软件。

```
https://www.onlinedown.net/soft/106556.htm
```

### CPU-Z

```
https://www.cpuid.com/
```

### GPU-Z

```
https://www.techpowerup.com/download/gpu-z/
```

### Fritz Chess Benchmark4

象棋跑分软件。

```
https://download.pchome.net/system/benchmark/download-3641.html
```

## 硬盘分区

### DiskGenius

```
https://www.diskgenius.cn/
```

## 压缩包解密工具

```
https://axu.lanzous.com/icbt77g
```

## 压缩工具

### Bandizip

破解补丁链接如下。

```
https://www.lanzous.com/ib29ludBand
https://ltribe.lanzous.com/iw8h3e30u0h
```

密钥如下。

```
// 企业版离线密钥
20380808-ENT000002-0E34A52561-166371E0
// 专业版离线密钥
20380808-PRO0BFAEBFDAE23C425E-173E2DF1
```

## 拷录工具

### UltraISO

注册码如下。

```
用户名 / 王涛
注册码 / 7C81-1689-4046-626F

用户名 / 累累
注册码 / 4EE9-A156-B015-A70E
```

### rufus

```
http://rufus.ie/
```

## 文本编辑工具

### WinHex

```
https://www.onlinedown.net/soft/1510.htm
```

### 010 Editor

```
http://www.pc0359.cn/downinfo/60574.html
```

### MarkdownPad 2

密钥如下。

```
// 邮箱
Soar360@live.com

// 授权秘钥
GBPduHjWfJU1mZqcPM3BikjYKF6xKhlKIys3i1MU2eJHqWGImDHzWdD6xhMNLGVpbP2M5SN6bnxn2kSE8qHqNY5QaaRxmO3YSMHxlv2EYpjdwLcPwfeTG7kUdnhKE0vVy4RidP6Y2wZ0q74f47fzsZo45JE2hfQBFi2O9Jldjp1mW8HUpTtLA2a5/sQytXJUQl/QKO0jUQY4pa5CCx20sV1ClOTZtAGngSOJtIOFXK599sBr5aIEFyH0K7H4BoNMiiDMnxt1rD8Vb/ikJdhGMMQr0R4B+L3nWU97eaVPTRKfWGDE8/eAgKzpGwrQQoDh+nzX1xoVQ8NAuH+s4UcSeQ==
```

## 本地搜索工具

### Everything

可将某目录映射到localhost。打开选项-HTTP服务器，勾选启用HTTP服务器，并选择服务器页面位置即可。

```
https://www.voidtools.com/zh-cn/
```

### Listary Pro

```
https://www.52pojie.cn/thread-821169-1-1.html
```

## 文字识别

### PandaOCR

```
https://github.com/miaomiaosoft/PandaOCR
```

### 天若OCR

```
https://tianruoocr.cn/
```

## 虚拟机

### Qemu Manager

```
https://www.cr173.com/soft/794471.html
```

## 密码破解工具

### Accent RAR

RAR文件密码破解工具。

```
http://www.uzzf.com/soft/118363.html
```

### ziprello

ZIP文件密码破解工具。

```
https://www.lanzous.com/i5mxkza
```

## 图片放大工具

### Waifu2x

```
https://github.com/AaronFeng753/Waifu2x-Extension-GUI/releases
```

### X-Mind

```
https://ltribe.lanzous.com/icUNjdo4o1g
```

## 终端美化工具

### FluentTerminal

带SSH功能。

```
https://github.com/felixse/FluentTerminal
```

## 网络调试工具

### PsPing

支持ICMP Ping、TCP Ping、网络延迟（TCP/UDP）、带宽测试（TCP/UDP）。

```
https://download.sysinternals.com/files/PSTools.zip
```

教程如下。

```
https://51.ruyo.net/15171.html
https://docs.microsoft.com/zh-cn/sysinternals/downloads/psping
https://blog.51cto.com/wujianwei/2274120
```

### TCPing

检测端口是否开启和TCP延迟的工具。

#### 安装

##### Windows

复制tcping.exe到C:\Windows\System32\即可。在命令行输入`tcping`以调用。

```
https://www.elifulkerson.com/projects/tcping.php
```

##### Mac

通过Homebrew安装即可。

```
brew install tcping
```

#### 教程

```
https://51.ruyo.net/14528.html
```

### FinalShell

一体化的服务器网络管理软件。

```
http://www.hostbuf.com/t/989.html
```

## 其它

### Unlocker

解除文件被占用的限制。

```
https://www.onlinedown.net/soft/24732.htm
```

### f.lux

添加夜间模式。

```
https://justgetflux.com/
```

### 蚂蚁笔记

```
https://leanote.com/
```

### 绿色程序制作

```
https://www.lanzoux.com/iMtkjfcey0f
```

### WizTree

磁盘文件分析。

```
https://www.lanzoux.com/iyl7Ig6ilxe
```

# 参考教程

## 安装好电驴插件后，怎么用来下ED2K

```
https://tieba.baidu.com/p/5381182810?red_tag=x2303093348&traceid=
```

## Windows 10下忘记密码的解决方案(本地账户，微软账户通杀)

```
https://www.bugprogrammer.me/2018/05/27/reset-password-for-windows-10.html
```

## Scoop软件包管理神器

```
https://www.limufang.com/post/569.html
```

## 从Ubuntu for WSL 1升级成WSL 2的命令及设WSL 2为默认版本

```
https://ywnz.com/linuxjc/7219.html
```

## WSL2下让Ubuntu20.04安装Gnome桌面

```
https://www.chengxuzhilu.com/2395.html
https://www.most-useful.com/ubuntu-20-04-desktop-gui-on-wsl-2-on-surface-pro-4.html
```
