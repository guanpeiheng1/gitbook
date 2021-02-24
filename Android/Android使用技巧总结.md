---
title: Android使用技巧总结
categories: Android
abbrlink: Android-Skills
date: 2020-04-22 22:54:29
tags:
---

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gegao3bnq6j31800kvaax.jpg)

安卓使用技巧。

<!-- more -->

# 软件

## 破解软件库

```
https://590m.com/dir/845023-30099335-9e922b
https://306t.com/dir/14633726-31244416-26771f
https://www.uptodown.com/android
```

## 微信抢红包插件

```
https://github.com/geeeeeeeeek/WeChatLuckyMoney
```

# 系统

## LineageOS

LineageOS可在旧款Android手机上流畅运行。

以LG G3为例，需要先将手机降级至KitKat。打开USB调试模式并选择连接模式为MTP，然后使用线刷的一般方法完成刷机。

然后使用Root工具获取系统root权限，此处使用PurpleDrake。

```
https://forum.xda-developers.com/t/root-root-your-lg-g3-easily-with-purpledrake-lite-osx-linux-windows.2821000/
```

寻找本机对应的TWRP固件，对于该机型可采用以下固件。下载完成后放到手机中。

```
https://forum.xda-developers.com/t/recovery-3-3-1-unofficial-twrp-for-lg-g3.3813552/
```

从Google Play商店中下载TWRP，并使用固件完成安装。安装后关闭手机，进入恢复模式，此时应当出现TWRP Recovery界面。进行备份后选择Wipe-Advanced Wipe，选择Dalvik/ART Cache、Cache、Data、Internal Storage、System并完成擦除。

通过以下链接下载相应版本的LineageOS和Open Google Apps，复制到手机后用TWRP安装LineageOS固件即可。

```
https://wiki.lineageos.org/devices/
https://opengapps.org/
```

## 氧OS

适用于一加手机，以OnePlus 7为例。在电脑下载ADB工具包。

在手机上进入设置-关于手机，点击7次版本号启用开发者选项。启用开发者选项后，Google服务框架会被自动取消隐藏。然后前往设置-系统-开发者选项，启用OEM 解锁和高级重启。

长按电源键，从电源菜单中选择引导器模式，手机将会自动重启进入Bootloader模式。打开电脑的终端，使用以下命令解锁Bootloader。

```
fastboot devices # 检查设备是否连接
fastboot oem unlock
```

手机会提示解锁Bootloader的风险警告信息，需要用音量键操作选择是否解锁，并使用电源键确认。解锁后手机会自动抹掉所有数据并执行重启。

到TWRP官网下载OnePlus 7的TWRP，链接如下。

```
https://twrp.me/oneplus/oneplus7.html
```

再次通过上述方法使手机进入Bootloader模式。在电脑终端使用以下指令以临时启动TWRP。

```
fastboot boot [TWRP的img包路径]
```

正常情况下，手机会临时进入一次TWRP。若未正常进入，则可先继续以下步骤以刷入氧OS。

下载适用于OnePlus 7的氧OS，如下。其中国行、印度和全球版本的OnePlus 7需要刷GM57AA（美版、印版）的氧OS，而GM57BA（欧版）的由于欧洲运营商频段覆盖不同，基带存在差异。

```
https://forum.xda-developers.com/t/oneplus-7-rom-ota-oxygen-os-repo-of-oxygen-os-builds.3937152/
```

氢氧OS可以无缝互刷，但是不能从高版本降级到低版本。若当前的氢OS并非最新，则可直接将氧OS的固件导入手机存储中，直接在设置-系统-系统升级-右上角齿轮按钮-本地升级中选择氧OS固件以升级。

若上述方法失败，可使用氢氧互刷工具，如下。

```
http://www.oneplusbbs.com/thread-2805359-1.html
```

如果已经进入临时TWRP，也可直接在TWRP中刷入氧 OS，但是注意刷完氧OS后一定先不要重启。如果之前无法进入临时 Bootloader，升级到氧OS 9.5.4以后Bootloader已经可以使用fastboot boot，可以直接通过上述方法临时进入TWRP。

