---
title: Windows各版本系统的安装及配置
categories: Windows
abbrlink: Windows-Installation
date: 2020-04-09 00:59:29
tags:
---

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gegb2ckiyij31c00u016c.jpg)

不同版本的Windows在现代电脑上安装方法并不相同。通过相关配置，可以运行较旧的Windows版本。

<!-- more -->

# 系统支持性

系统对MBR/GPT分区表的支持性如下。建议GPT分区表使用64位操作系统。

|     操作系统    | MBR  |                GPT                |
|-----------------|------|-----------------------------------|
| Windows XP x86  | 支持 | 不支持                            |
| Windows XP x64  | 支持 | 支持（仅基于x64的IA64安腾处理器） |
| Windows 7 x86   | 支持 | 不支持                            |
| Windows 7 x64   | 支持 | 支持                              |
| Windows 8 x86   | 支持 | 支持                              |
| Windows 8 x64   | 支持 | 支持                              |
| Windows 8.1 x86 | 支持 | 支持                              |
| Windows 8.1 x64 | 支持 | 支持                              |
| Windows 10 x86  | 支持 | 支持                              |
| Windows 10 x64  | 支持 | 支持                              |

# 资源

## 镜像与软件

从以下链接可以获取官方安装包，主要分为自解压包和ISO镜像两种。

```
// 推荐，可获得原版系统
http://msdn.itellyou.cn/

// 推荐，可获得旧版系统和软件
https://winworldpc.com/library/operating-systems

// 其它镜像网站
https://www.getmyos.com/family/
https://www.imsdn.cn/

// 其它软件网站
https://www.oldapps.com/zh-cn/
http://www.oldversion.com/
http://20th.hotsun168.com/
http://www.mdgx.com/
http://toastytech.com/guis/index.html
http://www.bockelkamp.de/software/
http://toastytech.com/index.html
https://www.thecollectionbook.info/information/downloads/
```

对于Windows 10，可打开以下链接并按F12，切换为手机浏览视图即可看到下载按钮。

```
https://www.microsoft.com/zh-cn/software-download/windows10/
```

## 驱动

```
// 推荐，离线驱动包大全
http://sdi-tool.org/
https://www.52pojie.cn/thread-1060773-1-1.html

// Windows 2000/XP驱动
https://labmice.techtarget.com/drivers/default.htm

// Windows 2000/XP/2003驱动包
http://driverpacks.net/driverpacks/windows/xp
```

# 各系统安装

对于虚拟机，若为ISO镜像，直接挂载安装即可。若为自解压包，则需要先把解压出来的文件全部复制到要安装分区的根目录，然后进入DOS环境，打开Setup.exe完成安装。DOS环境可从微PE工具箱制作的U盘进入，也可通过挂载微PE工具箱或者纯DOS的ISO镜像的方式。

对于实体机，可能需要在虚拟机的安装流程基础上添加步骤。

## MS-DOS 5

### 现成虚拟机

```
https://jamesfriend.com.au/pce-js/ibmpc-games/
```

## MS-DOS 6.22

### 下载

该镜像为ISO格式。

```
https://www.jb51.net/softs/609407.html
```

### 虚拟机安装

直接挂载ISO。由于安装程序没有格式化工具，如果找不到安装磁盘，则先退出安装程序，进入纯DOS命令行环境，然后输入`fdisk`，按照提示进行分区操作（按1后再按1）。分区完成后需要格式化，命令为`format <盘符>`。完成后重启即可。

安装程序完成后不要马上推出ISO，再次进入ISO时将提示安装CDROM支持。选择安装，完成后再推出ISO。

如果无法完成操作，可以先挂载另一个DOS版本的ISO镜像，完成分区后再挂载原安装镜像。也可以挂载微PE工具箱的ISO镜像，通过内置的DOS工具箱完成操作。

## MS-DOS 7.10

### 下载

该镜像为ISO格式。

```
https://www.jb51.net/softs/86539.html
```

### 虚拟机安装

直接挂载ISO即可，安装程序会自动创建FAT分区并进行安装。建议选择`Full Installation`，允许所有Add-On的安装，但不要允许其Autorun。若遇到三个选项，选择Both，其它的都是Use Default即可。

### 实体机安装

首先通过微PE工具箱的DOS工具箱功能，使电脑进入DOS模式。工具箱中带有LOADISO程序，可挂载ISO，若无此工具则可自行下载。通过`LOADISO <镜像名>`挂载ISO安装镜像并切换到目录进行安装即可。注意，安装完成后需在Windows下通过Diskgenius等软件将MS-DOS所在分区设为活动分区，开机时才会进入到DOS系统。

### 配置

#### 软件安装

##### 安装方法

挂载ISO安装包可用LOADISO，挂载IMG镜像可用FAKEDISK。

若软件已解压，则将安装包放到适当位置，在DOS下进入安装路径并执行`SETUP`或`INSTALL`即可。

若软件为IMG包，在虚拟机上可直接挂载，在实体机上需提前整合为一个IMG文件再通过FAKEDISK进行挂载，也可直接把所有文件从IMG提取出来后再进行安装。可使用UltraISO进行IMG的整合或提取。

##### 软件库

```
https://www.dosgamesarchive.com/
https://www.sac.sk/files.php?d=14&l=F
http://www.kennedysoftware.ie/index.html
```

##### 软件列表

###### Turbo Debugger

```
http://old-dos.ru/files/file_1403.html
```

###### Turbo Basic

```
https://winworldpc.com/product/turbo-basic/1x
```

###### TASM

```
https://www.ticalc.org/archives/files/fileinfo/15/1504.html
```

###### Borland Turbo Assembler

```
https://winworldpc.com/product/turbo-assembler/5x
```

###### FORTRAN Optimizing Compiler

```
https://winworldpc.com/product/ibm-fortran-compiler/100
```

###### Sea Graphics Viewer V1.3

```
http://omolini.steptail.com/mirror/dcee/Files/Graf/index.html
```

###### PictView 1.94

```
http://www.pictview.com/
```

###### Adobe Acrobat Reader

```
https://winworldpc.com/product/acrobat-reader/4x
```

###### dBASE

```
https://winworldpc.com/product/dbase/v
```

###### 天汇标准汉字系统 V3.1

```
https://www.cr173.com/soft/20363.html
```

#### 添加NTFS支持

用`NTFS4DOS`即可。

### 常用命令

#### doskey

为命令提供别名。

```
// 定义
doskey a=echo hello

// 使用
a // 显示hello
```

### 说明

#### DOS挂载镜像后的盘符

挂载的ISO通常在CD-ROM设备上，如D:\，R:\等，DOS在启动时有记录。软盘或系统文件一般在A:\。

## Windows 1.0

### 下载

该Windows 1.0镜像为IMG格式。

```
https://winworldpc.com/product/windows-10/104
```

### 现成虚拟机

```
https://copy.sh/v86/?profile=windows1
```

### 虚拟机安装

首先安装好MS-DOS 7.10，然后向虚拟机添加一个软盘驱动器设备，在该设备上挂载Windows 1.0的IMG镜像。进入DOS环境，输入软盘盘符（通常为A:），并通过dir命令查看当前目录文件。通过`cd <目录>`命令可以切换到指定目录位置，切换到含有SETUP.EXE的目录后，直接输入`SETUP.EXE`进入Windows 1.0的安装程序，注意Graphics Mode应选择6。

安装完成后会回到DOS环境，输入以下命令即可运行Windows 1.0。

```
CD WINDOWS
WIN
```

### 实体机安装

首先安装好DOS，然后在DOS环境下通过`FAKEDISK <镜像名>`在DOS下进行IMG挂载，若无此工具则可自行下载。挂载后进入安装包并执行`SETUP`即可。

注意，由于`FAKEDISK`一次只能挂载一个IMG镜像，因此需提前将多个IMG文件合成为一个IMG文件，若系统在安装过程中提示插入另一张软盘，直接回车即可。

### 配置

#### 软件安装

软件库链接如下。

```
http://toastytech.com/guis/win1x2x.html
```

## Windows 2.0

### 下载

该Windows 2.0镜像为IMG格式。

```
https://winworldpc.com/product/windows-20/20
```

### 虚拟机安装

首先安装好MS-DOS 6.22，然后添加一个软盘驱动器设备并挂载Windows 2.0的IMG镜像，一般为SETUP.IMG。

安装前需要先配置DOS环境以避免安装程序出错。输入`edit config.sys`命令，将打开文本编辑器。清空并写入以下内容，保存并退出。

```
DEVICE=C:\DOS\SETVER.EXE
DEVICE=C:\DOS\HIMEM.SYS
DOS=LOW
FILES=30
DEVICE=C:\SMARTDRV.SYS 2704
```

完成配置后切换到软盘，运行SETUP.EXE以启动安装程序。其中电脑型号选择`Intel Inboard 386/AT`，Graphics Mode应选择2。在Run Memset Now页面选择M，在Change your CONFIG.SYS file页面选择C，按照提示更换挂载的IMG镜像即可完成安装。安装完成后会回到DOS环境，输入以下命令即可运行Windows 2.0。

```
CD WINDOWS
WIN86
```

### 配置

#### 软件安装

与Windows 1.0相同。

## Windows 3.x

### 下载

该Windows 3.x镜像为自解压包格式。

```
ed2k://|file|SC_Windows31.exe|8472384|84037137FFF3932707F286EC852F2ABC|/
```

### 现成虚拟机

```
https://jamesfriend.com.au/pce-js/ibmpc-win/
```

### 虚拟机安装

解压得到文件并用UltraISO将所有文件打包为ISO或IMG镜像（无需引导文件）。安装MS-DOS 7.10，然后挂载打包好的镜像，进入该分区，切换到安装包目录并运行SETUP.EXE即可。

也可在DOS环境安装好以后直接复制镜像文件到虚拟机分区根目录，再在根目录下直接运行安装程序。

安装完成后，在DOS输入`WIN`即可进入Windows 3.x。

