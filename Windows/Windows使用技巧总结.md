---
title: Windows使用技巧总结
categories: Windows
abbrlink: Windows-Skills
date: 2020-05-04 12:55:29
tags:
---

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gegayckavoj31be0qot9i.jpg)

Windows相关技巧。

<!-- more -->

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

适用于Win7及以上系统。以管理员身份运行命令提示符，运行以下命令以启用虚拟无线网卡。

```
netsh wlan set hostednetwork mode=allow ssid=”Win7 AP WOW!” key=wifimima
```

打开网络与共享中心，进入适配器选项。如果在运行上述命令后没有出现虚拟无线网卡，就说明真实网卡不支持该功能。若出现该网卡，可继续以下操作。

选择已连接到互联网的网络，右键点击属性-共享，勾选`允许其他网络用户通过此计算机的Internet连接来连接`并选择刚才开启的虚拟网卡，同时勾选`允许其他网络用户控制或禁用贡献的Internet连接`。

继续在命令提示符运行以下命令以开启无线网络。开启后连接该无线网络即可。

```
netsh wlan start hostednetwork
```

## 设置Hyper-V

Hyper-V为Windows自带的虚拟机软件。打开控制面板，选择程序和功能-启用或关闭Windows功能，勾选Hyper-V，确定并安装即可。

## 设置Linux子系统

### 安装

进入系统设置，点击更新和安全-针对开发人员，选择开发人员模式。打开控制面板，选择程序和功能-启用或关闭Windows功能，勾选`适用于Linux的Windows子系统`，确定并安装。打开Windows应用商店，搜索Linux，安装并打开Ubuntu。注意需要经常在命令行界面敲回车键，以确认其安装情况。

完成安装后可直接通过打开应用启动Linux，也可打开命令行并输入`bash`。

### 基本配置

#### 更换软件源

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

#### 更新为WSL2

Windows 10 19041及更高版本可进行此更新。打开以下链接并点击Linux内核更新包，下载并安装。

```
https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-kernel
```

以管理员身份打开PowerShell，执行以下命令并重启即可。

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl.exe --set-version Ubuntu 2
wsl.exe --set-default-version 2
```

#### WSL1图形界面

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

#### WSL2图形界面

##### 安装

打开终端并输入以下命令。

```
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install tasksel -y
sudo tasksel
```

选择`Ubuntu Desktop`，然后用Tab键选择OK，回车确定。

如果突然退出，遇到`tasksel: apt-get failed (100)`，则需要重新运行上面最后一条命令，并且重新选择`Ubuntu Desktop`。安装完成会出现`xserver-xorg install`。

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
```

通过以下命令编辑Xorg文件。

```
sudo vim /usr/bin/Xorg
```

内容如下，保存并退出。

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
```

通过以下命令编辑deviated-preverts.conf文件。

```
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
```

通过以下命令编辑lightdm.conf文件。

```
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

连接前在Linux的终端输入以下命令。

```
genie -s
hostname -I | awk '{print $1}'
```

记住输出的IP地址。打开VcXsrv，选择One window without titlebar后点击下一步，然后选择Open session via XDMCP后点击下一步。在Connect to host输入框中输入刚才的IP地址后点击下一步。选择Clipboard和Disable access control，取消选择Native opengl，然后点击下一步，完成即可。

## 打印机共享

在连接有打印机的主机上设置打印机为共享。

在需要连接共享打印机的电脑上打开控制面板，选择程序和功能-启用或关闭Windows功能，勾选打印与文件服务、SMB 1.0/CIFS文件共享支持，确定并安装。然后打开设备管理器，对于较老的打印机，需点击操作-添加过时硬件，选择手动从列表选择，找到打印机并点击下一步。

然后选择创建新端口，端口类型为Local Port，端口名称为`\\[IP地址]\[打印机名称]`。添加后选择`从磁盘安装`，选择驱动程序中的INF文件后，在列表中点击对应打印机的型号，完成安装即可。

## 开启卓越模式电源计划

打开Powershell，执行以下命令即可。

```
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
```

## 删除「添加或删除程序」里的无用选项

Win+R调出运行框，输入regedit，定位到HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall，将要删除的软件对应的键删除即可。

## 登录相关

### 取消按Ctrl+Alt+Del登录

进入控制面板，双击管理工具，选择本地安全策略，点击安全设置-本地策略-安全选项，在右侧选择`交互式登录：无须按Ctrl+Alt+Del`，启用即可。

### 开启自动登录

Win+R打开`运行`，输入`control userpasswords2`后回车，取消勾选`要使用本机，用户必须输入用户名和密码`即可。