完成刷入后，需将TWRP包倒入到手机存储并在TWRP中刷入，使TWRP在手机中持久化。安装包会在AB两个分区中都安装一次TWRP，这样无论手机从哪个分区启动都可以进入TWRP。

下载最新版的Magisk安装包和Magisk Manager并导入手机存储，链接如下。

```
https://github.com/topjohnwu/Magisk
```

在TWRP中直接刷入Magisk的zip安装包。刷入成功后重启手机即可进入氧OS，并且已经安装好Magisk，从手机存储中安装Magisk Manager即可。

如果希望下一次通过TWRP刷入ROM时要保留所有TWRP和Magisk，需要在刷入ROM之前尽量关闭所有Magisk 模块，将ROM、TWRP包和magisk安装包全部导入手机存储中，按顺序刷入后重启手机。

如果希望在系统内升级并保留TWRP和Magisk，需要将ROM、TWRP包和magisk安装包全部导入手机存储中，使用系统设置中的系统升级来升级系统后，在Magisk Manager的Modules页面，通过+按钮刷入TWRP包，然后点击Magisk旁边的Install-Install-Direct Install以及Install-Install-Install to Inactive Slot，重启手机。

## Root/Busybox/Xposed

翻墙后安装Google Play，然后安装Busybox。准备好系统原镜像，安装Magisk Manager并打开，通过修补boot的方式得到新镜像，并通过Odin进行线刷即可。

# 虚拟机

在安卓手机上通过安装相应的APP，可以运行电脑系统。

## Bochs

通过以下链接下载Bochs安装包和适用于Bochs的XP镜像。

```
https://pan.baidu.com/s/1i4FDLGT
http://pan.baidu.com/s/1gfj4a2Z
```

将apk格式的Bochs安装包和img格式的Windows系统镜像传送到手机，放置于任何目录下均可。安装Bochs，将`ata0-master`前面的复选框选中，然后点击`select`。选择img格式的系统镜像文件Windows.img，其他项无需更改。点击顶部的`HARDWARE`选项卡，按照以下设置。

|      选项     |             设置            |
|---------------|-----------------------------|
| CPU Model     | /                           |
| Chipset       | i440fx                      |
| 内存          | 517MB（可根据手机配置调整） |
| VGA Card      | cirrus_5446                 |
| Sound Card    | sb16                        |
| Ethernet Card | rtl8029                     |
| PCI/Slot1     | cirrus                      |
| PCI/Slot2     | ne2k                        |
| PCI/Slot3     | es1370                      |
| PCI/Slot4     | voodoo                      |
| PCI/Slot5     | none                        |

点击顶部的MISC选项卡，将Full screen前面的复选框选中以使Windows可以全屏运行。点击右上角绿色的Start按钮以启动Windows。

## Limbo

通过以下链接下载Limbo安装包和适用于Limbo的XP镜像。

```
http://pan.baidu.com/s/1pK7ua3D
http://pan.baidu.com/s/1miRtjJU
```

将apk格式的Limbo安装包和qcow2格式的Windows系统镜像传送到手机，放置于任何目录下均可。

安装Limbo并打开，加载虚拟机下选择新建，在弹出对话框中输入虚拟机名称，然后按照以下设置。

|       选项       |                   设置                  |
|------------------|-----------------------------------------|
| 用户界面项       | SDL                                     |
| CPU型号项        | athlon                                  |
| CPU核心数        | 2（可根据手机配置调整）                 |
| 运存项           | 64（可根据手机配置调整）                |
| 光驱/软盘A/软盘B | /                                       |
| 硬盘A            | 点击后选择打开，选择qcow2格式的镜像文件 |
| 硬盘B            | /                                       |
| 引导设备         | 硬盘                                    |
| 网络配置         | User                                    |
| 网卡             | rtl8139                                 |
| 显卡配置         | cirrus                                  |
| 声卡配置         | sb16                                    |
| 高级设置         | /                                       |
| 多线程AIO        | 勾选右侧复选框（警告可以无视）          |