### 实体机安装

过程与虚拟机安装基本一致。若在启动时遇到错误，则在DOS下执行`EDIT SYSTEM.INI`，添加以下代码并保存。

```
MaxPhysPage=30000
PageOverCommit=1
```

## Windows Whistler 2419

### 下载

该镜像为ISO格式。

```
https://cloudflare-ipfs.com/ipfs/QmXnj2yY4bnZjZStbjwdfxrYoUf9e4TSrpjT9Nnkz3J6kx/Microsoft%20Windows%20XP%20(''Whistler''%205.1.2419.1%20Professional%20B1).7z
```

### 虚拟机安装

安装前需先修改BIOS内的时间为`2001/1/14`。密钥如下。

```
RBDC9-VTRC8-D7972-J97JY-PRVMG
```

## Windows Whistler 2446

### 下载

该镜像为ISO格式。

```
https://cloudflare-ipfs.com/ipfs/QmNTSNtx8VxezgzEqeGBm4ZNd84JBn8RoMtD9qfosvKTqy/Microsoft%20Windows%20XP%20(''Whistler''%205.1.2446.1%20Professional%20B2).7z
```

### 虚拟机安装

直接挂载安装即可。

## Windows Whistler 2542

### 下载

该镜像为ISO格式。

```
https://winworldpc.com/product/windows-xp/rc-2
```

### 虚拟机安装

密钥如下。

```
BX6HT-MDJKW-H2J4X-BX67W-TVVFG
FP2SD-SI7RB-H4TYP-R460Q-TAYO2
```

## Windows Longhorn 4074

### 下载

该镜像为ISO格式。

```
https://winworldpc.com/product/windows-longhorn-vis/post-reset

链接 / https://pan.baidu.com/s/1f84tMQONUviZWTCkfaK43w
提取码 / x94r
```

### 虚拟机安装

安装前需先修改BIOS内的时间为`2004/04/27`。Windows Longhorn的安装程序在选择安装位置时，是不认识未格式化的新硬盘的，故需提前将硬盘格式化成NTFS。密钥如下。

```
TCP8W-T8PQJ-WWRRH-QH76C-99FBW
```

## Windows Longhorn 4093

### 下载

该镜像为ISO格式。

```
https://winworldpc.com/product/windows-longhorn-vis/pre-reset
```

### 虚拟机安装

虚拟机类型选择XP，直接安装即可。

## Windows FLP

### 下载

该镜像为ISO格式。

```
https://share.pipipan.com/info/1aq518408
```

### 虚拟机安装

直接挂载安装即可。密钥如下。

```
F4297-RCWJP-P482C-YY23Y-XH8W3
MRX3F-47B9T-2487J-KWKMF-RPWBY
```

## Windows Chicago

### 下载

该镜像为ISO格式。

```
https://winworldpc.com/product/windows-95/chicago
```

### 虚拟机安装

直接挂载安装即可。

## Windows Neptune

类似Windows 2000的系统。

### 下载

该镜像为ISO格式。

```
https://winworldpc.com/product/windows-neptune/build-5111

链接 / https://pan.baidu.com/s/10jHOYNAGuhayQu8IrzkY0w
提取码 / ldnh

链接 / https://pan.baidu.com/s/1jRn7N9IAJt3KV1r36u2B0Q
```

### 虚拟机安装

直接挂载安装即可。

## Windows Nashville 999

### 下载

该镜像为ISO格式。

```
https://winworldpc.com/product/windows-nashville/build-999
```

### 虚拟机安装

直接挂载安装即可。

## Windows NT 3.51 Workstation

### 下载

该镜像为ISO格式。

```
https://winworldpc.com/product/windows-nt-3x/351
```

### 虚拟机安装

直接挂载安装即可。

### 配置

#### 补丁安装

##### Windows NT 3.51 Service Pack 5

```
https://winworldpc.com/product/windows-nt-3x/patches
```

## Windows 95

Windows 95基于MS-DOS，行为与Windows 3.x相同。启动Windows 95时，都会先启动MS-DOS，然后Command.com在Windows目录中启动win.com。

### 下载

该镜像为ISO格式。

```
https://www.jb51.net/softs/160869.html
```

### 现成虚拟机

```
https://github.com/felixrieseberg/windows95
```

### 虚拟机安装

由于Windows 95无法自动识别ISO，因此需要先加载MS-DOS 7.10的安装镜像进行分区。完成格式化后不要安装DOS，改挂载Windows 95镜像，选择Boot from CD-ROM，然后选择Start Windows 95 Setup from CD-ROM即可。如果无法正确进入安装程序，可手动切换到安装目录并执行`SETUP`进行安装。安装采用以下密钥即可。

```
35296-OEM-0017544-62261
```

### 实体机安装

与虚拟机安装基本一致，注意安装路径为`C:\WINDOWS`。安装完成后，需要修复系统的CPU配置以正常启动系统。

下载以下CPU速度修复补丁。

```
http://lonecrusader.x10host.com/fix95cpu.html
```


提取FIX95CPU.IMA，并将对应文件复制到系统盘覆盖，具体路径如下。

|      IMA内的路径      |           复制到           |
|-----------------------|----------------------------|
| SYSTEM                | C:\WINDOWS\SYSTEM          |
| SYSTEM\IOSUBSYS       | C:\WINDOWS\SYSTEM\IOSUBSYS |
| SYSTEM\VMM32          | C:\WINDOWS\SYSTEM\VMM32    |
| SYSTEM\VMM32\POSTPACK | C:\WINDOWS\SYSTEM\VMM32    |

### 配置

#### 进入DOS

启动时按F8。

#### 软件安装

##### Microsoft Plus! 95

Windows 95补充包，提供附加功能。

```
https://winworldpc.com/product/plus/1995
```

##### SciTech Display Doctor

万能显卡驱动。

```
https://pan.baidu.com/share/link?shareid=505079&uk=3909595028
```

##### Virtual PC 2004

```
https://winworldpc.com/product/virtual-pc/2004
```

##### WinRAR 3.80

```
http://dl.pcgames.com.cn/download/47923.html
```

##### Internet Explorer 5.5 Service Pack 2

推荐先安装Internet Explorer 4.0获得Windows桌面更新。

```
https://winworldpc.com/product/internet-explorer/ie-55
```

##### DirectX 8.0a

```
http://www.oldversion.com/windows/download/directx-8-0a
https://www.fileplanet.com/archive/p-16752/DirectX-8-0a-for-Win95/download
```

##### Visual Studio 6.0

```
https://winworldpc.com/product/microsoft-visual-stu/60
```

##### AutoCAD 2000

```
http://www.xz7.com/downinfo/84226.html
```

##### Photoshop 5.x

```
http://www.sj00.com/soft/6827.htm
```

##### Winamp 2.81

```
http://www.oldversion.com/windows/download/winamp-2-81
```

##### IrfanView

```
// 源程序
https://www.irfanview.com/

// 语言包
https://www.irfanview.net/lang/irfanview_lang_chinese.exe
```

##### Total Command

```
// 源程序
https://www.ghisler.com/amazons3.php

// 插件包
http://xbeta.info/tc/plugins.htm
```

##### Opera 9.27

```
https://dl1.filesoul.com/a516a47cf24alv12aa70368c4a309ebb5cbbbfe8be46fe09/Opera-9-27.exe
```

##### CorelDraw 9

安装时是英文界面，安装后是中文版的，序列号随意填写即可。安装过程可能会出现一处找不到文件的错误，不影响使用。

```
https://web.archive.org/web/20150331194128/http://big.cr173.com/coreldraw9.7z
```

##### Microsoft Office 2000

```
https://winworldpc.com/product/microsoft-office/2000
```

##### Windows Media Player 7.1

```
http://www.oldversion.com/windows/download/windows-media-player-7-1
```

##### Microsoft VM (With Java)

```
http://www.uzzf.com/soft/4416.html
```

##### Windows Installer 2.0 For Windows 9x

```
https://download.cnet.com/Windows-Installer-Windows-95-98-Me/3000-2216_4-10049515.html
```

##### Windows Script 5.6

```
http://www.xitongzhijia.net/soft/16702.html
```

##### Flash Player 7.0

```
http://fpdownload.macromedia.com/get/flashplayer/installers/archive/fp7_archive.zip
```

##### NetMeeting 3.01

```
https://www.malavida.com/en/soft/netmeeting/download
```

##### Microsoft Agent 2.0

```
https://www.softpedia.com/get/System/System-Miscellaneous/Microsoft-Agent.shtml#download
```

##### Cih-kill终结版

```
http://pan.baidu.com/share/link?shareid=2928938278&uk=1883208263
```

##### WPS97

```
https://pan.baidu.com/share/link?shareid=2925457102&uk=1883208263
```

## Windows 98

### 下载

该镜像为自解压包格式。

```
ed2k://|file|SC_WIN98SE.exe|278540368|939909E688963174901F822123E55F7E|/
```

### 虚拟机安装

与Windows 3.x基本一致。

### 实体机安装

在执行安装程序时，需要通过`setup /p i`命令，否则将出现`0E 0028:ff02847b`错误。

安装完成后若出现内存问题，需修改SYSTEM.INI和SYSTEM.DB。进入PE环境，打开系统根目录下的SYSTEM.INI和SYSTEM.DB，添加以下代码并保存。

```
[386Enh]
EMExclude=C000-CFFF
MaxPhysPage=20000

[vcache]
MinFileCache=2048
MaxFileCache=65536
```

若最后黑屏且无法进入系统，则表明显卡驱动不兼容，此时只能进系统的安全模式。

### 配置

#### 进入DOS

启动时按Ctrl。

#### 补丁安装

部分错误对应的补丁可从以下网站下载。

```
https://www.tacktech.com/display.cfm?ttid=90
http://www.mdgx.com/web.htm#OSR2
https://erpman1.tripod.com/w9xmeupd.html
https://diarywind.com/blog/e/g13_084_win_98_se_1_win98_1.html
```

