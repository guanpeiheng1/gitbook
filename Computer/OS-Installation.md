# 红星OS

## 下载

```
链接 / https://pan.baidu.com/s/1Y-F4xulXJAmnfF7eQ1vFag
提取码 / 23om

链接 / https://pan.baidu.com/s/1i3iSOCH#list/path=%2F
```

## 安装

虚拟机类型选择XP。

### 通过挂载光盘

挂载ISO后点击蓝色按钮一路下一步即可。安装过程中应当为用户添加密码，网络配置应当设置为DHCP。

若感觉韩文比较困难，修改镜像/isolinux/isolinux.cfg里的lang=ko为lang=en即可用英文安装。也可在ISO启动后重复按Esc几秒钟，然后输入以下命令，按回车保存后按B引导即可。

安装的前一步需要点击手动配置，将所有组件都选上。

```
linux lang=en
```

### 通过从Windows启动

将ISO里的文件复制到Documents文件夹，编辑`m_rs.lst`文件，将lang=ko替换为lang=en并保存。运行install.exe，单击蓝色按钮以重新启动系统并进入安装。

## 配置

### 获取Root权限

点击拓展坞上的应用程序文件夹，单击没有扳手图标的普通文件夹，再单击有扳手图标的文件夹。然后单击终端，输入以下命令。

```
rootsetting
```

单击锁按钮并输入密码，然后选中复选框，输入密码后关闭对话框。以后即可在终端输入`su`获取Root权限。

### 设置系统语言为英语

打开终端并输入以下命令，重启即可。

```
su
sed -i 's/ko_KP/en_US/g' /etc/sysconfig/i18n /usr/share/config/kdeglobals
```

### 设置浏览器语言为英语

打开Naenara Browser，转到倒数第二个菜单，选择第三个选项。转到倒数第二个选项卡，选择Korean（ko-KP）加载项，单击第一个按钮将其禁用，然后单击黄色栏上的按钮以重新启动浏览器。浏览器将询问您是否确认打开了多个选项卡，请单击蓝色按钮（如果已打开）。

### 设置Sogwang Office语言为英语

打开终端并输入以下命令即可。

```
su
cd /Applications/SGOffice.app/Contents
rm RedStar/resource/*ko.res share/registry/Langpack-ko.xcd
```

### 设置APM Manager语言为英语

在浏览器输入以下网址可打开APM管理器。

```
http://localhost:10000
```

打开终端并输入以下命令即可。

```
su
sed -i 's/ko_KP.UTF-8/en/g' /etc/sat/config /usr/share/sat/web-lib.pl
service sat restart
```

### 清除防火墙规则

打开终端并输入以下命令即可。

```
su
rm /etc/sysconfig/iptables
service iptables restart
```

### 删除系统文件修改检测器

打开终端并输入以下命令，重启即可。

```
su
rm /usr/share/autostart/intcheck_kde.desktop
```

### 禁用恶意组件

克隆以下仓库并打包成ISO镜像，然后加载到虚拟机中。

```
https://github.com/takeshixx/redstar-tools
```

在终端执行以下命令以复制文件夹，并执行脚本。

```
su
cp -r [源位置] [目标位置]
[目标位置]/defuse.sh
```

# Ylmf OS 3.0

## 下载

```
链接 / https://pan.baidu.com/s/10jHOYNAGuhayQu8IrzkY0w 
提取码 / ldnh
```

## 安装

### Vmware Tool安装

下载链接如下。

```
https://www.7down.com/soft/312275.html
```

在终端切换到安装包目录后运行以下命令。

```
ls
mkdir /vmtools
tar xvf VMwareTools-10.0.0-2977863.tar.gz -C /vmtools
cd /vmtools/
ls
cd vmware-tools-distrib/
./vmware-install.pl
reboot
```

# Sigma OS 3.0

## 下载

```
https://techvortal.pl/viewtopic.php?t=2883
```

## 安装

### Vmware Tool安装

与Ylmf OS 3.0相同。

# ReactOS

```
https://reactos.org/
https://sourceforge.net/projects/reactos/files/ReactOS/
```

# 参考教程

## Notes on Red Star OS 3.0

```
https://richardg867.wordpress.com/2015/01/01/notes-on-red-star-os-3-0/
https://richardg867.wordpress.com/2020/03/31/more-notes-on-red-star-os-3-0/
```