全部设置完毕后，回到顶部，点击运行以启动Windows。

## APQ

通过以下链接下载APQ安装包、Zarchiver安装包和适用于APQ的XP镜像。

```
https://www.cr173.com/soft/692055.html
https://dl.pconline.com.cn/download/1815731.html
https://pan.baidu.com/s/1_aoxWWi0mfN1cXyS3V4I6g
```

获取root权限后打开APQ，点击data分区，等待文件处理完成。

打开zarchiver，打开APQ/kolibri.conf，将以下位置的文件名改为下载好的镜像文件即可。

```
#第一块硬盘，硬盘镜像文件为xp.qcow2，应位于SD卡上的APQ主目录下或写绝对路径
-fda kolibri.img
```

## Termux
 
无需root，打开即可在Android上运行Linux。

### 更换软件源

输入以下命令以打开配置文件。

```
vi $PREFIX/etc/apt/sources.list
```

修改安装源如下，保存并退出。

```
deb https://mirrors.tuna.tsinghua.edu.cn/termux stable main
```

运行以下命令完成更新。

```
pkg update
pkg upgrade
```

### SSH连接

输入以下命令。

```
// 安装SSH服务
pkg install openssh

// 设置密码
passwd

// 查询手机IP
ifconfig

// 查询当前用户
whoami

// 开启SSH
sshd -p 9000

// 确认SSH服务的监听端口
netstat -ntlp | grep sshd
```

完成后即可根据信息进行连接。

### 运行程序

SD卡目录的文件不具有可执行权限，需要拷贝到系统的/home文件夹并修改权限才可运行，示例如下。

```
cp Demo /home/Demo
cd /home
chmod 777 Demo

// 运行
./Demo
```

### 优化终端

可用zsh代替bash作为默认shell。通过以下命令即可。

```
sh -c "$(curl -fsSL https://github.com/Cabbagec/termux-ohmyzsh/raw/master/install.sh)"
```

可通过修改以下文件更改配色。

```
vi ~/termux-ohmyzsh/install.sh
```

### 软件库

#### waifu2x

用于图片放大。下载并直接运行即可。

```
https://github.com/tanyiok1234/waifu2x_srmd-ncnn-vulkan-termux-binary
```


### 常用命令

#### 访问SD卡目录

```
termux-setup-storage
```

### 完整Linux

Termux不是完整Linux，但可配合其它APP实现完整Linux的安装。

#### 通过AnLinux

##### 安装

安装AnLinux后打开，选择需要安装的Linux发行版。复制指令并在Termux的终端中运行即可。以Debian为例，完成后在Termux可通过以下指令启动系统。

```
./start-debian.sh
```

修改以上脚本，将以下行的注释取消即可访问手机自身的存储文件系统。

```
command+=" -b /sdcard"
```

##### 桌面环境配置

安装前需先启动系统。在AnLinux中按照步骤操作即可。

完成安装后需要安装VNC Viewer，然后在Termux中运行以下命令以开启VNC。

```
vncserver-start

// 若出现错误，需要先输入以下指令停止VNC
vncserver-stop
```

复制输出的地址到VNC Viewer，此处以`localhost:1`为例，密码为开启VNC时所设置的密码。完成后连接即可。

#### 通过atilo

##### 安装

输入以下命令。

```
pkg install proot
termux-chroot
echo "deb [trusted=yes] https://yadominjinta.github.io/files/ termux extras" >> $PREFIX/etc/apt/sources.list
pkg in atilo-cn
```

通过以下命令查看能够安装的版本。

```
atilo list
```

以安装Debian为例，命令如下。

```
// 安装
atilo install debian

// 卸载
atilo remove debian
```

注意，如果下载失败，需要先通过以下命令在手机的tmp目录中删除安装包，否则该脚本会直接跳过下载。

```
cd ~/.atilo
ls
rm -r tmp
```

完成安装后运行以下命令启动系统。

```
termux-chroot
startdebian
```

##### 环境配置

###### JAVA