### 常见错误

#### 没有关机声音

禁用快速关机选项。

若无效，则打开`C:\Windows\WIN.INI`，在`load=`和`run=`前面加上`rem `，如下。然后保存。

对于`C:\Windows\SYSTEM.INI`，则对`[386Enh]`下结尾为`.386`的`Device=`添加`rem `。

```
rem load=...
rem run=...
```

若仍无效，则删除或重命名CONFIG.SYS与AUTOEXEC.BAT，使其启动时不起作用。

若仍无效，则在设备管理器中转到系统设备，然后转到高级电源管理。单击设置选项卡，然后取消选中启用电源管理。

若仍无效，则进入控制面板-电源管理，将关闭显示器和关闭硬盘都设置为从不。

若仍无效，则在设备管理器中转到系统设备，双击PCI Bus，进入IRQ指导选项卡，取消勾选使用IRQ指导。

若仍无效，则在设备管理器中转到系统设备，选择即插即用BIOS-设置，勾选禁用NVRAM/ESCD更新。

若仍无效，可尝试卸载杀毒程序，或禁用BIOS中的`Resume by Ring and LAN`。

### 安装程序参数说明

包括Windows 95和Windows 98。

|          参数         |                                                                                                                                         解释                                                                                                                                        |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                       | **Windows 98**                                                                                                                                                                                                                                                                      |
| /m                    | 跳过安装程序启动声音                                                                                                                                                                                                                                                                |
| /na                   | 跳过程序检查，可选参数有0（默认），1（没有基于窗口的程序检查，基于MS-DOS的程序被阻止），2（没有基于MS-DOS的程序检查，基于窗口的程序被阻止），3（没有基于MS-DOS和基于窗口的程序检查）                                                                                                |
| /nd                   | 忽略Migration.dll以强制重写较新的文件（包含`,,,32`标志的inf文件仍然会强迫Windows 98安装程序保留较新的文件）                                                                                                                                                                         |
| /nf                   | 跳过将软驱从软驱驱动器弹出的步骤                                                                                                                                                                                                                                                    |
| /nh                   | 在0%和只运行一次的文件上跳过运行Hwinfo.exe                                                                                                                                                                                                                                          |
| /nx                   | 跳过安装程序版本检查                                                                                                                                                                                                                                                                |
| /ie                   | 跳过Windows 98启动向导和Windows\Command\EBD目录的创建                                                                                                                                                                                                                               |
| /iv                   | 在Windows内升级期间跳过安装屏幕                                                                                                                                                                                                                                                     |
|                       | **Windows 95/98**                                                                                                                                                                                                                                                                   |
| /?                    | 显示可用参数                                                                                                                                                                                                                                                                        |
| /c                    | 跳过SMARTDrive                                                                                                                                                                                                                                                                      |
| /d                    | 不使用已存在的Windows配置                                                                                                                                                                                                                                                           |
| /l                    | 安装过程中允许使用Logitech鼠标                                                                                                                                                                                                                                                      |
| /n                    | 安装过程中允许没有鼠标                                                                                                                                                                                                                                                              |
| -s                    | 允许使用自选的Setup.inf文件                                                                                                                                                                                                                                                         |
| /t                    | 制定临时文件的位置（该文件夹内的文件将全部被删除）                                                                                                                                                                                                                                  |
| /ig                   | 允许安装程序在拥有早期BIOS的Micron/Gateway电脑上运行                                                                                                                                                                                                                                |
| /ih                   | 使磁盘检查程序在前台运行                                                                                                                                                                                                                                                            |
| /im                   | 跳过传统内存检查                                                                                                                                                                                                                                                                    |
| /iq                   | 停止安装程序检查驱动器中的交叉链接文件                                                                                                                                                                                                                                              |
| /is                   | 跳过磁盘检查程序                                                                                                                                                                                                                                                                    |
| /it                   | 跳过对终止驻留程序（TSRs）是否存在的检查（已知可能会导致安装程序出现问题）                                                                                                                                                                                                          |
| /p                    | 强制设置直接将字符串传递给检测管理器（或Sysdetmg.dll），需包含一个或多个检测选项，多个检测选项用分号分隔（如`setup /p b;c`），有些参数为开关选项，没有开关或在开关后加负号（-）则关闭，有开关则打开。有些参数形如`<c>=<params>`，若使用多个参数则用逗号分隔                         |
| /p a                  | 使每个检测模块尝试更安全的检测方法实现安全检测，默认启用                                                                                                                                                                                                                            |
| /p b                  | 在每次调用检测模块之前提示是否跳过，默认禁用                                                                                                                                                                                                                                        |
| /p c                  | 启用类检测（为特定类型的设备查找提示的机制），默认启用                                                                                                                                                                                                                              |
| /p c-                 | 禁用安全类检测，总是搜索所有网络适配器卡、声卡、CD-ROM驱动器                                                                                                                                                                                                                        |
| /p d=\<name>          | 仅检测列出的检测模块，其中`<name>`是检测模块名或设备类名                                                                                                                                                                                                                            |
| /p e                  | 启用设置模式检测，默认在安装期间启用，在其他情况下禁用                                                                                                                                                                                                                              |
| /p f                  | 启用Clean Registry模式，强制检测在启动前清除注册表的根分支，当安装程序在图形用户界面中运行时此开关将被忽略，默认禁用                                                                                                                                                                |
| /p g=\<n>             | 指定详细级别，可以帮助诊断检测问题，其中`<n>`为0到3，在最大级别（3）时内置的进度条会随进度条显示被检测设备的所有资源，默认禁用（0）                                                                                                                                                 |
| /p i                  | 在安装期间跳过报告存在即插即用BIOS                                                                                                                                                                                                                                                  |
| /p j                  | 撤销`/p i`开关的结果，应只在使用了`/p i`参数的计算机更新了其即插即用BIOS之后使用（在Windows 98中需要该参数才能启用ACPI）                                                                                                                                                            |
| /p l=\<n>             | 设置Detlog.txt的日志记录级别，其中`<n>`为0到3，默认最大（3）                                                                                                                                                                                                                        |
| /p m                  | 启用迷你窗口模式，只有当安装程序在MS-DOS下运行时才会生效                                                                                                                                                                                                                            |
| /p n                  | 启用无恢复模式，阻止创建Detcrash.log文件，默认禁用                                                                                                                                                                                                                                  |
| /p o(=\<traceoutput>) | 指定跟踪输出，其中信息被写入当前目录中的Tracelog.txt文件，仅在sysdetmm.dll的调试版本中可用                                                                                                                                                                                          |
| /p p                  | 将性能时间信息写入Detlog.txt文件，默认禁用                                                                                                                                                                                                                                          |
| /p r                  | 启用恢复模式，将使用Detcrash.log文件进行安全恢复，仅在安装过程中选择安全恢复时使用                                                                                                                                                                                                  |
| /p s=\<name>          | 跳过列出的检测模块或检测模块的类名，其中`<name>`为检测模块名或设备类名                                                                                                                                                                                                              |
| /p t=\<n>             | 指定跟踪级别，其中`<n>`为0到9，默认禁用（0），仅在sysdetmm.dll的调试版本中可用                                                                                                                                                                                                      |
| /p v x=\<res list>    | 启用仅验证模式，有两个阶段（验证注册表中的现有设备、检测新设备），PCMCIA向导使用此开关验证注册表中的遗留设备。检测模块不可访问在`<res list>`中的资源，其可能性有四种（`io(xxx-yyy,xxx-yyy,...)`、`mem(xxxxx-yyyyy,xxxxx-yyyyy,...)`、`irq(x,y,z,...)`、`dma(x,y,z,...)`），默认禁用 |

## Windows ME

### 下载

该Windows ME镜像为自解压包格式。

```
ed2k://|file|SC_WINME.exe|174098008|EEBAABADCD0162DA9F66F68E91B1B92A|/
```

### 虚拟机安装

安装流程与安装Windows 98的步骤一致。如果安装过程中遇到以下错误之一，则在DOS环境下执行`edit config.sys`，删掉所有与386有关的行，并重新运行安装程序即可。

```
Standard Mode: Invaild DPMI return.
Standard Mode: Fault in MS-DOS Extender.
Standard Mode: Bad Fault in MS-DOS Extender.
Standard Mode: Unknown stack in fault dispatcher.
Standard Mode: Stack Overflow.
```

### 实体机安装

安装完成后需修改SYSTEM.INI和SYSTEM.DB以解决内存问题。进入PE环境，打开系统根目录下的SYSTEM.INI和SYSTEM.DB，添加以下代码并保存。

```
[386Enh]
EMExclude=C000-CFFF
MaxPhysPage=20000 // 也可采用30000

[vcache]
MinFileCache=2048
MaxFileCache=65536
```

若最后黑屏且无法进入系统，则表明显卡驱动不兼容，此时只能进系统的安全模式。

### 配置

#### 进入DOS

启动时按F8。

## Windows Chicago

### 下载

该镜像为ISO格式。

```
https://winworldpc.com/product/windows-95/chicago
```

### 虚拟机安装

先装DOS622，装完后不要弹出ISO，继续装CDROM驱动。然后挂载Chicago镜像，进入setup。注意要安装TCP/IP协议。

```
Workgroup: WORKGROUP
Beta Site ID: 101907
Password: 999b4cb09
```

装完后强制关机，然后进入fail safe mode才能够进到系统。

## Windows Nashville

也被称为Windows 96。

### 下载

该镜像为ISO格式。

```
https://mega.nz/file/lJ8CTA6Q#f1nJ0YbcOde2nanjO15Lfnym-AXdGTXNK55lCZlpDmY
https://archive.org/details/WindowsNashvilleBuild999
```

### 虚拟机安装

先装DOS622，然后装Win3.1，注意装Win3.1时选择Custom，将安装目录改名。完成后回到DOS，挂载ISO，进行setup（不要在Win3.1运行安装程序），记得添加TCP/IP协议。安装完成后删除Win3.1的目录即可。