## 运行库安装

提示运行库缺失时，可通过AiO Runtimes一键安装。

```
// Windows 2000/XP
https://pan-yz.chaoxing.com/external/m/file/337329473334308864

// Windows Vista及以上
https://pan-yz.chaoxing.com/external/m/file/337350294375264256
```

## 停止自动更新

### 通过删除文件

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

### 通过把网络设为按流量计费的连接

打开网络和Internet属性，选择更改连接属性，打开`设为按流量计费的连接`即可。

### 通过禁止系统自动下载更新程序

在此电脑上右键点击属性，选择高级系统设置-硬件-设备安装设置，在`是否要自动下载适合你设备的制造商应用和自定义图标`选择否即可。

## 破解登录密码

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

## 桌面美化

可通过以下软件。

```
http://www.apumiao.com/
```

## 重置网络设置

打开命令行并输入以下命令即可。

```
netsh winsock reset(需要管理员身份，输完之后重启)
ipconfig/release
ipconfig/renew
ipconfig/displaydns
ipconfig/flushdns
```

# 操作技巧

## 快速打开网站

进入浏览器快捷方式属性，在目标后空一格并输入网址即可。

## 批量修改后缀名

复制以下代码到记事本，并另存为bat文件，使用时双击打开即可。注意，本bat文件对文件夹内的所有文件均适用。

```
// 修改gif为jpg
ren *.gif *.jpg

// 修改所有文件为jpg
ren *.* *.jpg
```

# 软件技巧

## Google Chrome

### 暗黑模式

在Chrome快捷方式上右键点击属性，在`目标`的后面添加`--force-dark-mode`即可。

### 实验功能

在Chrome的地址栏输入以下链接打开实验功能页。

```
chrome://flags
```

常用功能如下。

|                    项目                   |           含义          |
|-------------------------------------------|-------------------------|
| Overlay Scrollbars                        | 自动收起滚动条          |
| Flash Overlay Scrollbars When Mouse Enter | 滚动条跟随鼠标移动出现  |
| Smooth Scrolling                          | 平滑滚动                |
| Tab Hover Cards                           | 悬停标签页显示网址      |
| Desktop PWAs                              | 桌面版Chrome支持PWA应用 |
| Parallel downloading                      | 提升下载速度            |

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

### 雷鸟下载

暂时失效。

```
https://thunderbird.lanzous.com/b01bdspaj
```

## Scoop

类似Homebrew的软件管理器。打开PowerShell并运行以下命令以执行安装。

```
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
// 或
iwr -useb get.scoop.sh | iex
```

执行以下命令安装aria2以提升下载速度。

```
scoop install aria2
```

常用命令如下。

|                  命令                  |         说明         |
|----------------------------------------|----------------------|
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

软件库推荐如下。

```
scoop bucket add main // 默认
scoop bucket add extras // 推荐
scoop bucket add versions
scoop bucket add nightlies
scoop bucket add nirsoft
scoop bucket add php
scoop bucket add nerd-fonts
scoop bucket add nonportable
scoop bucket add java
scoop bucket add games
scoop bucket add jetbrains // 推荐
scoop bucket add echo https://github.com/echoiron/echo-scoop
scoop bucket add dorado https://github.com/chawyehsu/dorado
scoop bucket add dodorz https://github.com/dodorz/scoop-bucket
```

## Office

### 激活

#### 方法

主流的激活工具均为KMS激活，会安装KMS密钥。通过Kmspico或Microsoft Toolkit可以激活Office 2010。也可用KMS Activator、microKMS等。

由于Office安装完成后带有Retail密钥，因此KMS密钥安装完成后，如果无法激活，需先删除Retail密钥。

```
// KMSpico
https://www.pcsoft.com.cn/soft/41721.html

// KMS Activator
https://www.jb51.net/softs/116555.html

// Microsoft Toolkit
http://www.downza.cn/soft/187399.html

// microKMS
https://www.jb51.net/softs/451557.html
```

#### 密钥

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

#### 分屏浏览

选择视图-拆分窗口即可。

#### 特殊字符