```
apt search openjdk
apt install default-jdk
```

###### python

通过以下命令安装。

```
apt install python3 python3-pip
```

打开~/.pip/pip.conf，修改为以下内容以更换pip的软件源。

```
[global]
index-url =http://pypi.douban.com/simple  
[install]
trusted-host=pypi.douban.com
```

### 环境配置

#### Java

若未安装完整Linux，可通过以下命令。

```
pkg install ecj
pkg install dx
```

#### Fortran

需要安装完整Linux。输入以下命令以修改软件源。

```
vim $PREFIX/etc/apt/sources.list
```

在文件末尾添加以下语句，保存并退出。

```
deb https://its-pointless.github.io/files/24 termux extras
```

输入以下命令安装。

```
wget https://its-pointless.github.io/pointless.gpg
apt-get install gnupg
apt-key add pointless.gpg
apt-get update
```

通过以下命令检查版本号，选择两者均有的版本。

```
apt search gcc
apt search gfortran
```

以gcc-8为例，安装及检验命令如下。

```
apt install gcc-8
gfortran-8 -v
gcc-8 -v
```

#### OpenCL

需要安装完整Linux。运行以下命令以安装编译器。

```
apt install clang
apt install cmake
```

从以下链接下载编译所必须的头文件。

```
https://github.com/KhronosGroup/OpenCL-Headers
```

寻找手机中名称类似`libOpenCL.so`的文件，复制到主程序目录。主程序目录结构如下。

```
├── CL
│   ├── libOpenCL.so
│   └── [头文件]
├── CMakeLists.txt
├── [cl核文件，以HelloWorld.cl为例]
└── [主程序，以test.cpp为例]
```

CMakeLists.txt按照以下格式填写，通过cmake和make即可生成相应文件。

```
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8.11)
 
# 项目信息
project (Test)
 
set(CMAKE_INCLUDE_CURRENT_DIR ON)
 
include_directories ("${PROJECT_SOURCE_DIR}/CL") 
 
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
 
#首先申明动态库的位置
LINK_DIRECTORIES("${PROJECT_SOURCE_DIR}")
 
SET(HELLO_SRC libOpenCL.so )
 
# 指定生成目标
add_executable (testopencldemo ${DIR_SRCS})
 
#制定具体的动态库的名称 
target_link_libraries (testopencldemo ${HELLO_SRC})
```

#### Jupyter

需要安装完整Linux。通过以下命令安装。

```
pip3 install numpy
pip3 install pandas
apt install libzmq
pip3 install jupyter
apt install freetype libpng pkg-config
pip3 install matplotlib
```

完成安装后通过以下指令启动。

```
jupyter notebook
```

可通过配置文件，避免输入`http://localhost:8888/`后面的一串密匙。

运行以下命令以生成配置文件和密匙。

```
jupyter notebook --generate-config
ipython
from notebook.auth import passwd
passwd()
```

完成密码输入后复制所得内容，修改data/data/com.termux/files/home/.jupyter目录下的jupyter_notebook_config.py，如下。

```
c.NotebookApp.password = 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed' # 修改为前面所得的内容
c.NotebookApp.ip = '*' # 允许任何ip访问
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888 # 可自行指定端口，访问时使用
```

完成后保存，启动后即可直接通过以下网站访问。

```
http://localhost:8888/

// 电脑端利用SSH登录手机后可输入以下网站
[手机IP地址].8888
```

可运行以下命令以安装python扩展包，使编写python时有提示符。安装完成后在Jupyter notebook中出现Nbextensions，选择Hinterland即可。

```
apt install libxml2
apt install libxslt
pip3 install jupyter-contrib-nbextensions
jupyter contrib nbextension install --user
pip3 install jupyter_nbextensions_configurator
```

#### VS Code

需要安装完整Linux。

输入以下命令安装node、npm和cnpm。

```
apt install nodejs
apt install npm
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install -g npm
```

用cnpm安装wcode。

```
cnpm install -g wcode
```

使用时通过以下命令启动。

```
wcode -p 9999 /home
```

