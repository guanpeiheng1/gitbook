---
title: 
categories: 
abbrlink: 
date: 2020-04-10 18:41:29
tags:
---

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gegariofn3j31c00u04qp.jpg)

分区表、引导方式和不同方式引导系统。

<!-- more -->

# 分区表与引导

## 分区表

`分区表`用于存储分区信息。

### MBR分区表

MBR分区表会在磁盘的第一个扇区写入引导代码，这些代码用来引导系统的启动，该扇区被称为主引导扇区。主引导扇区共512字节，写有主引导记录MBR（446字节）、磁盘分区表项DPT（16×4=64字节）以及结束标志AA55，小端规则为55AA（2字节），被称为幻数。

由于DPT只有64字节，MBR分区表最多只能有四个分区（三个主分区加一个扩展分区，或四个主分区）。而MBR的代码决定其引导类型，包括grub、Windows NT 5.0等，可视作是规定启动引导器的代码。

每个分区的分区头部有本分区的引导信息，对于主分区而言称为PBR，对于扩展分区而言称为EBR。在扩展分区内可以进一步划分出无数个逻辑分区，主要是通过在每个逻辑分区的起始扇区建立一个虚拟MBR。注意，扩展分区/逻辑分区不能被设为激活分区。

### GUID分区表

为了兼容性，GUID分区表即GPT磁盘的第一个扇区仍然为MBR（被称为保护MBR），然后是GPT头。与MBR不同的是，GPT磁盘的保护MBR只有一个标识为0xEE的分区，以标注这块硬盘使用GPT分区表。如果在这段代码中储存GPT分区表的一部分分区，则可以使不支持从GPT启动的操作系统从这个MBR启动，启动后只能操作MBR分区表中的分区。

GPT需要有一个专门的`EFI分区`存放引导文件，这个EFI分区一般是FAT32或FAT16格式。EFI分区可以有多个，也可以只有一个，系统引导文件可以放到同一个地方，也可以分散放置。引导启动时，不同的系统文件会被识别为不同的引导项。

引导文件写有启动磁盘信息，主要是识别系统种类以及磁盘的唯一标识GUID。每个分区也有分区表，其中前16字节是分区类型GUID，接下来的16字节是分区唯一标识GUID。GPT分区表主要通过GUID识别不同分区。

## 引导方式

### BIOS

开机时，BIOS开电自检，然后读取MBR，根据MBR的引导代码启动系统。一般而言，BIOS会寻找可引导的活动分区，然后装入活动分区的PBR到内存，并把启动权移交系统。

### UEFI

开机时，电脑读取UEFI，UEFI寻找EFI分区并根据EFI分区的引导文件启动系统。

### 分区表与引导方式的对应关系

一般而言BIOS使用MBR分区表，而UEFI使用GPT分区表。然而由于GPT分区表也有MBR，因此理论上BIOS+GPT是可行的。同理UEFI+MBR也是可行的。

### 查看本机引导方式

在Windows下Win+R打开运行，输入`msinfo32`并回车，在打开的窗口中`系统摘要`一栏显示为传统即为BIOS模式，显示为UEFI即为UEFI模式。

# 移动硬盘安装多系统

## 安装前的准备

全盘格式化并转换为`GUID分区表`。新建一个200-300MB的EFI分区，后分别建立Windows分区（NTFS格式）、MacOS分区（APFS格式）、Ubuntu分区（NTFS分区），其余留空。

## Windows

### 安装

#### 通过Ghost

下载好Windows 10的gho版，在Windows或WinPE下用Ghost将gho镜像恢复到所要安装的分区。

安装完成后，由于EFI分区内没有引导，无法启动Windows，故需从任一Windows系统下复制一份EFI到分区中，然后打开`BOOTICE`进行引导修改。选择`BCD编辑`-`其它BCD文件`，路径为`EFI\Microsoft\BOOT\BCD`，点击`智能编辑模式`，启动磁盘/分区改为装好Windows的移动硬盘/分区，两个保存按钮均点击后退出即可。

#### 通过WinNTSetup