## Windows Whistler

### 下载

该镜像为ISO格式。

```
https://winworldpc.com/product/windows-xp/beta-1
```

### 虚拟机安装

虚拟机类型选XP。安装前需到BIOS修改系统时间，具体如下。

```
// Windows Whister 2419
2001/01/14

// Windows Whister 2446
2001/01/11

// Windows Whister 2542
2001/05/31
```

密钥如下。

```
// Windows Whister 2419
RBDC9-VTRC8-D7972-J97JY-PRVMG

// Windows Whister 2542
BX6HT-MDJKW-H2J4X-BX67W-TVVFG
```

## Windows NT 4.0

### 下载

该镜像为ISO格式。

```
http://www.xitongpe.com/win/39.html
```

### 虚拟机安装

直接挂载安装即可，密钥如下。

```
727-4198526
858-7198746
111-1111111
```

### 实体机安装

由于Windows NT 4.0无法通过WinNTSetup安装，因此需要通过恢复gho镜像的方式。镜像可通过虚拟机备份得到。

### 配置

#### 补丁安装

可安装的补丁列表如下。

##### KB918439

```
http://download.windowsupdate.com/msdownload/update/v3-19990518/cabpool/IE6.0sp1-KB918439-Windows-98-ME-x86-JPN_720b90d704dcdbb9e3a7055bd3357a2.exe
```

##### KB916281

```
http://download.windowsupdate.com/msdownload/update/v3-19990518/cabpool/IE6.0sp1-KB916281-Windows-98-ME-x86-JPN_9c9b4cc5e090467264a098fec8acf35.exe
```

##### KB837009

```
http://download.microsoft.com/download/2/2/5/2254fe6a-be28-4e2a-8dfe-bbe821cf3c75/OE6.0sp1-KB837009-x86-JPN.exe
```

##### Q329414

```
https://securityrealwebblog.blogspot.com/2011/11/http-mdac-component-query-bo.html
```

#### 软件安装

##### Windows NT 4.0 Service Pack 6

```
https://support.microsoft.com/en-us/topic/windows-nt-4-0-service-pack-6a-available-91aa538b-3d1d-0910-8538-350a2b3078a3
https://winworldpc.com/product/windows-nt-40/patches
```

##### Internet Explorer 6

```
https://www.oldapps.com/internet_explorer.php?old_internet_explorer=11
http://www.oldversion.com/windows/internet-explorer-6-0-full-installer
```

##### Microsoft GB18030 Support Package

```
https://microsoft-gb18030-support-package.software.informer.com/download/
```

##### MSXML 4.0 SP3 Parser

```
https://www.microsoft.com/zh-CN/download/details.aspx?id=15697
```

##### Visual Basic 6

```
https://winworldpc.com/product/microsoft-visual-bas/60
```

##### 远程桌面连接

```
https://remote-desktop-connection.software.informer.com/download/
```

##### Microsoft Agent 2.0

```
https://www.softpedia.com/get/System/System-Miscellaneous/Microsoft-Agent.shtml
```

##### Microsoft Data Access Components

```
https://www.microsoft.com/en-us/download/details.aspx?id=21995
```

## Windows 2000

### 下载

该镜像为ISO格式。

```
ed2k://|file|ZRMPSEL_CN.iso|402690048|00D1BDA0F057EDB8DA0B29CF5E188788|/
```

### 虚拟机安装

直接挂载安装即可。

### 实体机安装

Windows 2000无法通过WinNTSetup安装，通过gho镜像安装时会出现蓝屏错误。而安装程序在高于Windows 2000的系统运行时会出现已安装更高版本的提示。

因此需先安装好Windows NT 4.0，然后打开Windows 2000的安装包进行覆盖安装。

### 配置

#### 补丁安装

##### 工具

可使用`360漏洞修复独立版`、`金山卫士系统漏洞修复工具`、`RGSec漏洞修复2013`等。

##### 官方更新补丁

需要的安装包可在以下链接找到。

```
// 总补丁包
https://pan.baidu.com/s/1boKcmxp#list/path=%2F

// Service Pack 4
http://retro.remotecpu.com/files/win2k/updates/W2KSP4_EN.EXE

// KB835732
http://retro.remotecpu.com/files/win2k/updates/Windows2000-KB835732-x86-ENU.EXE

// Microsoft Internet Explorer 6
http://www.oldapps.com/internet_explorer.php?old_internet_explorer=11

// Windows Update Agent
http://download.windowsupdate.com/windowsupdate/redist/standalone/7.4.7600.226/windowsupdateagent30-x86.exe
http://greyghost.mooo.com/w2kadventures/WindowsUpdateAgent30-x86.exe

// DirectX 9.0c
http://retro.remotecpu.com/files/win2k/updates/DirectX-90c-x86-x86_64-Mar2009.zip

// KB975337
http://retro.remotecpu.com/files/win2k/updates/WindowsXP-KB975337-x86-ENU.exe

// Microsoft .Net 1.1 Framework
http://retro.remotecpu.com/files/win2k/updates/dotnet1.1fx.exe

// Microsoft .Net 2.0 SP1 Framework
http://retro.remotecpu.com/files/win2k/updates/NetFx20SP1_x86.exe

// Visual C++ 2005 Runtimes
http://retro.remotecpu.com/files/win2k/updates/vc++_2005_redist_x86.exe

// Visual C++ 2008 Runtimes
http://retro.remotecpu.com/files/win2k/updates/vc++_2008sp1_redist_x86.exe
```

首先需要安装Windows 2000 SP4和Windows 2000 SP4 Update Rollup 1，然后安装KB835732。为使用Windows Update，需要安装Microsoft Internet Explorer 6。然后安装Windows Update Agent。

打开Windows Update，保证允许ActiveX运行，并始终信任Microsoft Corporation的内容。若出现问题，则可将Windows Update添加到受信任的区域，并在Internet选项-高级选项卡下勾选使用TLS 1.0、SSL 2.0和SSL 3.0。

顺利打开Windows Update后，按照流程更新。完成后安装其它补丁即可。

##### 非官方更新补丁

###### Gurgelmyer's Unofficial Service Pack 5.1

即USP5.1，建议在运行Windows Update前安装。

```
http://retro.remotecpu.com/files/win2k/updates/Windows2000-UpdateServicePack-5.1.zip
https://www.majorgeeks.com/files/details/microsoft_windows_2000_unofficial_sp.html
```

###### Gurgelmyer's Unofficial Update Rollup 2

即UUR2，建议在运行Windows Update后安装。

###### Gurgelmyer's Unofficial Update Rollup

即UURollup。建议在运行Windows Update后安装，不能同时安装多个UURollup包。

先安装以下补丁。

```
// Windows 2000 Update Pack
http://win2k.dyniform.net/windows-2000-update-pack.html

// Gurgelmyer's Unofficial Update Rollup
http://retro.remotecpu.com/files/win2k/updates/Windows2000-UURollup-v11-d20141130-x86-ENU.7z
```

启动注册表编辑器，定位到HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Nls\Language，修改右侧最下方的Default为0804，InstallLanguage为0409。代码的具体含义如下。

| 代码 | 语言 |
|------|------|
| 0409 | 英文 |
| 0411 | 日文 |
| 0804 | 中文 |

安装KB979683-v16a-ENU、KB935839-v30e-ENU、NET3.5 RTM for Extended Kernel，然后再装驱动。补丁下载地址如下。

```
// KB979683&KB935839
http://blog.livedoor.jp/blackwingcat/archives/1299806.html

// NET3.5 RTM for Extended Kernel
http://blog.livedoor.jp/blackwingcat/archives/1095338.html
```

再次进入注册表的相同路径，将Default的值改为0804，InstallLanguage的值改为0409。

##### 存档

```
http://win2k.dyniform.net/security-updates
https://twilczynski.com/windows/updates/
https://mega.nz/folder/2lBVBBLI#WqmqhpxuX0qyCY1LiX4-gw/folder/e4IkXAZA
```

#### 软件安装

##### iTunes

```
http://blog.livedoor.jp/blackwingcat/archives/1327962.html
http://blog.livedoor.jp/blackwingcat/archives/873798.html
```

##### HeidiSQL 8.0.0.4396

```
https://www.npackd.org/p/heidisql/8.0.0.4396
```

##### mySQL 5.1.73

```
http://www.121down.com/soft/softview-54300.html
```

##### Java 6.0.31

```
http://www.oldversion.com/windows/download/java-platform-6-update-31
```

##### UltraEdit V17

```
https://www.afterdawn.com/software/general/download_splash.cfm/ultraedit
```

##### PHP 5.2.13

```
https://www.php.net/releases/index.php
```

##### FastStone Capture 7.1

```
https://faststone-capture.informer.com/download/#downloading
```

##### FastStone Viewer 6.3

```
https://xetbox.com/854/file
```

##### 光影魔术手NeoImaging 3.1.2.104

```
https://download.pchome.net/design/digipic/download-20642.html
```

##### LibreCAD 1.0.4

```
https://www.filehorse.com/download-librecad/15292/download/
```

##### PhotoScape 3.7

```
https://photoscape.soft32.com/free-download/?nc&dm=3
http://retro.remotecpu.com/files/win2k/apps/PhotoScape_V3.7.exe
```

##### PicPick 2.3.7

```
http://retro.remotecpu.com/files/win2k/apps/picpick-2.3.7.exe
```

##### PicPick 3.0.4

```
http://www.oldversion.com/windows/download/picpick-3-0-4
```

##### PicPick 3.1.8

```
http://retro.remotecpu.com/files/win2k/apps/picpick-3.1.8-win.exe
```

##### PicPick 4.1.1

```
http://retro.remotecpu.com/files/win2k/apps/picpick_inst_v4.1.1.exe
```

##### freeCAD 0.12.5284

```
https://www.filehorse.com/download-freecad-32/14114/download/
```