通过浏览器访问即可。

```
// 手机浏览器
localhost:9999

// 内网电脑
[手机IP地址]:9999
```

也可通过code-server。下载arm64版本的安装包，复制到手机并解压。

```
https://github.com/cdr/code-server/releases
```

解压完成后切换到该目录，并运行以下命令开启VS Code。通过浏览器访问即可，同上。

```
export PASSWORD=123456 
./code-server --auth password --port 9999
```


## Aid Learning

Termux的国内高仿版。无需安装Termux，直接下载即可使用。Aid Learning已自带VS Code。

### 环境配置

#### gfortran

Aid Learning已自带gcc/gfortran。若没有可通过以下命令安装。

```
apt install gcc
apt install gfortran
```

#### JAVA

以Java 8为例，通过以下命令安装。

```
apt install openjdk-8-jdk
apt install openjdk-8-jre
```

输入以下命令查看是否正确安装。

```
java -version
```

若出现错误，则需要修改环境变量。打开/etc/profile，添加以下内容后保存。

```
export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-arm64"
export CLASSPATH=.$JAVA_HOME/lib/
export JRE_HOME=$JAVA_HOME/jre
export PATH=$JAVA_HOME/bin:$PAT
```

#### Jupyter

Aid Learning已自带Jupyter。

##### 基本使用

可使用Jupyter Lab，通过以下命令安装和启动即可。

```
// 安装
pip3 install jupyterlab

// 启动
jupyter lab
```

也可使用Jupyter Notebook。直接运行以下命令即可。

```
jupyter notebook
```

##### Java内核

注意，Jupyter支持的JDK版本为9.0及以上，而Aid Learning内核支持的JDK为JDK8，因此在安装Java时需要留意版本。

可安装IJava支持套件。通过以下链接下载，然后复制到手机。

```
https://github.com/SpencerPark/IJava/
```

切换到文件所在位置并运行以下命令。

```
unzip ijava.zip -d ijava // 文件名为ijava.zip
cd ijava
python3 install.py --sys-prefix
```

安装完成后查看Jupyter内核，此时应当有Java。

```
jupyter kernelspec list
```

# 相关下载

## ADB工具包

可通过以下链接下载。

```
https://developer.android.com/studio/releases/platform-tools
```

对于Mac，可通过Homebrew安装。

```
brew cask install android-platform-tools
```


# 参考教程

## LG G3 LineageOS Tutorial

```
https://linustechtips.com/topic/1058206-lg-g3-lineageos-tutorial/
```

## 极致安卓之—Termux安装完整版Linux

```
https://zhuanlan.zhihu.com/p/95865982
```

## 把安卓手机性能发挥到极致之-Termux

```
https://zhuanlan.zhihu.com/p/92664273
```

## 把安卓手机性能发挥到极致之-Aid Learning运行Java及性能测试

```
https://zhuanlan.zhihu.com/p/92489740
```

## 把安卓手机性能发挥到极致之-Termux安装Python及Jupyter

```
https://zhuanlan.zhihu.com/p/94203587
```

## 安卓手机C/C++开发平台

```
https://zhuanlan.zhihu.com/p/97882309
```

## 把安卓手机性能发挥到极致之-Termux/Aid Learning使用Fortran

```
https://zhuanlan.zhihu.com/p/92280533
```

## 极致安卓之—Aid Learning基于Jupyter开发Java和Python

```
https://zhuanlan.zhihu.com/p/101147592
```

## 把安卓手机性能发挥到极致之-Termux运行Java及性能测试

```
https://zhuanlan.zhihu.com/p/92471681
```

## Termux安装Node.JS及网页版文档查看与修改器wcode

```
https://zhuanlan.zhihu.com/p/106298311
```

## 极致安卓—Termux/Aid Learning安装宇宙最强VS Code

```
https://zhuanlan.zhihu.com/p/106593146
```

## 一加 OnePlus 7 刷入氧 OS、TWRP、Magisk (Root)

```
https://blog.skk.moe/post/op7-oos-twrp-magisk/
```