下载Windows 10的ISO版本，在Windows或WinPE下打开`WinNTSetup安装器`。镜像选择ISO里的`\Sources\install.wim`，引导驱动器选择EFI分区，安装驱动器选择安装Windows 10的分区。保证引导驱动器右侧的三个选项均无红灯后点击安装，完成后重启电脑即可。

### 常见问题

#### 弹出「计算机意外地重新启动或遇到错误」

按下`Shift+F10`打开命令提示符，键入`regedit`并回车，启动注册表编辑器。转到`HKEY_LOCAL_MACHINE\SYSTEM\Setup\Status\ChildCompletion`，双击右侧窗格中的`setup.exe`，将数值数据从`1`修改为`3`，单击确定以应用更改。退出注册表编辑器并重新启动计算机即可。

#### 重启后出现defaultuser0用户

在Windows或WinPE下打开`NTPWEdit`，加载移动硬盘分区的`SAM`，路径为`\Windows\System32\config`，把所有用户密码都删掉，保存后重新进入系统即可。

## Mac

参见黑苹果安装教程，安装磁盘改为移动硬盘即可。注意不要安装不必要的驱动，config.plist中不要放显卡和声卡注入项。

将Clover文件夹与Microsoft文件夹放于同级，并将BOOT/BOOTX64.efi替换成Clover的BOOTX64.efi即可引导。

## Ubuntu

参见Linux安装教程，安装磁盘改为移动硬盘。同理，让EFI分区的EFI/BOOT下有ubuntu文件夹即可引导。

## Chrome OS

参见Chrome OS安装教程。注意，所有操作均从未分配空间中进行，因此最后移动硬盘将有两个EFI分区，一个用于引导Windows、Mac与Ubuntu，另一个用于引导Chrome OS。

## 在Ubuntu引导菜单添加Clover启动项

进入Ubuntu系统，在终端输入以下命令查询磁盘，记下EFI分区的`UUID`。

```
sudo blkid
```

编辑系统盘的`/etc/grub.d/40_custom`，在文件末尾添加以下内容并保存。

```
menuentry 'Mac OS X' --class os {
	insmod part_gpt
	insmod fat
	set root='hd0,gpt1' // 此处修改为EFI分区位置
search --no-floppy --fs-uuid --set=root C14D-581B // 此处修改为EFI分区的UUID后八位
	chainloader /efi/Boot/bootx64.efi
}
```

然后在终端输入以下命令更新grub引导器即可。

```
sudo update-grub
```

## 手动启动引导项的方法

若引导项丢失，可通过EFI Shell启动对应的系统引导文件。按顺序输入以下命令即可。

```
map -r -b
// 进入所需要的驱动器，此处为fs7
fs7:
ls
cd efi\boot\
bootx64.efi
```

# grub4dos制作多系统引导菜单

## 安装grub4dos启动管理器

将下载好的grub4dos包内的grldr、grldr.mbr和menu.lst复制到C盘根目录，其中grldr和grldr.mbr为引导文件，menu.lst为菜单配置文件。

进入WinPE环境，设置MBR为grub4dos 0.46a，重启即可进入grub4dos界面。也可直接在C盘修改boot.ini，添加以下引导项即可（未经试验）。

```
C:\grldr.mbr="grub4dos"
```

## 修改引导菜单

修改menu.lst即可增加或删除引导项。

### WinNT内核

```
// (hd0,0)为要引导的系统分区
title WinNT
fallback +1
rootnoverity (hd0,0)
chainloader /ntldr
boot
savedefault --wait=2
```

### DOS / Win9.x内核

```
// (hd0,1)为需要启动的分区
// 除要启动的分区外，其余分区需要隐藏
title Win9x/DOS
fallback +1
unhide (hd0,1)
hide (hd0,2)
...
hide (hd0,n)
rootnoverity (hd0,1)
chainloader (hd0,1)/io.sys
boot
savedefault --wait=2
```

### 引导ISO/IMG镜像