##### gimp 2.6.1

```
https://download.gimp.org/pub/gimp/v2.6/windows/
```

##### inkscape 0.47.3

```
https://inkscape.org/release/0.47/windows/32-bit/
```

##### paint.net 2.72

```
http://www.oldversion.com/windows/download/paint-net-2-72
```

##### Firefox 12.0

```
https://ftp.mozilla.org/pub/firefox/releases/12.0/win32/zh-CN/
http://retro.remotecpu.com/files/win2k/network/Firefox_Setup_12.0.exe
```

##### Firefox 43.0.1

需要USP 5.1。

```
http://retro.remotecpu.com/files/win2k/network/Firefox%20Setup%2043.0.1.exe
```

##### Firefox 44.0.0

需要USP 5.1和UURollup，通过Application Compatibility Launcher运行。

```
http://retro.remotecpu.com/files/win2k/network/Firefox%20Setup%2044.0.exe
```

##### Adobe Flash Player 11.1.102.55

```
http://www.oldversion.com/windows/download/macromedia-flash-player-11-1-102-55-ie
```

##### Adobe Flash 20.0 Plugin for FF

需要USP 5.1。

##### Opera 1202

```
http://get.opera.com/ftp/pub/opera/win/1202/zh-cn/
http://retro.remotecpu.com/files/win2k/network/Opera_1202_int_Setup.exe
```

##### QQ旋风 4.1.734

```
http://dl_dir.qq.com/invc/cyclone/QQDownload_Setup_41_734_401.exe
```

##### RASPPPOE 098B

```
https://www.softpedia.com/get/Network-Tools/Misc-Networking-Tools/RASPPPOE.shtml
```

##### Serv-U FTP Server 6.1.0.1

```
http://www.hanzify.org/software/10021.html
```

##### TeamViewer 7.0.15723

```
https://www.filepuma.com/download/teamviewer_7.0.15723-1692/download/
```

##### TeamViewer 8

```
http://retro.remotecpu.com/files/win2k/network/TeamViewer-v8_Setup.exe
```

##### Free Download Manager 3.0.852

```
https://www.filehorse.com/download-free-download-manager-32/6092/download/
```

##### Adobe Audition 1.5

```
https://www.wmzhe.com/soft-35098.html
```

##### audacity 2.0.5

```
https://www.filehorse.com/download-audacity/15176/download/
```

##### freac 1.0.19

```
https://sourceforge.net/projects/bonkenc/files/freac/1.0.19/
```

##### K-Lite Codec 7.10

```
https://www.filehorse.com/download-klite-mega-codec/7877/download/
```

##### Media Player Classic Home Cinema 1.3.1249.0

```
https://www.filehorse.com/download-media-player-classic-32/7649/download/
```

##### PotPlayer 1.5.45955

```
https://www.filehorse.com/download-daum-potplayer-32/16503/download/
```

##### QQ影音 3.8.897

```
http://dldir1.qq.com/qqyy/pc/QQPlayer_Setup_38_897.exe
```

##### QuickTime 7.0.4

```
https://www.filehorse.com/download-quicktime-player/4327/download/
```

##### aimp 3.20.1165

```
https://www.filepuma.com/download/aimp_3.20.1165-2105/download/
```

##### lame 3.99.5

```
https://sourceforge.net/projects/lame/files/lame/3.99/
```

##### 千千静听 5.79

```
https://www.jiejueba.com/yinyuebofangqi/1761.html
```

##### Foxit Reader 5.3.1

```
https://www.filepuma.com/download/foxit_reader_5.3.1.0606-923/download/
```

##### 谷歌拼音输入法 1.2

```
http://dl.google.com/pinyin/v1/GooglePinyinInstaller.exe
```

##### HuarWB 3.6.16.0728

```
http://fzyd.jz5u.com:7011/tougao/huarwb.rar
```

##### StoryView 1.9.5.3

```
https://www.ouyaoxiazai.com/soft/yyrj/73/45357.html
https://pan.baidu.com/s/1o8ivtHK
```

##### EBWin 3.06

```
http://ebstudio.info/home/EBPocket.html
```

##### GoldenDict 1.0.1

```
https://www.download82.com/get/download/windows/goldendict/
https://www.fileeagle.com/download/software/file/441/758c8a/
```

##### 汉字宝典 3.6

```
http://www.xitongzhijia.net/soft/58768.html
```

##### lingoes 2.9.2

```
http://www.lingoes.net/en/translator/trans_downsoft.php?id=30
```

##### Apabi Reader PRC 4.4.3.1726

```
http://xiazai.zol.com.cn/detail/41/406551.shtml
```

##### Notepad++ 6.6.6

```
https://www.filehorse.com/download-notepad-32/17129/download/
```

##### stellarium 0.12.9

```
https://github.com/Stellarium/stellarium/releases/tag/stellarium-0-12-4
```

##### 极点五笔

```
http://www.jisuxz.com/down/29.html
```

##### 精灵五笔98版 3.0.0.30

```
http://soft.miiyun.com/pcsoftware/shrf/432918.html
```

##### 可可五笔 4.2.1.1339E83

```
http://www.xitongcheng.cc/soft/16771.html
```

##### LibreOffice 3.6.7.2

```
http://www.oldversion.fr/windows/download/libreoffice-3-6-5
```

##### 7z 15.14

```
http://retro.remotecpu.com/files/win2k/utils/7z1514.exe
```

##### 7z 18.06

```
https://sourceforge.net/projects/sevenzip/files/7-Zip/18.06/
```

##### Autoruns

```
http://software.oldversion.com/windows/download/autoruns-9-2
```

##### CD Image GUI Beta 3

```
http://www.downza.cn/soft/260427.html
```

##### Daemon Tools Lite 4.49.1.0356

```
https://www.kviso.com/kviso/20664.html
```

##### DiskGenius 4.6.5

```
http://pe.udashi.com/html/56.html
```

##### FileZilla 3.7.4.1

```
https://www.filehorse.com/download-filezilla-32/16231/download/
```

##### FileZilla 3.8.0

```
http://retro.remotecpu.com/files/win2k/network/FileZilla_3.8.0_win32-setup.exe
```

##### FreeCommander 2009.02b

```
https://freecommander.com/en/downloads/
```

##### KeyboardTest 2.2

```
http://www.xitongzhijia.net/soft/16644.html
```

##### PowerISO 6

```
https://www.afterdawn.com/software/general/download_splash.cfm/poweriso
```

##### TreeSize 2000

```
https://www.filehorse.com/download-treesize-free/15951/download/
```

##### UltraISO 9.66.3300

```
https://www.filepuma.com/download/ultraiso_premium_9.6.6.3300-13396/download/
```

##### WiseCare365 2.99

```
https://www.filehorse.com/download-wise-care-365/16925/download/
```

##### CDBurnerXP 4.5.6.5844

```
https://www.filehorse.com/download-cdburnerxp-32/21654/download/
```

##### Dexpot 1.5.18

```
http://dexpot.de/download/dexpot_1518_r2098.exe
```

##### PeaZip 6.4.1

```
https://sourceforge.net/projects/peazip/files/6.4.1/
```

##### renamer 6.7

```
https://xetbox.com/2471/file
```

##### teracopy 2.3

```
https://www.filehorse.com/download-teracopy/15564/download/
```

##### Java Runtime Environment 1.5.0.22

```
https://www.filehorse.com/download-java-runtime-32/4417/download/
```

##### Visual Basic 6.0

```
https://winworldpc.com/product/microsoft-visual-bas/60
```

##### Lotus Domino Server 7.01

```
http://www.pc0359.cn/downinfo/57421.html
```

##### Symantec Endpoint Protection V11.0

```
http://www.uzzf.com/soft/10169.html
```

##### Adobe CS2

```
http://www.downza.cn/soft/226348.html
```

##### Adobe Acrobat Pro 8.0

```
http://xiazai.zol.com.cn/detail/44/437102.shtml
```

##### Adobe Reader 9.5

```
http://retro.remotecpu.com/files/win2k/apps/AdbeRdr950_en_US.exe
```

##### Apache OpenOffice 4.1.6

```
https://www.filepuma.com/download/apache_openoffice_4.1.6-20776/download/
```

##### Libre Office 3.6

```
https://www.filehorse.com/download-libreoffice-32/12594/download/
```

##### Microsoft Office 2003

```
http://xiazai.zol.com.cn/detail/36/357450.shtml
```

##### Visio 2000

```
https://winworldpc.com/product/visio/2000
```

##### WPS 2003

```
http://www.downza.cn/soft/31092.html
```

##### Borland C++ 5.02

```
https://winworldpc.com/product/borland-c/5x
```

##### borland C++ Builder 6.0

```
http://www.32r.com/soft/16007.html
```

##### Delphi 7

```
http://www.downza.cn/soft/25565.html
```

##### Microsoft Visual Studio 2005

```
http://www.xz7.com/downinfo/82542.html
```

##### 中文之星2006

```
http://www.piaodown.com/soft/13194.htm
```

##### AutoCAD 2005

```
http://www.xitongzhijia.net/soft/165187.html
```

##### 会声会影 10.0

```
https://www.fixdown.com/soft/634.html
```

##### Microsoft SQL Server 2005

```
https://dl.pconline.com.cn/download/364094.html
```

##### CorelDraw 12.0

```
https://www.onlinedown.net/soft/26604.htm
```

##### Macromedia Studio 8.0

```
https://archive.org/details/macromediastudio8_en
```

##### PEMaker 0.31

```
http://blog.livedoor.jp/blackwingcat/archives/1386003.html
```

##### iPod Touch Driver for Windows 2000

```
http://blog.livedoor.jp/blackwingcat/archives/773170.html
```

##### Windows Media Player 11

```
http://blog.livedoor.jp/blackwingcat/archives/367804.html
http://win8.8miu.com/thread-779531-1-1.html
```

##### mIRC 7.32