|   特殊字符  |      含义      |
|-------------|----------------|
| ^?          | 任意单个字符   |
| ^#          | 任意单个数字   |
| ^$          | 任意英文字母   |
| ^p          | 段落标记       |
| ^l          | 手动换行符     |
| ^g或^1      | 图形           |
| ^+          | 1/4长划线      |
| ^j          | 长划线         |
| ^q          | 短划线         |
| ^t          | 制表符         |
| ^           | 脱字号         |
| ^v          | 分栏符         |
| ^b          | 分节符         |
| ^n          | 省略号         |
| ^i          | 全角省略号     |
| ^z          | 无宽非分隔符   |
| ^x          | 无宽可选分隔符 |
| ^s          | 不间断空格     |
| ^~          | 不间断连字符   |
| ^%          | 段落符号       |
| ^           | 分节符（§）    |
| ^f或^2      | 脚注标记       |
| ^-          | 可选连字符     |
| ^w          | 空白区域       |
| ^m          | 手动分页符     |
| ^e          | 尾注标记       |
| ^d          | 域             |
| ^Unnnn      | Unicode字符    |
| ^u8195      | 全角空格       |
| ^32或^u8194 | 半角空格       |
| ^a或^5      | 批注           |

#### 通配符

要查找已被定义为通配符的字符，需该字符前键入反斜杠`\`。

如果使用了通配符，在查找文字时会大小写敏感。如果希望查找大写和小写字母的任意组合，需要使用方括号通配符。如输入`[Hh]*[Tt]`可找到heat、Hat或HAT，而用`H*t`找不到heat。

使用通配符时，Word只查找整个单词。如搜索e*r可找到enter，但不会找到entertain。如输入`<(e*r)`可找到enter和entertain。

在查找图形时，Word只查找嵌入图形，而不能查找浮动图形。在默认情况下，Word会将导入的图形以嵌入图形的方式插入到文档中。

如果包含可选连字符代码，Word只会找到在指定位置带有可选连字符的文字。如果省略可选连字符代码，Word将找到所有匹配的文字，包括带有可选连字符的文字。

##### 列表

|      通配符      |           含义          |
|------------------|-------------------------|
| ?                | 任意单个字符            |
| [ - ]            | 指定范围内任意单个字符  |
| [0-9]            | 任意单个数字            |
| [.0-9]           | 任意单个带小数点数字    |
| [!0-9]           | 所有非数字字符          |
| [a-zA-Z]         | 任意英文字母            |
| [a-z]            | 任意全小写英文字母      |
| [A-Z]            | 任意全大写英文字母      |
| [^1-^127]        | 所有西文字符            |
| [!^1-^127]       | 所有中文汉字和中文标点  |
| [一-龥]或[一-﨩] | 所有中文汉字            |
| [!一-龥^1-^127]  | 所有中文标点            |
| [!x-z]           | 指定范围外任意单个字符  |
| *                | 任意字符串              |
| @                | 1个以上前一字符或表达式 |
| { n }            | n个前一字符或表达式     |
| { n, }           | n个以上前一字符或表达式 |
| { n,m }          | n到m个前一字符或表达式  |
| ^t               | 制表符                  |
| ^s               | 不间断空格              |
| ^13              | 段落标记                |
| ^l或^11          | 手动换行符              |
| ( )              | 表达式                  |
| <                | 单词开头                |
| < >              | 单词开头/结尾           |
| ^g               | 图形                    |
| ^q               | 1/4长划线               |
| ^+               | 长划线                  |
| ^=               | 短划线                  |
| ^^               | 脱字号                  |
| ^n或^14          | 分栏符                  |
| ^m               | 分节符/分页符           |
| ^i               | 省略号                  |
| ^j               | 全角省略号              |
| ^z               | 无宽非分隔符            |
| ^x               | 无宽可选分隔符          |
| ^~               | 不间断连字符            |
| ^=               | 短划线                  |
| ^%               | 分节符（§）             |

##### 常用示例

###### 选取包含特定字符的一行

将手动换行符变为换行符，即^l全部替换为^p。查找与替换中勾选使用通配符，查找内容填写`[内容](*)^13`，搜索范围为全文档即可。

# 常用工具

## 文件传输

### Dukto

可使同一局域网下的电脑传输文件，传输双方的系统类型可以不同。

```
https://www.52pojie.cn/thread-1086314-1-1.html
```

## 截图

### Snipaste

有贴图功能。

```
https://www.snipaste.com/
```

### ShareX

```
https://getsharex.com/
```

### FSCapture

```
https://dl.pconline.com.cn/download/409863.html
```

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
https://www.jb51.net/softs/654861.html
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

查看CPU配置。

```
https://www.cpuid.com/
```

### GPU-Z

查看GPU配置。

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

## 文件压缩

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

## 镜像拷录

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

## 文本编辑

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

## 本地搜索

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

## 密码破解

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

### 压缩包解密

```
https://axu.lanzous.com/icbt77g
```

## 图片放大

### Waifu2x

```
https://github.com/AaronFeng753/Waifu2x-Extension-GUI/releases
```

### X-Mind

```
https://ltribe.lanzous.com/icUNjdo4o1g
```

## 终端美化

### FluentTerminal

带SSH功能。

```
https://github.com/felixse/FluentTerminal
```

## 网络调试

### PsPing

支持ICMP Ping、TCP Ping、网络延迟（TCP/UDP）、带宽测试（TCP/UDP）。

```
https://download.sysinternals.com/files/PSTools.zip
```

下载后放到C:\Windows\System32，在命令行输入psping即可开始使用。可输入以下命令查询具体命令。

```
// ICMP PING
psping -? i