```
// ISO后缀可以改为IMG
title ISO
map (hd0,0)/name.iso (fd0)
map --hook
chainloader (fd0)+1
rootnoverity (fd0)
```

# BIOS+GPT引导

## U盘引导模拟UEFI

通常使用Clover在BIOS下模拟UEFI。在Windows下载BootDiskUtility（BDU），将Clover的taz.lzma镜像文件放到同级目录。打开BDU，点击`Options`，镜像已自动选中。若无法选择镜像，则更换另一版本的BDU。选择要制作的U盘并点击`Format`即可。

若启动时出现`6_`且卡住，则需切换到Mac系统，注意需要关闭系统的SIP。打开Clover的pkg安装包，安装位置选择U盘，点击自定，只选择CloverEFI下的BiosBlockIO，进行安装。再次从U盘启动，理论上出现`7_`并成功进入。

## 将模拟UEFI写入硬盘

下载`模拟UEFI`压缩包内的文件，进入WinPE环境。将磁盘转换为GPT分区，开头划分出一个200-300MB的EFI分区，可以不建MSR分区。将EFI分区格式化为FAT32，复制压缩包内`EFI分区根目录`的文件到新建的EFI分区的根目录下。

运行`bootice`文件夹里的`BOOTICE`程序，目标磁盘选择GPT磁盘，点击主引导记录，选择`恢复MBR`，恢复文件选择文件夹内的`boot0ss`。恢复完成后回到主界面，点击`分区引导记录`，目标分区选择EFI分区，点击`恢复PBR`，恢复文件选择文件夹内的`boot1f32alt`。完成后重启即可进入模拟UEFI环境。

若需向GPT磁盘中在安装Windows，则可以先行分出另一个EFI分区，并对此分区添加盘符，利用WinNTSetup安装系统时将此EFI分区设为引导驱动器，安装完成后再次进行上述MBR和PBR的恢复操作。确认系统能够正常进入后，可保留当前EFI结构或者将EFI分区中的Microsoft文件夹复制到模拟UEFI所安装的EFI分区。注意，系统安装完成后不能进行更新操作，否则由于Windows更新会重写EFI分区，更新完成后会无法进入系统。

完成后可以精简和更新EFI分区的内容。如果使用的是64位的UEFI，则可删掉driverIA32.efi，同时可以将CLOVERX64.efi替换为最新版本。

注意，如果需要用Clover引导MBR磁盘下的Windows，则需要在drivers64UEFI文件夹里放置NTFS.efi，并编辑config.plist，在Scan标签页下勾选`Legacy`。

# 关闭BIOS安全启动

从以下地址下载镜像文件，并用Etcher写入U盘。在具有安全启动功能的PC上的首次启动将显示访问冲突消息框。按确定，然后选择`从文件注册证书`，选择ENROLL_THIS_KEY_IN_MOKMANAGER.cer并确认证书注册即可。

```
https://github.com/ValdikSS/Super-UEFIinSecureBoot-Disk/releases
```

# 引导选择工具

选择您要启动哪个efi。

```
https://github.com/jief666/BootloaderChooser
https://jief-machak.gitbook.io/cloverhelp/initial-setup
```




# 相关下载

## 模拟UEFI工具包

```
https://pan.baidu.com/s/1bnCNCTp#list/path=%2F
```

# 参考教程

## 装系统不求人——硬盘的秘密深入

```
https://www.paincker.com/sysinst-harddisk-details
```

## Dual Booting DOS and Windows 95 : Follow-up

```
http://retropcbuilder.blogspot.com/2016/11/dual-booting-dos-and-windows-95-follow.html
```

## 怎么让老电脑实现UEFI启动NVME SSD固态硬盘进系统方法

```
http://www.upantool.com/jiaocheng/ssd/2018/13568.html
```

## 无需U盘，任何机器可以用上32/64bit两种架构的UEFI

```
http://bbs.pcbeta.com/viewthread-1536721-1-1.html
```

## How to Boot Linux ISO Images Directly From Your Hard Drive

```
https://www.howtogeek.com/196933/how-to-boot-linux-iso-images-directly-from-your-hard-drive/
```