```
http://retro.remotecpu.com/files/win2k/network/mirc732.exe
```

##### mIRC 7.43

需要USP 5.1。

```
http://retro.remotecpu.com/files/win2k/network/mirc743.exe
```

##### qBittorrent 2.9.7

```
http://retro.remotecpu.com/files/win2k/apps/qbittorrent-2-9-7-win.exe
```

##### qBittorrent 3.3.3

需要USP 5.1。

```
http://retro.remotecpu.com/files/win2k/network/qbittorrent_3.3.3_setup.exe
```

##### Httrack 3.48.21

```
http://retro.remotecpu.com/files/win2k/network/httrack-3.48.21.exe
```

##### DosDrop

```
http://retro.remotecpu.com/files/win2k/utils/dosdrop.zip
```

##### Iconoidv 3.8.6

```
http://retro.remotecpu.com/files/win2k/utils/iconoid.zip
http://retro.remotecpu.com/files/win2k/desktop/iconoid.zip
```

##### Virtual Clone Drive 5.5.0.0

```
http://retro.remotecpu.com/files/win2k/utils/SetupVirtualCloneDrive5500.exe
```

##### WinSplit Revolution 1.9

```
http://retro.remotecpu.com/files/win2k/apps/winsplit-revolution-1.9.exe
```

##### VLC 1.1.11

```
http://retro.remotecpu.com/files/win2k/apps/vlc-1.1.11-win32.exe
```

##### zMatrix 1.5.2

```
http://retro.remotecpu.com/files/win2k/apps/zmatrixsetupnt_1_5_2.exe
```

##### PDF reDirect 2.2.8

```
http://retro.remotecpu.com/files/win2k/utils/Install_PDFR_v228.exe
```

##### CursorXP 1.30

```
http://retro.remotecpu.com/files/win2k/desktop/cursorxpfree130.exe
```

##### ClamWin 0.99

```
http://retro.remotecpu.com/files/win2k/utils/clamwin-0.9.9-setup.exe
```

##### Total Commander 8.52a

```
http://retro.remotecpu.com/files/win2k/utils/tcmd852ax32.exe
```

##### Notepad++ v6.6.9

```
http://retro.remotecpu.com/files/win2k/utils/npp.6.6.9.Installer.exe
```

##### WinVICE 2.4

```
http://retro.remotecpu.com/files/win2k/emulators/WinVICE-2.4-x86.zip
```

##### CGTerm 1.7b2

```
http://retro.remotecpu.com/files/win2k/terms/cgterm-1.7b2-win32.zip
```

##### CoolMon 1.0

```
http://retro.remotecpu.com/files/win2k/desktop/coolmon_setup.exe
```

##### Transbar 1.4.2

```
http://retro.remotecpu.com/files/win2k/desktop/transbar-1.4.2_setup.exe
```

##### Calibre 1.48.0

需要USP 5.1。

```
http://retro.remotecpu.com/files/win2k/apps/calibre-1.48.0-winxp.msi
```

##### Google Picasa v3.9

需要USP 5.1。

```
http://retro.remotecpu.com/files/win2k/apps/picasa39-setup.exe
```

##### VLC 2.22

需要USP 5.1和UURollup，通过Application Compatibility Launcher运行。

```
http://retro.remotecpu.com/files/win2k/apps/vlc-2.2.2-win32.exe
```

##### Winamp 5.621

需要USP 5.1。

##### WinSplit Revolution 11.04

需要USP 5.1。

```
http://retro.remotecpu.com/files/win2k/apps/WinSplit-Revolution-v11.04.exe
```

##### Putty 0.66

需要USP 5.1。

```
http://retro.remotecpu.com/files/win2k/utils/putty.exe
```

#### 驱动工具

##### WiFi驱动

###### Intel PROSET Utility (802.11a/g,g) for Windows 2000

Intel的WiFi驱动。

```
https://support.dynabook.com/support/viewContentDetail?contentId=1197543
```

###### Boingo Wireless

WiFi管理工具，可用于WPA2连接。

```
http://retrosystemsrevival.blogspot.com/2018/01/boingo-wireless.html
https://yadi.sk/d/yJ8R94-5e2xFgg
```

##### ATI显卡驱动

下载链接如下。

```
// ATI Display Drivers
http://retro.remotecpu.com/files/win2k/drivers/6-2_xp-2k_dd_30152.exe

// ATI Catalyst Suite
http://retro.remotecpu.com/files/win2k/drivers/6-2_xp-2k_dd_ccc_wdm_enu_30152.exe

// 驱动
https://driverzone.com/%7Beb1a4349-a527-4837-9abf-a66c64fff9eb%7D?id=1547175
http://blog.livedoor.jp/blackwingcat/archives/571484.html?blog_id=2914225
https://devid.info/find?lng=ca&text=ATI+Mobility+Radeon+HD+3470
```

##### 驱动包

###### 显卡

```
https://drive.google.com/file/d/0B_3gCgMW-1B7bW5HTE9sRUgzeXM/view
https://drive.google.com/file/d/0B_3gCgMW-1B7c0xDT2N0WEFDdTg/view

// 旧版
https://drive.google.com/file/d/0B_3gCgMW-1B7VXNzYllrWTNUOVk/view
https://drive.google.com/file/d/0B_3gCgMW-1B7RDN5V3NKWl91OU0/view
```

###### 有线网卡

```
https://drive.google.com/file/d/0B_3gCgMW-1B7YTlsZkF1a29KMW8/view

// 旧版
https://drive.google.com/file/d/0B_3gCgMW-1B7Tk04ZDhKS2pIRjQ/view
```

###### 无线网卡

```
https://drive.google.com/file/d/0B_3gCgMW-1B7V3VmWnBLeG5zaGM/view

// 旧版
https://drive.google.com/file/d/0B_3gCgMW-1B7TWlsUzVDTzRueXc/view
```

###### 存储

```
https://drive.google.com/file/d/0B_3gCgMW-1B7SjAtbVJjeV9UV3M/view

// 旧版
https://drive.google.com/file/d/0B_3gCgMW-1B7ekNkdy02Q0JNMDA/view
```

#### 启用无线配置管理器

进入控制面板，选择管理工具，打开服务。找到Wireless Manager的服务，将其设为自动启动即可。

### 扩展内核版

下载黑翼猫的Windows 2000镜像并安装，然后安装KB979683、KB935836、Colortrap、MUI，在控制面板安装NET。安装必要补丁，但不要安装NET4。千万不要安装非官方更新补丁，否则会蓝屏。

## Windows Server 2003

### 下载

该镜像为ISO格式。

```
ed2k://|file|cn_windows_server_2003_sp2_x86_cd.iso|390135808|4C38E53EF100F80683810CAC1044CA70|/
```

### 虚拟机安装

直接挂载即可。

### 实体机安装

通过PE下的WinNTSetup安装即可。

### 配置

#### 补丁安装

适用于Windows Server 2003的补丁包如下。

```
链接 / https://pan.baidu.com/s/1pLG846B
提取码 / 72wy
```

## Windows XP

### 下载

该镜像为ISO格式。

```
ed2k://|file|zh-hans_windows_xp_professional_with_service_pack_3_x86_cd_x14-80404.iso|630239232|CD0900AFA058ACB6345761969CBCBFF4|/
```

### 虚拟机安装

直接挂载即可。

### 实体机安装

通过PE下的WinNTSetup安装即可。

### 配置

#### 系统激活

可用`XP OEM免激活`工具激活。

```
http://soft.onlinedown.net/soft/580556.htm
```

#### 制作CAB压缩/自解压缩包

在运行输入IExpress，选择Create new Self Extraction Directive file，然后选择打包类型后添加文件即可。

#### 驱动安装

##### 显卡驱动

以ATI显卡为例。下载`ATI Catalyst for XP`，链接如下。

```
https://www.crsky.com/soft/3287.html
```

打开后安装包会把安装程序解压到`C:\AMD\Support`，完成驱动安装。

如果安装不成功，则下载DH Mobility Modder.NET，运行时需要.net frameworks 2.0和MSXML 4.0。下载地址如下。

```
http://www.crsky.com/soft/5368.html
```

解压后运行MobilityDotNET.EXE，目录选择C:\ATI\SUPPORT\14-4-xp32-64-dd-ccc-Pack3，点击Modify即可。

在设备管理器中，更新显卡驱动程序，`从磁盘安装...`时选择C:\ATI\SUPPORT\14-4-xp32-64-dd-ccc-Pack3\Driver\Driver\XP_INF下的CX_59344.inf文件，在目录表中选择`ATI Radeon HD 3400`即可。

安装完成后，如果没有Catalyst Control Center，则到C:\AMD\Support\14-4-xp32-64-dd-ccc-Pack3\Packages\Apps\CCC\Core-Static安装ccc-core-static.msi即可。

正确安装驱动后可连接外接显示器。进入Catalyst Control Center，点击Displays Manager，选择外接的显示器并设置为Clone，然后在Desktop Area选择合适的分辨率即可。

##### 指纹驱动

以AuthenInc指纹识别器为例，安装`TrueSuite Access Manager`指纹系统即可。

```
https://driverscollection.com/?file_cid=447398325664ec25068609a422f
```

#### 软件安装

##### Office 2010

```
http://www.xitongzhijia.net/soft/24189.html
```

##### VirtualBox 5.2.16

```
https://www.zdfans.com/html/4937.html
```

##### Vmware Workstation 10.0.7

```
https://www.3xiazai.com/soft/system/50321.html
```

##### Chrome 49.0.2623

```
https://www.jb51.net/softs/143399.html
```

##### FireFox 52.9.0

```
https://www.filehorse.com/download-firefox-32/36446/download/
```

##### Windows优化大师

```
http://www.youhua.com/
```

##### McAfee VirusScan Enterprise 8.8

企业版免费使用，下载链接如下。

```
http://www.downcc.com/soft/1943.html
https://www.jb51.net/softs/116659.html
https://www.macxin.com/archives/6551.html
```