// TCP PING
psping -? t

// 响应延迟
psping -? l

// 带宽测试
psping -? b
```

示例如下。

```
// ICMP PING
psping -4 -n 10 -w 2 -h 10 www.baidu.com

// TCP PING
psping -4 -n 10 -w 2 -h 10 www.baidu.com:443

// 响应延迟
psping -l 1500 -n 300 -h 10 51.ruyo.net:443

// 带宽测试
psping -b -l 1500 -n 300 -h 10 51.ruyo.net:443
```

### TCPing

检测端口是否开启和TCP延迟的工具。对于Windows，可通过以下链接下载，然后复制tcping.exe到C:\Windows\System32，在命令行输入`tcping`以调用。

```
https://www.elifulkerson.com/projects/tcping.php
```

对于Mac，通过Homebrew安装即可。

```
brew install tcping
```

对于Linux，可在终端执行以下命令以安装。

```
yum install -y tcptraceroute
yum install -y bc
 
cd /usr/bin
wget http://www.vdberg.org/~richard/tcpping
chmod +x tcpping
mv tcpping tcping
```

### FinalShell

一体化的服务器网络管理软件。

```
http://www.hostbuf.com/t/989.html
```

## 流程图

### draw.io

```
https://www.lanzoux.com/iQUObgedhrc
```

### Processist

```
https://www.lanzoux.com/idmOCgedlsh
```

### Dia

```
https://www.lanzoux.com/i5E9bgedkng
```

### ClickCharts

```
https://www.lanzoux.com/ii7PNgedi8j
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

### 远程关机

```
https://ltribe.lanzoui.com/iDJjmjekfpi
```

### Teambition云盘直链解析平台

直接打开以下链接即可使用。

```
https://teambition.icu/
```

也可在服务器自行搭建。对于Linux，在终端运行以下命令。

```
wget https://one.blob.core.chinacloudapi.cn/badyun/teambition/v0.01/app
chmod +x ./app
./app
```

然后在宝塔面板后台添加反向代理，代理名称为5213，目标URL为http://127.0.0.1/5213，发送域名为$host。

然后执行以下命令以让程序在后台运行。

```
nohup ./app &
```

对于Windows，下载以下应用并运行即可。

```
https://one.blob.core.chinacloudapi.cn/badyun/teambition/v0.01/app.exe
```

### 好司机下载器

提供电脑端大型游戏下载。

```
https://wwi.lanzous.com/i6FHJk3b1di
```

### 全能抢购神器

```
https://lanren.lanzous.com/iJPgojk0lfa
```

### 增强版Chrome

```
https://shuax.com/project/chrome/
```

# 常见问题

## 分区表错误导致硬盘无法识别

进入DOS版的Diskgenius并修复分区表即可。注意，WinPE的Diskgenius无效。

## 删除文件显示「访问被拒绝」

对于XP系统，需要先选择菜单栏的工具-文件夹选项-查看，取消勾选`使用简单文件共享`后确定。在需要操作的文件上右键选择属性-安全-高级-所有者，选择`Administrators`并勾选`替换子容器及对象的所有者`，确定即可。

如果仍然不行，则点击安全-添加-高级-立即查找，然后一路确定即可。

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

## Word查找替换详细用法及通配符一览表

```
https://www.cnblogs.com/whchensir/p/5768030.html
```

## 用Win7，电脑就是路由器

```
https://hoochanlon.github.io/fq-book/#/append/win7-wifi
```

## 微软PsPing适用于Windows平台的网络测试工具

```
https://51.ruyo.net/15171.html
```

## TCPing 一款检测端口是否开启和TCP延迟的工具

```
https://51.ruyo.net/14528.html
```

## Teambition云盘直链解析平台 支持多用户，支持永久直链，支持列目录

```
https://www.52pojie.cn/thread-1320716-1-1.html
```
