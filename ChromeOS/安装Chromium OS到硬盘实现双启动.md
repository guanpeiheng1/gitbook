---
title: 安装Chromium OS到硬盘实现双启动
categories: Chromium OS
abbrlink: Chromium-OS-Installation
date: 2019-11-25 22:24:29
tags:
---

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g9apspq7y8j30u00mijs7.jpg)

Chromium OS是Google开发的一款基于Linux内核的系统，需要连网才能使用，整个系统的架构等同于一个Chromium浏览器。

<!-- more -->

# 安装

打开以下链接下载Chromium的镜像。

```
https://chromium.arnoldthebat.co.uk/index.php?dir=special&order=modified&sort=desc
```

将Chromium的镜像利用Etcher拷录到U盘中。重启进入USB的Chromium OS，激活Google账号需要使用代理，可通过Google搜索`免费代理节点`找到。

进入Linux系统，在终端运行以下命令安装Gparted。

```
sudo apt-get install gparted
```

打开Gparted，选择要安装到的硬盘，在未分配空间上新建以下分区。

|       |     分区大小    | 分区格式 | 标签和名称 |
|-------|-----------------|----------|------------|
| 分区1 | 32MB            | FAT16    | EFI-SYSTEM |
| 分区2 | 5000MB          | EXT2     | ROOT-A     |
| 分区3 | Chrome OS系统盘 | EXT4     | STATE      |

选择USB驱动器，记下分区位置`/dev/sd*`。

打开终端并输入以下命令，以创建文件夹并挂载U盘系统盘后，复制系统文件到需要安装的硬盘。

```
mkdir state
mkdir root
mkdir efi
mkdir localroot
mkdir localefi
mkdir localstate

// sdb改为USB的位置
// sda改为移动分区的位置
// sda*是EFI-SYSTEM/ROOT-A/STATE的分区位置
// [username]是Linux的用户名
sudo mount /dev/sdb12 /home/[username]/efi
sudo mount /dev/sda* /home/[username]/localefi
sudo cp -av /home/[username]/efi/* /home/[username]/localefi

sudo mount /dev/sdb3 /home/[username]/root
sudo mount /dev/sda* /home/[username]/localroot
sudo cp -av /home/[username]/root/* /home/[username]/localroot

sudo mount /dev/sdb1 /home/[username]/state
sudo mount /dev/sda* /home/[username]/localstate
sudo cp -av /home/[username]/state/* /home/[username]/localstate

sudo gedit /home/[username]/localroot/usr/sbin/write_gpt.sh
```

在打开的文件中搜索`base_vars`以及`partition_vars`，只保留EFI-SYSTEM、ROOT-A、STATE以及对应数字的引导信息（共六个），然后修改每个分区的参数（在Gparted可查看）。示例如下。

```
PARTITION_SIZE_ROOT_A=5242880000
  RESERVED_EBS_ROOT_A=0
     DATA_SIZE_ROOT_A=5242880000
        FORMAT_ROOT_A=
     FS_FORMAT_ROOT_A=ext2
    FS_OPTIONS_ROOT_A=""
 PARTITION_NUM_ROOT_A="6"
     PARTITION_SIZE_3=5242880000
       RESERVED_EBS_3=0
          DATA_SIZE_3=5242880000
             FORMAT_3=
          FS_FORMAT_3=ext2
         FS_OPTIONS_3=""
      PARTITION_NUM_3="6"
```

修改完成后保存，在终端输入以下命令以复制分区文件到主目录。

```
cp -av /home/[username]/localroot/usr/sbin/write_gpt.sh /home/[username]
```

# 引导

## 原生引导

安装完成后重新启动，拔出U盘，在BIOS里会多出Chromium OS的启动项，启动即可。

若需要修改Chromium的grub引导项，可打开Linux，在终端输入以下命令后，记下ROOT-A的`PARTUDID`。

```
sudo blkid
```

打开`EFI-SYSTEM`分区的`efi/boot/grub.cfg`，只留第一个menuentry，将`PARTUUID=`的值修改为记下的UUID，Menuentry里的名字改为Chromium OS即可。

## Clover引导

配置config.plist，扫描项中勾选`Linux`，隐藏项中添加`vmlinuz`即可。
