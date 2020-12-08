---
title: Android使用技巧总结
categories: Technology
abbrlink: Android-Skills
date: 2020-04-22 22:54:29
tags:
---

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gegao3bnq6j31800kvaax.jpg)

安卓使用技巧。

<!-- more -->

# 破解软件

## 软件库

```
https://590m.com/dir/845023-30099335-9e922b
https://306t.com/dir/14633726-31244416-26771f
https://www.uptodown.com/android
```

## 微信抢红包插件

```
https://github.com/geeeeeeeeek/WeChatLuckyMoney
```

# Root/Busybox/Xposed

翻墙后安装Google Play，然后安装Busybox。准备好系统原镜像，安装Magisk Manager并打开，通过修补boot的方式得到新镜像，并通过Odin进行线刷即可。

# 虚拟机

在安卓手机上通过安装相应的APP，可以运行电脑系统。

## Bochs运行XP

### 安装前的准备

#### Bochs安装包

```
https://pan.baidu.com/s/1i4FDLGT
```

#### 适用于Bochs的XP镜像

```
http://pan.baidu.com/s/1gfj4a2Z
```

### 安装流程

将apk格式的Bochs安装包和img格式的Windows系统镜像传送到手机，放置于任何目录下均可。安装Bochs，将`ata0-master`前面的复选框选中，然后点击`select`。选择img格式的系统镜像文件Windows.img，其他项无需更改。点击顶部的`HARDWARE`选项卡，按照以下设置。

```
CPU Model无需更改
Chipset选择i440fx
内存设置为517MB（可根据手机配置调整）
VGA Card设置为cirrus_5446
Sound Card设置为sb16
Ethernet Card设置为rtl8029
PCI设置中，Slot1设置为cirrus，Slot2设置为ne2k，Slot3设置为es1370，Slot4设置为voodoo，Slot5设置为none
```

点击顶部的MISC选项卡，将Full screen前面的复选框选中以使Windows可以全屏运行。点击右上角绿色的Start按钮以启动Windows。

## Limbo运行XP

### 安装前的准备

#### Limbo安装包

```
http://pan.baidu.com/s/1pK7ua3D
```

#### 适用于Limbo的XP镜像

```
http://pan.baidu.com/s/1miRtjJU
```

### 安装流程

将apk格式的Limbo安装包和qcow2格式的Windows系统镜像传送到手机，放置于任何目录下均可。

安装Limbo并打开，加载虚拟机下选择新建，在弹出对话框中输入虚拟机名称，然后按照以下设置。

```
用户界面项选择SDL
CPU型号项选择athlon
CPU核心数设置为2（可根据手机配置调整）
运存项设置为64（可根据手机配置调整）
光驱和软盘A和软盘B项留空
硬盘A项点击后选择打开，选择qcow2格式的镜像文件
硬盘B项留空
引导设备项选择硬盘
网络配置项选择User
网卡项选择rtl8139
显卡配置项选择cirrus
声卡配置项选择sb16
高级设置全部留空
勾选多线程AIO右侧的复选框（警告可以无视）
```

全部设置完毕后，回到顶部，点击运行以启动Windows。

## APQ模拟器运行XP

### 安装前的准备

#### APQ安装包

```
https://www.cr173.com/soft/692055.html
```

#### Zarchiver安装包

```
https://dl.pconline.com.cn/download/1815731.html
```

#### 适用于APQ的XP镜像

```
https://pan.baidu.com/s/1_aoxWWi0mfN1cXyS3V4I6g
```

### 安装流程

获取root权限。打开APQ，点击data分区，等待文件处理完成。

打开zarchiver（自己百度下载，不知道为什么复制不了链接），找到手机内存的APQ打开，找到kolibri.conf文件。将以下位置的文件名改为下载好的镜像文件即可。

```
#第一块硬盘，硬盘镜像文件为xp.qcow2，应位于SD卡上的APQ主目录下或写绝对路径
-fda kolibri.img
```

## Termux运行Linux
 
无需root，安装完成后打开即可使用。

# LineageOS

对于LG G3的教程如下。

```
https://linustechtips.com/topic/1058206-lg-g3-lineageos-tutorial/
```