安装后可能无法进行病毒库自动更新，则需要手动更新病毒库。访问以下网站，下载最新的`avvdat-xxxx.zip`到电脑并解压，得到dat文件。

```
ftp://ftp.nai.com/commonupdater/
```

进入VirusScan控制台，双击访问保护，取消勾选`禁止McAfee服务被停止`并确定。Win+R打开运行，输入`services.msc`，选择`McAfee McShield McAfee Framework`服务并停止。

复制得到的dat文件到C:\Program Files\Common Files\McAfee\Engine，覆盖后进入VirusScan控制台，点击帮助-关于，查看dat版本是否为最新。

完成更新后重新打开`McAfee McShield McAfee Framework`服务，并在VirusScan控制台勾选`禁止McAfee服务被停止`即可。

##### Windows清理助手

```
http://www.arswp.com/
```

#### 补丁安装

##### 普通补丁

可通过Windows Update安装。

##### 获取更新补丁

伪装为PosReady，可获取更新的补丁。下载以下补丁包以安装2014年5月前XP的所有补丁，通过Windows Update确认补丁已经全部打完。

```
https://www.xiazaiba.com/html/120.html
```

打开记事本，输入以下内容后保存为reg文件，然后双击运行。

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\WPA\PosReady]
"Installed"=dword:00000001
```

打开以下网站以安装2014年5月后的补丁，然后再通过Windows Update更新其余补丁。

```
https://www.zxinc.org/xppatch.htm
```

注意，伪装成PosReady后不能为Office打补丁，否则Office将会显示操作系统设置不匹配的错误。

若想关闭伪装，另存为以下reg文件并运行即可。

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\WPA\PosReady]
"Installed"=dword:00000000
```

##### GPT补丁

打开以下链接下载补丁。

```
http://www.downcc.com/soft/21920.html
```

切换到路径C:\Windows\Driver Cache，备份sp3.cab后替换其中的disk.sys为下载的文件。注意，解压操作可通过普通压缩软件，重新打包操作可通过XP自带的IExpress等。然后替换C:\Windows\system32\dllcache和C:\Windows\system32\drivers目录下的disk.sys，重启即可。

### 常见错误

#### 安装程序提示「the file xxx.sys could not be found」

将系统安装镜像i386中的所有文件复制到`C:\win.NT~BT`和`C:\win.NT~LS\i386`即可。

#### 无法/拒绝访问文件夹

以管理员身份登录，打开资源管理器，选择菜单工具-文件夹选项，单击`查看`选项卡，取消勾选`使用简单共享`。右键单击不能打开的文件夹，从弹出菜单中选择`属性`，单击安全-高级，选择`将所有者更改为下方的用户`，选中下方的`替换子容器及对象的所有者`，单击`应用`。单击权限-添加，在`选择用户或组`中单击高级-立即查找。选择要给予权限的用户，连续单击两次`确定`，直到出现`权限项目`对话框。在`权限`中的`完全控制`右侧选择`允许`，连续单击确定直到关闭所有对话框即可。

## Windows XP x64

### 下载

该镜像为GHO格式。

```
https://cloud.189.cn/t/NzMFZ3nuyIFf
```

### 虚拟机安装

挂载PE镜像并通过Ghost恢复即可。

### 实体机安装

进入PE并通过Ghost恢复即可。

### 配置

#### 软件安装

##### Office 2007

```
http://www.downza.cn/soft/20590.html
```

##### 完美解码20190329版

```
https://www.ranwenzw.com/xiazai/27995.html
```

#### 补丁安装

##### Service Pack 2 for Windows XP Professional, x64 Edition

```
https://www.microsoft.com/en-us/download/confirmation.aspx?id=17791
```

## Windows XP Tablet PC

### 下载

```
// CD1
http://www.ilovext.com/msdn/9.html

// CD2
http://www.ilovext.com/msdn/10.html
```

### 安装

实际上Tablet PC和Media Center是同一张ISO，安装完其实是Windows XP Media Center Edition 2005。密钥如下。

```
HCQ9D-TVCWX-X9QRG-J4B2Y-GR2TT
C4BH3-P4J7W-9MT6X-PGKC8-J4JTM
```

## Windows XP Media Center Edition 2005

### 下载

```
https://getintopc.com/softwares/operating-systems/windows-xp-media-center-edition-2005-iso-free-download-7260653/
```

### 安装

密钥如下。

```
WPPXX-GYT2K-TK339-W64QB-DKT4J
```

## Windows XP Starter Edition

### 下载

原镜像如下，该镜像为ISO格式。

```
链接 / http://pan.baidu.com/s/1ctUDm2
提取码 / er1u
```

中文包如下。

```
https://pan.baidu.com/s/1c10vWmk
```

### 虚拟机安装

直接挂载即可。

### 实体机安装

进入PE并通过WinNTSetup安装即可。

## Windows XP Vienna Edition

### 下载

该镜像为ISO格式。

```
https://getintopc.com/softwares/operating-systems/windows-xp-vienna-edition-free-download-1613893/
```

### 虚拟机安装

直接挂载即可，密钥如下。

```
3D2W3-8DJM6-YKQRB-B2XDB-TVQHF
```

### 实体机安装

进入PE并通过WinNTSetup安装即可。

## Windows XP SP3 Black Edition

### 下载

该镜像为ISO格式。

```
https://getintopc.com/softwares/operating-systems/windows-xp-sp3-black-edition-2014-free-download-1173658/
```

### 虚拟机安装

直接挂载即可，密钥如下。

```
3D2W3-8DJM6-YKQRB-B2XDB-TVQHF
```

### 实体机安装

进入PE并通过WinNTSetup安装即可。

## Windows CE 6.0

### 下载

该系统需要在Visual Studio中运行。

#### Visual Studio 2005

```
https://www.imsdn.cn/developer-tools/visual-studio-2005/
```

#### Visual Studio 2005 Service Package 1

```
https://www.microsoft.com/zh-CN/download/details.aspx?id=5553
```

#### Windows Embedded CE 6.0

Windows Embedded CE 6.0.msi为安装工具，其余为必要依赖。

```
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/Windows Embedded CE 6.0.msi
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/tools.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/tools_platman.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/tools_shared.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/tools_vc80.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/tools_diagnostics.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/tools_corecon.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/emulator.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/Data_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/wcetk_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/wcetk_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/wcetk_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/wcetk_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/wcetk_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/wcetk_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/wcetk_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/wcetk_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_1_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_1_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_1_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_1_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_1_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_1_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_2_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_2_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_2_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_2_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_2_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_3_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_23.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_4_24.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_23.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_5_24.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_23.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_24.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_25.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_6_26.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_7_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_23.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_24.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_8_25.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_9_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_23.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_24.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_25.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_10_26.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_11_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_23.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_24.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_25.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_12_26.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_13_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_23.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_24.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_25.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_14_26.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_15_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_1.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_2.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_3.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_4.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_5.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_6.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_7.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_8.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_9.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_10.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_11.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_12.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_13.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_14.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_15.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_16.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_17.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_18.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_19.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_20.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_21.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_22.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_23.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_24.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_25.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CE_16_26.cab
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/CRC.cab
```

#### Windows Embedded CE 6.0 Platform Builder Service Pack 1

```
https://www.microsoft.com/en-us/download/details.aspx?id=4097
```

#### Microsoft Device Emulator 2.0

```
https://www.microsoft.com/en-us/download/details.aspx?id=25191
```

#### Microsoft Virtual PC 2007

```
https://www.microsoft.com/en-us/download/details.aspx?id=4580
```

#### Visual Studio 2005 Service Pack 1 Update for Vista

```
https://www.microsoft.com/en-US/download/details.aspx?id=7524
```

#### Windows Embedded CE 6.0 Platform Builder

```
http://download.microsoft.com/download/a/0/9/a09e587c-4ff9-4a58-a854-56fe50b862b2/Windows%20Embedded%20CE%206.0.msi
```

### 虚拟机安装

向虚拟机分配40G，安装Windows XP SP3，然后安装Visual Studio 2005，密钥如下。

```
M3C9X-9K3Q9-DC8PX-B3YR3-BKQR8
```

安装完成后安装Visual Studio 2005 Service Package 1（KB926601）。然后安装Windows Embedded CE 6.0，密钥如下。

```
H8RQR-MMKRP-XFRFC-9HKGJ-82R6J
```

然后安装Windows Embedded CE 6.0 Platform Builder Service Pack 1、Microsoft Device Emulator 2.0、Microsoft Virtual PC 2007。注意，Microsoft Virtual PC 2007能在Windows XP/Vista/Server 2003 Standard上安装而无法在Enterprise上安装，Enterprise上只能装更为高级的Virtual Server 2005。

打开Visual Studio 2005，选择新建一个新安装的OS Design项目模板，进入向导页面。在Board Support Packages中选择`Device Emulator: ARMV4I`，设计模板选择PDA Device-Mobile Handheld，一路下一步完成创建。

右键单击项目名称，选择Properties，在Build options中取消勾选`Enable KITL(no IMGKITL=1)`。然后选中整个Solution，点击Build-Build Solutions进行编译。然后选择Target-Connectivity options，配置如下。修改完配置后点击Apply并关闭窗口。

|      项目     |          内容         |
|---------------|-----------------------|
| Target Device | CE Device             |
| Download      | Device Emulator (DMA) |
| Transport     | Device Emulator (DMA) |
| Debugger      | None                  |
| 屏幕尺寸      | 600*400，32色         |

选择Target-Attach device，开始附加到设备。模拟器正常启动后可以看到Windows CE系统的模拟器窗口。

选择Project-Add new SDK, 打开SDK属性页。在Emulator中将Configuration修改为Device Emulator ARMV4I Debug，填写必要的信息后点击确定，然后在项目窗口中右单击刚才创建的SDK，选择编译。

编译完成后生成一个msi安装文件，位于OSDesign项目文件夹下`SDKS/<SDKname>`目录中。关闭Visual Studio 2005并运行此SDK进行安装。

安装完成后打开Visual Studio 2005，选择菜单Tools-Device Emulator Manager，在WINCE6_EMU下单击刚才新建的CE项目，选择Connect，弹出模拟器窗口，CE系统启动成功。

## Windows Thin PC

### 下载

镜像为ISO格式。

```
ed2k://|file|en_windows_thin_pc_x86_697681.iso|1576980480|2D0E6A048EB3F314F556B4F0834A95E2|/
```

### 虚拟机安装

直接挂载安装即可。

### 实体机安装

进入PE并通过WinNTSetup安装即可。

## Citrix WinFrame 1.8

### 下载

```
https://winworldpc.com/product/winframe/18
```

### 虚拟机安装

直接挂载安装即可。

### 现成虚拟机

```
链接 / https://pan.baidu.com/share/init?surl=Aw1n9oH0HfEYyGvpU40u5A
提取码 / ile1
```

# 多系统启动策略

## 安装顺序

多系统的安装顺序一般是从低到高，以最高版本系统为主系统。如本机安装有MS-DOS 7.10、Windows 3.1、Windows 95、Windows NT 4.0、Windows 2000和Windows XP，则应该以Windows XP作为主系统。主系统应当为WinNT内核，需安装到主分区，并设置为激活分区。

如果系统未按照从低到高的顺序，则应当注意旧版系统对新版系统的引导覆盖问题。新版系统引导兼容旧版系统，而旧版系统引导不兼容新版系统，因此需要进行备份和替换。

比如在已安装Windows XP的电脑上安装Windows 2000，则Windows 2000安装程序将会覆盖Windows XP的引导。由于两者都是WinNT内核，因此引导方式完全相同，但由于Windows 2000的引导文件旧于Windows XP，会导致安装后无法引导至Windows XP。

对于WinNT内核而言，其引导文件为NTDETECT.COM和NTLDR，而BOOT.INI用于配置启动项。为解决以上问题，需要先备份Windows XP的NTDETECT.COM和NTLDR，在Windows 2000安装完成后替换旧版文件即可。

## 安装位置

由于MBR分区表最多只能支持四个主分区，因此其它系统需要安装到扩展分区中。其中NT内核的系统可以通过先安装到主分区，再通过恢复gho镜像或磁盘克隆的方式拷贝到扩展分区中，添加引导后可以正常运行。而以DOS为基础的其它系统只能安装到主分区中。

## 引导添加

在最高版本系统中，可以添加其它系统的引导项。

### WinNT内核

对于WinNT内核，引导信息写在BOOT.INI中，到主系统根目录编辑BOOT.INI即可。

#### 手动添加

将需要添加引导的系统根目录下的BOOT.INI内的引导条目复制到主系统的BOOT.INI即可，格式如下。

```
multi(0)disk(0)rdisk(0)partition(1)\WINDOWS="WinNT"  /noexecute=optin /fastdetect
```

#### 自动添加

用NTBOOTautoFix工具即可。

```
https://www.onlinedown.net/soft/1150737.htm
```

### Win 9.x/DOS内核

注意，如果有多个Win 9.x/DOS系统，则无论保存哪个系统的PBR，都将被引导到第一个Win 9.x/DOS系统。

#### 保存PBR

##### 通过DOS命令

进入到DOS环境，输入以下命令。

```
debug
-L 100 2 0 1 // 将C盘的引导扇区写入内存，其中2表示C盘（0-A盘，1-B盘，2-C盘，以此类推），需更换为要添加引导的分区数值
-R cx
CX 0000
:200 // 读取200个扇区
-N C:\BOOTSECT.DOS // 新建DOS文件
-W // 写入扇区内容到文件
-Q
```

##### 通过Diskgenius

打开新版Diskgenius，在需要操作的系统磁盘上选择编辑扇区内容，将扇区的前200个字节另存为bootsect.dos文件即可。

#### 添加条目

将得到的写有PBR的文件复制到主系统根目录，并手动在主系统的BOOT.INI添加引导项，格式如下。

```
C:\bootsect.dos="Win 9.x"
```

# 通用系统配置

## 补丁相关

### 下载

#### Microsoft Update Catalog

翻墙后在较新的Windows系统（如Windows 10）上用Internet Explorer打开Microsoft Update Catalog网站，会提示安装加载项。安装完成后在搜索框搜索系统名称即可下载所有官方补丁，下载时可关闭代理工具。Microsoft Update Catalog网站如下。

```
http://www.catalog.update.microsoft.com/home.aspx
```

#### Windows Legacy Update

```
http://win2k.org/wlu/wluen.htm
```

### 批量安装

#### 通过生成脚本

将需要安装的补丁放到同一目录。进入命令行，输入以下命令以得到文件夹内的所有文件路径。

```
dir /b >a.txt
```

打开得到的文本文档，删除`a.txt`一行，将`.exe`替换为`.exe /q`。保存后将文件后缀改为bat，然后双击运行即可。

一般而言，在命令行中输入补丁路径后，加上`/q`参数即可静默安装。对于大部分补丁而言，也可使用`/passive /norestart`。加`/?`参数能够查看补丁支持的参数选项。

#### 通过现成脚本

在补丁目录新建一个文本文档，将以下代码复制到文档中并保存为bat文件，双击运行即可。

```
@echo off 
for %%i in (*.exe) do %%i /r
```

## 系统激活

系统版本解释如下。

|     版本    |               说明               |
|-------------|----------------------------------|
| RTL Edition | 零售版，只能激活一台电脑         |
| VOL Edition | 批量授权版，可用MARK密钥永久激活 |

Windows 7以上可用KMSAuto激活系统，下载链接如下。

```
https://lanzous.com/i3mbn3c
```

## Dll修复

可用DirectX Repair、DLL Escort等。

```
// DirectX Repair
https://www.pcsoft.com.cn/soft/36821.html

// DLL Escort
https://www.lhdown.com/soft/28621.html
```

# Vmware配置

## 安装Vmware Tools

### Windows 9.x内核

Windows 2000及以前使用`VMware tools for winPre2k`，下载地址如下。

```
http://www.upantool.com/qidong/qtqd/7232.html
```

### Windows NT内核

包括Windows XP Starter Edition。

Windows 2000以后可直接在Vmware进行安装。如果安装出现问题，可从以下网站下载Vmware Tools镜像包。

```
https://packages.vmware.com/tools/releases/
```

### MS-DOS/Windows 3.1

从以下链接下载驱动程序包，并根据指示进行安装即可。

```
http://www.scampers.org/steve/vmware/#31pack
```

## 挂载虚拟机分区到主机系统

进入虚拟机设置，选择硬盘，点击映射，选择需要映射的虚拟机硬盘，取消勾选`以只读模式打开文件`，确定即可。

## 从虚拟机克隆到实体机

若在实体机上无法完成安装，可以先在虚拟机上安装，然后克隆到实体机上。注意虚拟机上的系统不要安装驱动或Vmware Tools，进入WinPE环境后将系统盘备份为GHO文件，然后从虚拟机拷贝出来即可。

如果Win8内核的WinPE无法识别系统盘，可以换一个WinPE，比如Win2000/WinXP内核的PE。

## 常见错误

### 安装Vmware Tool时出现「Setup was unable to upgrade the Windows Installer」

下载以下msi文件，启动到目录服务恢复模式，安装msi文件即可。

```
https://pan.baidu.com/s/1pLFkyQR
```

### 虚拟机挂载WinPE的ISO后无法进入到WinPE主页面

适当增加虚拟机内存，建议大于1GB。

### 打开虚拟机时显示不是有效的虚拟机配置文件

大多数情况为vmx文件损坏。进入虚拟机路径，删除vmx文件，打开vmware.log，找到`vmx| DICT --- CONFIGURATION`，复制该行以下内容到记事本，至`vmx| DICT --- USER DEFAULTS`为止（不含）。删除所有日期时间标识，如`Jan 24 23:13:15.438: vmx| DICT config.version = 8`改为`config.version = 8`。全部修改完成后，给所有行等号后的字符加上英文双引号，保存为`<虚拟机名>.vmx`，放到虚拟机目录，双击打开即可。

### Vmware异常关闭后无法打开虚拟机

关闭进程`vmware-vmx.exe`，然后删除虚拟机目录下的lck文件即可。

# 参考教程

## Install Windows/386 2.11 in VMware

```
https://medium.com/@anubis2591/install-windows-386-2-11-in-vmware-9cfcc2b6dd56
```

## Standard Mode: Fault in MS Dos Extender

```
https://it.toolbox.com/question/standard-mode-fault-in-ms-dos-extender-102506
```

## Invalid DPMI During Setup Of Windows ME

```
http://discussions.virtualdr.com/showthread.php?92799-Invalid-DPMI-During-Setup-Of-Windows-ME
```

## vmware异常关闭后导致虚拟机无法打开问题解决

```
https://blog.csdn.net/itwxming/java/article/details/99727215
```

## How do I enable Windows Wireless Manager for Win2k, Xp and Vista

```
https://networkoverload.com/guides/how_do_i_enable_windows_wireless_manager_win2k_xp_and_vista
```

## win98se install freezes

```
https://forums.techguy.org/threads/win98se-install-freezes.384921/
```

## Windows 2000 Un-Official Updates

```
http://greyghost.mooo.com/limestone/Web-Features/W2K_UU/
```

## DriverPacks for Windows 2000/XP/2003 (x86)

```
https://jackngblog.blogspot.com/2011/10/driverpacks-for-windows-2000xp2003-x86.html
```

## Windows 2000 Sp4 Files

```
http://retro.remotecpu.com/win2k.php
```

## Citrix Winframe 1.8基于Window

```
https://tieba.baidu.com/p/6289561988?red_tag=2476268029
```

## Win95/98 Shutdown Problems

```
http://educ.jmu.edu/~jarvislb/utils/shutdown.html
```
