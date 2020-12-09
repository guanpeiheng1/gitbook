# 苹果系统版本

| 系统名称              | 系统版本 | 版本号 |
| --------------------- | -------- | ------ |
| macOS Catalina        | 10.15.5  | 19F96  |
| macOS Mojave          | 10.14.6  | 18G84  |
| macOS High Sierra     | 10.13.6  | 17G65  |
| macOS Sierra          | 10.12.6  | 16G29  |
| OS X El Capitan       | 10.11.6  | 15G31  |
| OS X Yosemite         | 10.10.5  | 14F27  |
| OS X Mavericks        | 10.9.5   | 13F34  |
| OS X Mountain Lion    | 10.8.5   | 12F37  |
| OS X Lion             | 10.7.5   | 11G56  |
| Mac OS X Snow Leopard | 10.6.8   | 10K540 |
| Mac OS X Leopard      | 10.5.8   | 9L30   |
| Mac OS X Tiger        | 10.4.11  | 8S165  |

# 虚拟机安装

安装`Vmware Workstation Player`后，打开`任务管理器`，切换到`服务`选项卡，停止所有与VM有关的服务。打开Vmware虚拟机苹果破解补丁`unlocker`，右键以管理员身份运行`win-install`，完成虚拟机对macOS系统的破解。

打开Vmware Workstation Player，创建新虚拟机，安装光盘选择Mac OS X的ISO镜像文件，系统选择`Mac OS X`，创建新虚拟机并完成虚拟机内Mac的安装，注意用户密码不能为空。安装完成后，在虚拟机菜单上点击`安装Vmware Tools for Mac`，即可使Mac满屏显示。

点击虚拟机选项-共享文件夹，勾选`总是启用`并设置好共享文件夹，即可完成Windows和Mac的文件互通。

# 硬盘安装

## 记录硬件配置

首先需要确定电脑设备的型号，为以后的引导和驱动安装做好准备。在Windows下，右键点击`此电脑`，选择`管理`，在侧边栏选择`设备管理器`，得到设备清单。在某设备上点击`属性`，切换到`详细信息`选项卡，在属性下拉栏选择`硬件ID`，记录下`VID`和`PID`。

对于笔记本，主要记录显卡、声卡、网卡、键盘、触控板和蓝牙的型号。以本机为例，本机型号为华硕FL8000U，硬件基本信息如下。

|          |               型号               | Device ID | Vendor ID |
| -------- | -------------------------------- | --------- | --------- |
|   CPU    | Intel Core i7-8550U（Kaby-Lake） |     /     |     /     |
| 核心显卡 |      Intel UHD Graphics 620      |   5917    |   8086    |
| 独立显卡 |       NVIDIA GeForce 940MX       |   134D    |   10DE    |
|   声卡   |         Realtek ALC 294          |   0294    |   10EC    |
| 有线网卡 |         Realtek RTL8111          |     /     |     /     |
| 无线网卡 |          Atheros AR9565          |   0036    |   168C    |
|   键盘   |             PS2键盘              |     /     |     /     |
|  触摸板  |            I2C触摸板             |     /     |     /     |
|   蓝牙   |      Atheros Bluetooth 4.0       |   3018    |   04CA    |

## 安装前的准备

由于macOS只能以EFI方式引导，因此需确保分区表为GPT格式。可在Windows下打开DiskGenius，确认分区表是否为GUID格式，若不是，则在硬盘上单击右键，选择`转换分区表类型为GUID格式`。此处不讨论BIOS引导GPT这一方法。

黑苹果的安装需要为macOS的安装划出空间。确保硬盘有一个200-300M的`EFI分区`，并提前备份EFI内的文件至U盘，防止安装Mac时引导丢失。为MacOS的安装划出50G以上的分区，格式化为`APFS`。

插入容量大于8G的U盘，提前备份好U盘资料。以管理员身份打开`TransMac`，在插入的U盘上点击右键，选择`Restore with Disk Image`，点击`Yes`以抹掉资料，选择dmg镜像，等待写入完成。此处选择macOS Majove 10.14.3的dmg镜像。

## 推荐BIOS设置

### 全局设置

有AHCI的均选`AHCI`，开启支持UEFI启动。

| 选项                                | 本机BIOS地址 | 设定值 | 设置           | 说明                                                        |
| ----------------------------------- | ------------ | ------ | -------------- | ----------------------------------------------------------- |
| Fast Boot                           | 0xF7A        | 0x0    | Disabled       | 快速启动                                                    |
| Secure Boot                         |              |        | Disabled       | 安全启动                                                    |
| CFG Lock                            | 0x527        | 0x0    | Disabled       | CFG锁（MSR 0xE2写入保护）                                   |
| Vt-d                                | 0x7EC        | 0x0    | Disabled       | 直接 I/O 访问的 VT虚拟化技术                                |
| CSM                                 | 0x1032       | 0x0    | Disabled       | 兼容性支持模块（如果看到乱码则启用）                        |
| Intel SGX                           | 0x5DC        | 0x0    | Disabled       | Intel架构新扩展                                             |
| IO Serial Port                      |              |        | Disabled       |                                                             |
| LAN/WLAN switching                  |              |        | Disabled       |                                                             |
| Extended Idle Power States          |              |        | Disabled       |                                                             |
| Wake on LAN/WLAN/WWAN               |              |        | Disabled       |                                                             |
| Wake on USB                         |              |        | Disabled       |                                                             |
| Firewire/IEEE 1394                  |              |        | Disabled       |                                                             |
| USB 3.0                             |              |        | Disabled       | 安装好系统后可以开启                                        |
| TMP/Security Chips/Security modules |              |        | Disabled       | 安全模块                                                    |
| VT-x                                | /            | /      | Enabled        | 英特尔虚拟化技术                                            |
| Above 4G decoding                   | 0x7ED        | 0x1    | Enabled        | 大于4G地址空间解码                                          |
| Hyper Threading                     | 0x4F0        | 0x1    | Enabled        | 处理器超线程                                                |
| Execute Disable Bit                 | 0x4E9        | 0x1    | Enabled        | 执行禁止位                                                  |
| EHCI Hand-off                       | 0x2          | 0x1    | Enabled        | 接手EHCI控制                                                |
| XHCI Hand-off                       | 0x1B         | 0x1    | Enabled        | 接手XHCI控制                                                |
| USB Hand-off                        |              |        | Enabled        | 接手USB控制（打开后则无需ReleaseUsbOwnership/Fixownership） |
| Launch Hotkeys without Fn keypress  |              |        | Enabled        | 对于Skylake和KabyLake平台                                   |
| USB Legacy support                  |              |        | Enable         |                                                             |
| OS type                             | /            | /      | Windows 8.1/10 | 操作系统类型Windows 8.1/10                                  |

### 显卡设置

若有Nvidia/AMD的显卡，则主显示器应设置为PEG或PCIE，并禁用与Intel显卡有关的选项（Hybrid Graphics/Dual Graphics/DVMT size等）。

若有Intel的显卡，则将DVMT设置为64MB或更大。注意，本文主要讨论Intel显卡的黑苹果安装方法。

## 黑苹果安装

BIOS中启动顺序把UEFI启动的U盘设为第一，在弹出的Clover界面选择`Boot OS X Install from Install macOS Majove`，稍等片刻进入安装界面。

在macOS实用工具中选择`磁盘工具`，点击左侧按钮选择`显示所有设备`，选择之前分好的Mac安装盘，点击`抹掉`，并选择格式为`APFS`。返回安装界面，选择刚才格式化好的盘进行安装，等待安装完成。

重启时需继续从U盘启动，Clover界面中选择macOS的安装盘，进入欢迎界面后注意用户密码不能为空且跳过iCloud设置。若正常进入黑苹果桌面，则切记不要登录iCloud/iMessage/FaceTime。

若在跑代码的过程中发生错误，则根据说明中的常见错误进行排除。

## 黑苹果引导

### Clover安装

打开Clover的安装包，同意协议后点击自定，勾选`安装在Clover到EFI分区`、`安装RC scripts到目标磁区`和`UEFI Drivers`。安装完成后打开EFI分区的EFI/CLOVER/kexts，只留Others文件夹，其余删除。打开Others文件夹，只留`VirtualSMC.kext`、`WhatEverGreen.kext`和`Lilu.kext`。然后打开EFI/CLOVER/DRIVER64UEFI，删除SMCHelper-64.efi并复制`VirtualSMC.efi`到此目录下。

### Clover配置

确保有Clover Configurator后，打开config.plist，按下列设置进行修改。

#### ACPI

将Fixs下所有选项取消勾选（1和2两个页面的选项都要取消），并添加以下更名补丁。

```
Comment: change _DSM to XDSM
Find: 5F44534D
Replace: 5844534D
```

#### Boot

Arguments下勾选Verbose（-v）、nv_disable=1、dart=0，其余不勾选。

#### Grpahics

取消勾选Inject Intel、Inject EDID。

#### Devices

在Audio的Inject下拉菜单选择No，把下方Arbitrary、Properties和Add Properties所有内容删掉。

### 进入系统

退出Clover Configurator，保存更改后拔出U盘并重启，在BIOS下应当多出一项UEFI OS，进入后选择macOS安装盘启动。此时已完成Clover引导器的安装，可以脱离U盘引导。重启多遍后，EFI根目录下出现nvram.plist，表明Clover已完成nvram的模拟。

若Windows引导丢失，将备份好的EFI/BOOT中的Microsoft文件夹复制到EFI分区中的EFI/BOOT，与Clover文件夹同级即可。

## 驱动修补

一般不建议更改系统SLE或LE。若需将驱动装入SLE或LE，可通过Kext Utility（在Catalina下需先解锁系统分区）或Hackintool，在Hackintool的`工具`选项卡下点击下方的小屋按钮即可。

### 显卡驱动

#### 选择需要驱动的显卡

在DSDT的PCI0设备目录下，Intel显卡拥有关键字`Name (_ADR, 0x00010000)`。Mac原生支持Intel显卡，但需要进行缓冲帧修补。

对于笔记本而言，如果独显不与核显绑定，即独显本身可以独立工作，则不需要屏蔽独显，可从BIOS层面关闭核显，并寻找独显驱动。

否则，由于macOS不支持双显卡切换技术，为使显卡正常，需要屏蔽独显。大多数笔记本上的双显卡切换是让集显承担图像输出任务，所以不能屏蔽集显，屏蔽会导致黑屏。

可以通过外接显示器判断独显是否独立。外接显示器后在桌面右键，打开NVIDIA控制面板，然后看是否可以调节数字振动这类设置。如果可以，说明此独显可以被独立驱动。

对于本机而言，由于BIOS中有External Gfx Card Primary Display Configuration，理论上可以屏蔽核显以驱动独显。考虑到BIOS中的设置会影响其他系统，此处采用屏蔽Nvidia独显而驱动Intel核显的方案。

对于旧版macOS的独显，可尝试通过purge-wrangler驱动，链接如下。

```
https://github.com/mayankk2308/purge-wrangler
```

#### 驱动Nvidia独显

Nvidia显卡一般免驱，部分显卡打开Inject Nvidia即可驱动成功。对于未受支持的显卡，则需要安装Nvidia WebDriver。此驱动最新版只适用于10.13，因此需要进行修补。

在终端输入以下命令以启动安装脚本。

```
curl -O https://raw.githubusercontent.com/Benjamin-Dobell/nvidia-update/master/nvidia-update.sh
chmod 755 nvidia-update.sh
./nvidia-update.sh -f
```

安装完成后重启，在系统设置中已经能够看到Nvidia控制面板。可安装WebDriver Manager获得更好的体验。若未驱动，则需要在Clover的System Parameters下勾选NvidiaWeb。

若为笔记本，独显通常不可驱动。若为台式机，在Catalina上独显无法开启硬件加速。若该显卡正常驱动，可安装CUDA Driver获得更好的功能。

#### 驱动Intel核显

确保kexts文件夹内有Lilu.kext和WhateverGreen.kext。打开`Hackintool`，在`缓冲帧`菜单项选择`macOS 10.14`，在`IntelGen`菜单项选择显卡的次代。本机为Intel HD Graphics 620，选择Kaby Lake。然后选择`PlatformID`，本机选择0×59160000。具体对应架构和PlatformID可在以下链接查询。

```
https://blog.daliansky.net/Intel-core-display-platformID-finishing.html
```

点击应用补丁选项卡，进行以下设置。

```
// 通用
选择设备属性，勾选自动侦测变化、全部、接口、基本显存、EDID、图形卡、音频、PCI设备

// Advenced
勾选将DP映射到HDMI、使用英特尔HDMI、修复热插拔重启、HDMI无限循环修复、仿冒声卡ID、显存2048MB、启用HDMI20（4K）、缓冲帧接口数限制：3
勾选仿冒图形卡ID，选择对应显卡设备
```

点击`Generate Patch`生成显卡驱动补丁。点击`File`菜单项，选择Export-Clover config.plist，路径选择EFI/CLOVER并确认替换。注意，Hackintool会将原来的config.plist和生成的补丁进行合成，替换以后的config.plist为原有基础上添加了显卡补丁的配置文件，而原有配置文件被备份为config-backup.plist。

#### 屏蔽独显

##### 利用添加引导标志（不推荐）

保证Whatevergreen存在，在Clover引导标志中添加`-wegnoegpu`。此方法不能保证始终正常工作。

##### 利用添加独显设备并禁用

该方法会禁用同一品牌的所有显卡，也会禁用开普勒GPU。在Clover的Device-Add Properties下添加以下内容即可。

对于Nvidia显卡采用以下内容。

| Device | Key        | Value            | Disabled | Value Type |
| ------ | ---------- | ---------------- | -------- | ---------- |
| NVidia | name       | 23646973706C6179 | False    | DATA       |
| NVidia | IOName     | \#display        | False    | STRING     |
| NVidia | class-code | FFFFFFFF         | False    | DATA       |

对于AMD显卡则采用以下内容。

| Device | Key        | Value            | Disabled | Value Type |
| ------ | ---------- | ---------------- | -------- | ---------- |
| ATI    | name       | 23646973706C6179 | False    | DATA       |
| ATI    | IOName     | \#display        | False    | STRING     |
| ATI    | class-code | FFFFFFFF         | False    | DATA       |
| ATI    | vendor-id  | FFFF0000         | False    | DATA       |
| ATI    | device-id  | FFFF0000         | False    | DATA       |

##### 利用SSDT（推荐）

首先需要查找独显的设备地址，可通过以下两种方式找到。

###### 查找设备地址

打开Windows下的设备管理器，在独显设备上右键单击`属性`，在详细信息标签卡下拉菜单选择`BIOS设备名称`即可。

也可通过ACPI表查找。在Clover引导界面按下`F4`，进入系统后打开EFI/CLOVER/ACPI/origin，可得到本机所有ACPI文件。复制到任一位置后打开终端，切换到复制到的位置并执行以下命令。

```
grep -l _OFF *.aml
grep -l _INI *.aml
```

检查输出并记录两个输出中均存在的文件。打开这些文件并查找\_OFF方法，如果这个方法在PowerShell宏中（即由PowerShell大括号括住），则舍弃。找到独立的\_OFF方法并记录其设备路径即可。

设备路径为类似`_SB.PCI0.PEG0.PEGP`的形式。

###### 应用SSDT

打开MaciASL并新建文件，将以下代码复制，并将`_SB.PCI0.PEG0.PEGP`修改为所找到的设备路径，保存为aml文件并放到EFI/CLOVER/ACPI/hotpatch即可。

对于Nvidia显卡采用以下SSDT。

```
DefinitionBlock ("", "SSDT", 2, "hack", "spoof", 0)
{
    Method(_SB.PCI0.PEG0.PEGP._DSM, 4)
    {
        If (!Arg2) { Return (Buffer() { 0x03 } ) }
        Return (Package()
        {
            "name", Buffer() { "#display" },
            "IOName", "#display",
            "class-code", Buffer() { 0xFF, 0xFF, 0xFF, 0xFF },
        })
    }
}
```

对于AMD显卡采用以下SSDT。

```
DefinitionBlock ("", "SSDT", 2, "hack", "spoof", 0)
{
    Method(_SB.PCI0.PEG0.PEGP._DSM, 4)
    {
        If (!Arg2) { Return (Buffer() { 0x03 } ) }
        Return (Package()
        {
            "name", Buffer() { "#display" },
            "IOName", "#display",
            "class-code", Buffer() { 0xFF, 0xFF, 0xFF, 0xFF },
            "vendor-id", Buffer() { 0xFF, 0xFF, 0,  0 },
            "device-id", Buffer() { 0xFF, 0xFF, 0, 0 },
        })
    }
}
```

##### 利用设备路径

下载`gfxutil`后打开终端，并切换到gfxutil所在路径，执行以下命令以输出独显设备路径。

```
gfxutil -f GFX0
```

将输出的路径复制，然后打开Clover，切换到Devices选项卡，点击Properties，在Devices栏新增一项，其路径为刚刚复制的路径，然后在右侧添加以下内容。

对于Nvidia显卡采用以下内容。

| Properties Key | Properties Value | Value Type |
| -------------- | ---------------- | ---------- |
| name           | 23646973706C6179 | DATA       |
| IOName         | \#display        | STRING     |
| class-code     | FFFFFFFF         | DATA       |

对于AMD显卡则采用以下内容。

| Properties Key | Properties Value | Value Type |
| -------------- | ---------------- | ---------- |
| name           | 23646973706C6179 | DATA       |
| IOName         | \#display        | STRING     |
| class-code     | FFFFFFFF         | DATA       |
| vendor-id      | FFFF0000         | DATA       |
| device-id      | FFFF0000         | DATA       |

注意，进入该设备路径的其它设备也将获得这些属性，从而失效。故可用此法禁用其它PCIe驱动器。

#### 检验效果

重启后显卡正常情况下已经驱动，Dock栏变为半透明。点击左上角苹果按钮，选择`关于本机`，若图形卡一栏显示2048MB而不是7MB，则核显驱动成功。在`系统报告`中选择`图形卡`，若不能看到所屏蔽的独显，则独显屏蔽成功。点击核显，若有`Metal:    支持，功能集macOS GPUFamily2 v1`字样，则metal支持打开成功。

显卡驱动后需检验显卡硬件加速（QE/CI）是否已经打开。如果能够运行象棋游戏、屏幕保护程序正常工作、Grapher能看到三维图像、截屏功能正常，则硬件加速已打开。

也可使用VDADecoderCheck进行检查（在Catalina工作似乎不正常），或者使用`VideoProc`检测。对于VideoProc，打开应用后进入`设置`，点击`硬件加速引擎`旁边的`选项`，查看检测结果即可。

### 有线网卡驱动

#### 具备有线网卡的设备

对于Realtek RTL8111有线网卡，网络上寻找对应驱动的kext，并放入Clover的kexts/Others即可驱动。驱动后在`系统偏好设置`中点击`网络`，可看到有线网卡。或点击左上角苹果按钮，选择`关于本机`-`系统报告…`，在`以太网卡`中可以看到有线网卡。

#### 不具备有线网卡的设备

因为Mac App Store、iCloud、FaceTime和iMessage的正常工作要求在`en0`处具有内置以太网，因此，若无有线网卡，需将`NullEthernet.kext`放到Clover的kexts中以模拟出一个有线网卡设备。

#### 重置网络端口

若以太网端口不在en0，则需进入`网络偏好设置`，删除所有接口并应用，然后删除`/Library/Preferences/SystemConfiguration/NetworkInterfaces.plist`，重新启动后进入网络偏好设置，重新添加网络端口。

#### 内建有线网卡

在Clover的Devices选项卡中选择Properties，查找前面通过Hackintool找到的有线网卡设备，并添加条目built-in，数值为01，类型为DATA，重启即可。

如果用的是`NullEthernet.kext`，则把SSDT-RMNE.aml放入Clover的ACPI/patched中即可。

### 声卡驱动

#### 使用AppleALC（推荐）

保证系统SLE下有`AppleHDA.kext`，在Clover的kexts文件夹中放入`AppleALC.kext`。打开Hackintool，切换到Audio，通过下拉列表设置Layout ID。切换到`Intel`-`Patch`，选择Devices/Properties，勾选`Auto Detect Changes`、`Audio`，点击`Generate Patch`生成声卡驱动补丁。完成后保存以替换。

也可手动生成设备信息。打开Clover的config.plist，在Devices选项卡下选择Properties，先在左侧栏添加声卡的设备路径（可在Hackintool中查找），然后右侧栏添加以下条目即可。

| Properties Key | Properties Value | Value Type |
| -------------- | ---------------- | ---------- |
| alc-layout-id  | 21               | NUMBER     |
| hda-gfx        | onboard-1        | STRING     |
| AAPL,slot-name | Internal         | STRING     |

若为Windows/Linux与macOS双引导，则打开Clover的config.plist，在Devices选项卡勾选Audio项下的`ResetHDA`选项。

重启后检查声卡是否被驱动，若未被驱动则重复上述过程，更换不同的Layout ID。本机使用的Layout ID为12。

#### 使用VoodooHDA

下载kext并放到Clover中即可。  

### 蓝牙驱动

本机蓝牙为与无线网卡Atheros AR9565共建的Bluetooth 4.0。打开以下链接并下载，对于Clover而言直接放到Kexts文件夹即可。对于OpenCore，放置到Kexts文件夹后需要在config.plist中添加对应条目，注意Ath3kBTInjector必须加载在Ath3kBT后面。

```
https://github.com/zxystd/AthBluetoothFirmware/releases
```

#### 启用蓝牙的关闭按钮（过时）

找到SLE下的`IOBluetoothFamily.kext`，复制到桌面后右键单击显示包内容，进入Contents/PlugIns，在`BroadcomBluetoothHostControllerUSBTransport.kext`上右键单击显示包内容，打开Contents/info.plist。用搜索功能搜索`2652`，修改`idProduct`和`idVendor`为自己的ID。注意需进行进制转换，从Windows上得到的是十六进制，需转换为十进制，可利用下面的进制转换工具。

```
http://tool.oschina.net/hexconvert/
```

将修改好的IOBluetoothFamily.kext用Kext Utility进行安装，重启即可。注意，此法可以实现蓝牙关闭，但需要从Windows重启进入Mac后才可用。

#### 冷启动下无法驱动蓝牙的替代方案

##### 通过Ubuntu虚拟机

从Clover的kexts文件夹删除`BrcmPatchRAM.kext`和`BTFirmwareUploader.kext`。下载Ubuntu镜像，安装并打开Vmware Fusion，新建虚拟机并选择Ubuntu系统，CD/DVD驱动器选择刚才的镜像，完成配置。

关闭虚拟机，切换到虚拟机设置页。选择`USB和蓝牙`首选项，取消选中`与Linux共享蓝牙设备`，列表中将出现一个蓝牙设备。返回主界面并点击`启动磁盘`，选择CD/DVD为启动项，然后退出设置并启动虚拟机。

镜像引导成功后，选择`试用Ubuntu`。待开机完成后，点击虚拟机的蓝牙选项以将蓝牙设备连接至Ubuntu。等待4-5秒钟，然后取消蓝牙连接，固件已由虚拟机上传完成，蓝牙将正常运行。关闭VM，VMware将保存计算机状态，只要不让机器进入睡眠或重启状态，就可以使用蓝牙。

如果在睡眠/重新启动后失去了蓝牙功能，重新打开虚拟机，连接再断开蓝牙即可。

##### 通过TinyCore虚拟机

使用以下虚拟机即可，使用方法同Ubuntu。

```
https://drive.google.com/file/d/1Ydm7Hd0d5XOKQkb02EMpm-6E-WQFiMkh/view
```

### USB修补

#### USB定制

首先需要在Clover下放置`USBInjectAll.kext`，且确保ACPI更名中已应用EHC1->EH01、EHC2->EH02以及XHCI/XHC1 -> XHC_补丁（若DSDT中无对应变量则可不使用），然后按照以下方法之一操作。

##### 通过Hackintool

打开Hackintool，点击USB，确保芯片组已被正确识别。把所有端口删除后刷新，然后分别取USB3与USB2的可移动设备对每一个接口进行插拔，每插拔一次设备将出现一个条目的改变，记录对应端口名称及USB类型，如下表所示。注意，TypeC接口需要正反各插拔一次。

| 设备类型                               | 端口类型 |
| -------------------------------------- | -------- |
| 永久连接设备的USB端口（如蓝牙）        | Internal |
| 与USB3端口相连的HSxx端口（USB2）       | USB3     |
| 内部集线器（通常连接到端口PR11和PR21） | Internal |
| TypeC正反插端口一致                    | TypeC+Sw |
| TypeC正反插端口不一致                  | TypeC    |

完成后导出，将SSDT-EC.aml、SSDT-UIAC.aml和SSDT-USBX.aml放到Clover的ACPI/patched下。或将SSDT-EC.aml和USBPorts.kext分别放置到ACPI/patched和kexts/Other下。若放置的是SSDT，则需保留USBInjectAll.kext，若为SSDT+kext则需删除。

重启后若需检测是否成功，可插入USB3.0设备，通过系统报告查看速率是否为5G/s。或使用IOregistryExplorer，搜索XHC，可查看所有端口是否都已正常工作。

##### 通过USBMap脚本

打开终端并运行以下命令以打开脚本。

```
git clone https://github.com/corpnewt/USBMap
cd USBMap
chmod +x USBMap.command
```

脚本打开后会核查USBInjectAll是否被正确加载，USB控制器是否被正确识别。脚本需要USBInjectAll的版本在0.70以上。若已完成放置但无法识别，则可能还需要XHCI_unsupported.kext。

若端口数量大于15个，则需要应用端口解除补丁或在脚本主菜单上选择`S. Exclude SSxx Ports`或`H. Exclude HSxx Ports`端口以禁用SSxx（一般为USB3.0）或HSxx（一般为USB2.0）端口，完成后重启。若需要取消禁用，则选择`C. Clear Exclusions`。

选择`D. Discover Ports`，脚本将读取默认USB列表。依次将每个端口用USB设备插拔，过程与使用Hackintool相同。每发现新的端口会有提示，直接忽略即可。完成后按`Q`并回车，返回主菜单。

选择`P. Edit Plist & Create SSDT/Kext`，可以看到刚才所识别的结果。若需要将编号为5和7的端口改为USB2.0接口，则输入以下命令并回车。

```
// T表示现在要更改端口的类型（Type），0表示端口类型
T:5,7:0
```

数字代号与端口类型对应关系如下，设置要求与使用Hackintool一致。

| 数字   | 含义                                                   |
| ------ | ------------------------------------------------------ |
| 0      | （USB2.0）Type A connector                             |
| 1      | Mini-AB connector                                      |
| 2      | ExpressCard                                            |
| 3      | （USB3.0）USB 3 Standard-A connector                   |
| 4      | USB 3 Standard-B connector                             |
| 5      | USB 3 Micro-B connector                                |
| 6      | USB 3 Micro-AB connector                               |
| 7      | USB 3 Power-B connector                                |
| 8      | Type C connector - USB2-only                           |
| 9      | （TypeC+Sw）Type C connector - USB2 and SS with Switch |
| 10     | Type C connector - USB2 and SS without Switch          |
| 11-254 | Reserved                                               |
| 255    | （Internal）Proprietary connector                      |

也可直接用plist编辑器打开脚本编辑（一般在用户目录/USBMap/Scripts中），并删除无用端口。

设置完成后可选择`K. Build USBMap.kext`或`S. Build SSDT-UIAC`，其中第一种需把生成的kext复制到Clover的kexts目录下且删除USBInjectAll，而第二种需把生成的SSDT复制到Clover的ACPI/patched下且保留USBInjectAll。推荐第二种。

注意，在按下创建SSDT之前，可以调节SSDT的形式，默认为三个SSDT，可设置为一个SSDT。生成的所有文件都在USBMap/Results中。

##### 修复唤醒断线问题

在Clover的kexts文件夹放入`USBPower.kext`。在kext上右键选择显示包内容，打开Contents/Info.plist，展开`IOKitPersonalities_x86_64`，将`iMac19,1`改成正在使用的机型，保存并重启即可。

#### USB端口限制解除补丁

macOS规定USB端口的数量应少于15个。若端口数量大于15个，则需用以下补丁解除。

```
// Catalina补丁为以下两条
Comment: USB port limit patch #1 10.15.x modify by DalianSky(credit ydeng)
Name: com.apple.iokit.IOUSBHostFamily
Find: 83FB0F0F
Replace: 83FB3F0F

Comment: USB Port limit patch #2 10.15.x modify by DalianSky
Name: com.apple.driver.usb.AppleUSBXHCI
Find: 83F90F0F
Replace: 83F93F0F

// 其余系统版本补丁如下
<dict>
    <key>Comment</key>
    <string>Port limit increase</string>
    <key>Disabled</key>
    <false/>
    <key>Find</key>
    <data>
    g710////EA==
    </data>
    <key>InfoPlistPatch</key>
    <false/>
    <key>MatchOS</key>
    <string>10.12.x</string>
    <key>Name</key>
    <string>com.apple.driver.usb.AppleUSBXHCI</string>
    <key>Replace</key>
    <data>
    g710////Gw==
    </data>
</dict>

<dict>
    <key>Comment</key>
    <string>Port limit increase (RehabMan)</string>
    <key>Disabled</key>
    <false/>
    <key>Find</key>
    <data>
    g32IDw+DpwQAAA==
    </data>
    <key>InfoPlistPatch</key>
    <false/>
    <key>MatchOS</key>
    <string>10.13.x</string>
    <key>Name</key>
    <string>com.apple.driver.usb.AppleUSBXHCI</string>
    <key>Replace</key>
    <data>
    g32ID5CQkJCQkA==
    </data>
</dict>

<dict>
    <key>Comment</key>
    <string>Port limit increase (Ricky)</string>
    <key>Disabled</key>
    <false/>
    <key>Find</key>
    <data>
    g/sPD4OPBAAA
    </data>
    <key>InfoPlistPatch</key>
    <false/>
    <key>MatchOS</key>
    <string>10.14.x</string>
    <key>Name</key>
    <string>com.apple.driver.usb.AppleUSBXHCI</string>
    <key>Replace</key>
    <data>
    g/sPkJCQkJCQ
    </data>
</dict>
```

### 屏幕亮度调节

将`SSDT-PNLF.aml`和`SSDT-ALS0.aml`放入Clover的ACPI/patched下，打开config.plist，在Acpi选项卡下点击List Of Patches，选择`change GFX0 to IGPU`，在列表中出现此项后，关闭并保存。重启电脑后进入`系统偏好设置`-`显示器`，若出现亮度调节滑块则已成功。

### 传感器驱动

在Clover的kexts下放入VirtualSMC包内的`SMCLightSensor.kext`、`SMCProcessor.kext`和`SMCSuperIO.kext`即可。

### 无线网卡驱动

无线网卡驱动需要自行寻找适合自己无线网卡的驱动，对于无解的网卡，可以更换网卡、购买黑苹果USB网卡或者将手机作为热点。

Intel Wi-Fi卡可尝试以下内容。

```
https://wikikeep.com/how-to-fix-intel-wifi-and-bluetooth-on-macos-big-sur/
```

USB网卡可尝试以下内容。

```
https://github.com/chris1111/WirelessAdapterCloverBigSur
https://github.com/chris1111/Wireless-USB-OC-Big-Sur-Adapter
```

#### 正常驱动方法

本机无线网卡为Athreos AR9565，可用驱动由`corecapture.kext`、`CoreCaptureResponder.kext`和`IO80211Family.kext`组成。把它们放入Clover的kexts中，重启电脑后无线网卡可用。

#### 替代方法

对于iPhone用户，将手机接入电脑，手机开启个人热点功能，即可使用手机热点。

对于Android用户，可安装`hoRNDIS`，然后将手机连接到电脑，开启安卓手机的USB网络共享即可。

### 键盘驱动

在Clover的kexts放入`VoodooPS2Controller.kext`，重启即可驱动。

### 触控板驱动

以下的所有操作对软件版本均有要求。MaciASL需为RehabMan编译版，IORegistryExplorer需为2.1版本。

在Windows下打开设备管理器，若在`人体学输入设备`存在`I2C HID设备`，则一般为I2C HID触摸板。查看触摸板设备的属性，若显示位置是`在 I2C HID设备 上`，则得证。否则触控板为PS2类型。

#### PS2触控板

用VoodooPS2Controller.kext驱动即可。

#### I2C触控板

若为I2C触控板，则在`I2C HID设备`上右键选择属性，点击详细信息，选择`BIOS设备名称`并记录。然后切换到`资源`选项卡，记录IRQ后面的数字。

##### VoodooI2C的工作原理

I2C触控板均用VoodooI2C驱动，支持轮询模式或中断模式。

轮询模式是Voodoo的安全模式，每隔一个特定的时间扫描一次触摸板的状态，然后改变鼠标指针坐标。而中断模式是触摸板在Windows/Linux等其他系统的正常工作方式。

在DSDT的修改中定义如下。

```
// GPIO中断
DSDT在SBFG中存在有效GPIO Pin，CRS方法中返回(SBFB,SBFG)，并应用启用GPIO控制器的补丁。

// APIC中断
IOInterruptSpecifiers值没有或者小于2F（16进制）。此时无需对 DSDT进行大量修改，只用应用Windows(20XX)补丁即可，使用的是APIC控制器而不是GPIO控制器。

// 轮询
DSDT中除了Windows(20XX)补丁以外，不应用其他补丁，CRS方法中只返回SBFB或者(SBFB,SBFI)。
```

默认情况分析如下图。

![](https://tva1.sinaimg.cn/large/006tNbRwly1g9j9syjzgdj30jg0m8jst.jpg)

##### 用轮询模式驱动

###### 放置驱动

下载VoodooI2C，按照以下规则将驱动放入Clover的kexts中（一个核心驱动+一个目标驱动）并重启。

```
// 必放核心驱动
VoodooI2C.kext

// 根据情况选择目标驱动
VoodooI2CELAN.kext - ELAN触摸板
VoodooI2CHID.kext - I2C触摸板
VoodooI2CFTE.kext - FTE触摸板
VoodooI2CSynaptics.kext - Synaptics触摸板
VoodooI2CAtmelMXT.kext - MXT触摸屏
VoodooI2CUPDDEngine.kext - UPDD多点触控引擎
```

对于小部分机型（如华硕笔记本），触控板已被驱动。若未被驱动，则需要对DSDT应用`Windows补丁`，根据出厂时随附的操作系统应用对应的补丁，一般用Windows 10 Patch。

应用补丁可采取热补丁法或DSDT修改法。

###### 热补丁法应用补丁

在`SSDT-XOSI.aml`并放置在Clover文件夹的ACPI/patched目录下，然后在config.plist下添加以下改名补丁即可。

```
Comment: change _OSI to XOSI
Find: 5F4F5349
Replace: 584F5349
```

###### DSDT修改法应用补丁

打开MaciASL，保证有VoodooI2C的补丁源。点击Patch，选择VoodooI2C下的`Windows 10 Patch`补丁，Apply-Close后保存DSDT，并将新的DSDT放入Clover文件夹的ACPI/patched下即可。

若MaciASL无法加载出补丁源，可直接在Patch界面复制以下代码内容。根据操作系统自行选择。

```
// Windows 10 DSDT Patch for VoodooI2C
into_all method code_regex If\s+\([\\]?_OSI\s+\(\"Windows\s2015\"\)\) replace_matched begin If(LOr(_OSI("Darwin"),_OSI("Windows 2015"))) end;

// Windows 8.1 DSDT Patch for VoodooI2C
into_all method code_regex If\s+\([\\]?_OSI\s+\(\"Windows\s2013\"\)\) replace_matched begin If(LOr(_OSI("Darwin"),_OSI("Windows 2013"))) end;

// Windows 8 DSDT Patch for VoodooI2C
into_all method code_regex If\s+\([\\]?_OSI\s+\(\"Windows\s2012\"\)\) replace_matched begin If(LOr(_OSI("Darwin"),_OSI("Windows 2012"))) end;

// Windows 7 DSDT Patch for VoodooI2C
into_all method code_regex If\s+\([\\]?_OSI\s+\(\"Windows\s2009\"\)\) replace_matched begin If(LOr(_OSI("Darwin"),_OSI("Windows 2009"))) end;
```

保证对DSDT不进行其他修改后，触控板在重启后应当已被驱动。

###### 对Samsung笔记本的特殊处理

对于Samsung笔记本电脑，需在应用补丁的同时，在DSDT的以下代码中删除 `If (_OSI ("Windows 2012")) {`及对应的`}`。

```
If (_OSI ("Windows 2012"))
{
    Scope (_SB.PCI0.I2C0.SPTP)
    {
    }

    Scope (_GPE)
    {
        Method (_L04, 0, NotSerialized)  // _Lxx: Level-Triggered GPE
        {
            Notify (\_SB.PCI0.I2C0.SPTP, 0x02)
            Notify (\_SB.PWRB, 0x02)
        }
    }

    Scope (_SB.PCI0.I2C0.ATFU)
    {
        Method (_CRS, 0, NotSerialized)  // _CRS: Current Resource Settings
        {
            Name (SBFB, ResourceTemplate ()
            {
                I2cSerialBusV2 (0x0026, ControllerInitiated, 0x00061A80,
                    AddressingMode7Bit, "\\_SB.PCI0.I2C0",
                    0x00, ResourceConsumer, , Exclusive,
                    )
                I2cSerialBusV2 (0x0027, ControllerInitiated, 0x00061A80,
                    AddressingMode7Bit, "\\_SB.PCI0.I2C0",
                    0x00, ResourceConsumer, , Exclusive,
                    )
            })
            Return (SBFB)
        }
    }
}
```

若仍未解决，则在DSDT中进入设备SPTP的_INI方法，搜索以下代码并将其内容更改为`SHPO (Local0, One)`。

```
If (LEqual (SDM0, Zero))
{
	SHPO (Local0, One)
}
```

##### 用中断模式驱动

###### 查看APIC Pin值

不放置VoodooI2C驱动，并应用Windows补丁。打开IORegistryExplorer，搜索前面保存的BIOS设备名称，然后定位到`IOInterruptSpecifiers`，最前面的两位数即为`APIC Pin`值。

若无IOInterruptSpecifiers项或APIC Pin值不大于`2F`，则直接跳到安装步骤，否则继续下列步骤。

###### 移除APIC中断控制器

用MaciASL打开设备的DSDT，搜索`BIOS设备名称`（此处为TPAD），定位到触控板设备的代码块下。注意，以下操作全部在触控板设备的代码块下进行，请勿跳到其他设备下的代码操作。

在触控板设备的代码块下寻找类似以下的代码。

```
Method (_CRS, 0, Serialized)  // _CRS: Current Resource Settings
{
    Name (SBFI, ResourceTemplate ()
    {
        I2cSerialBusV2 (0x0015, ControllerInitiated, 0x00061A80,
            AddressingMode7Bit, "\\_SB.PCI0.I2C1",
            0x00, ResourceConsumer, , Exclusive,
            )
        Interrupt (ResourceConsumer, Level, ActiveLow, Exclusive, ,, )
        {
            0x0000006D,
        }
    })
    Return (SBFI)
}
```

在此段代码中，需将`SBFI`改为`SBFB`，并删除以下内容。

```
Interrupt (ResourceConsumer, Level, ActiveLow, Exclusive, ,, )
{
	0x0000006D,
}
```

以上面代码为例，修改后变为如下。

```
Method (_CRS, 0, Serialized)  // _CRS: Current Resource Settings
{
    Name (SBFB, ResourceTemplate ()
    {
    	I2cSerialBusV2 (0x0015, ControllerInitiated, 0x00061A80,
			AddressingMode7Bit, "\\_SB.PCI0.I2C1",
			0x00, ResourceConsumer, , Exclusive,
			)
		})
		Return (SBFB)
}
```

###### 确定触控板固定类型

在触控板的DSDT下寻找以下代码块。

```
Name (SBFG, ResourceTemplate ()
{
    GpioInt (Level, ActiveLow, ExclusiveAndWake, PullDefault, 0x0000,
        "\\_SB.PCI0.GPI0", 0x00, ResourceConsumer, ,
        )
        {   // Pin list
            0x0000
        }
})
```

若此段代码出现在设备的根目录，则称为触控板设备已经`root固定`。若出现在CRS方法中，则称为触控板设备已经`CRS固定`。如果Pin list对应非零值，称为`well-root固定`和`well-CRS固定`。若此段代码未出现，则称为触控板设备`未固定`。

若未固定，则先将以上代码段复制到设备根目录下，示例如下。复制完成后跳到GPIO固定步骤。

```
Scope (_SB.PCI0.I2C0)
{
       Device (TPAD)
       {
           Name (SBFG, ResourceTemplate ()
           {
                GpioInt (Level, ActiveLow, Exclusive, PullUp, 0x0000,
                        "\\_SB.PCI0.GPI0", 0x00, ResourceConsumer, ,
                        )
                        {   // Pin list
                            0x0000
                        }
           })
           ......
```

若为root固定或CRS固定，需在触控板代码块下查找`_CRS`方法，并确定以下代码是否存在。若存在，则证明设备已固定好，可直接跳到安装步骤，否则需继续GPIO固定步骤。

```
Return (ConcatenateResTemplate (SBFB, SBFG))
```

若为well-root固定或well-CRS固定，则直接跳到安装步骤。

###### GPIO固定

打开Hackintool，在`PCI`选项卡下可以查看触控板类型，包括`SunrisePoint`、`CannonPoint(Lake)[Coffee Lake(-R)]`和`CannonPoint(Lake)[Whiskylake]`。

对于SunrisePoint，打开下面第一个链接，查找设备的十六进制APIC Pin值，并记录左侧标签值（只需记录`GPP_XYY_IRQ`中的XYY部分），若有多个标签值，需全部记录。再打开第二个链接，搜索上面所记录的XYY，记下右侧的数值（十进制），此即为触控板的硬件引脚号。对于此平台，硬件引脚号即为GPIO引脚号。

```
// SunrisePoint[Kaby Lake(-R)]
https://github.com/coreboot/coreboot/blob/master/src/soc/intel/skylake/include/soc/gpio_defs.h#L43
https://github.com/coreboot/coreboot/blob/master/src/soc/intel/skylake/include/soc/gpio_soc_defs.h#L37

// CannonPoint(Lake)[Coffee Lake-R]
https://github.com/coreboot/coreboot/blob/master/src/soc/intel/cannonlake/include/soc/gpio_defs_cnp_h.h#L42
https://github.com/coreboot/coreboot/blob/master/src/soc/intel/cannonlake/include/soc/gpio_soc_defs_cnp_h.h#L40

// CannonPoint(Lake)[Whiskylake]
https://github.com/coreboot/coreboot/blob/master/src/soc/intel/cannonlake/include/soc/gpio_defs.h#L42
https://github.com/coreboot/coreboot/blob/master/src/soc/intel/cannonlake/include/soc/gpio_soc_defs.h#L45
```

对于CannonPoint(Lake)[Coffee Lake(-R)]或CannonPoint(Lake)[Whiskylake]，前面步骤与上面相同，但得到的硬件引脚号并不是GPIO引脚号，因此还需要进行转换。打开下面转换公式链接，搜索前面记录的GPP_X，得到格式如`CHIPSET_GPP(数字, 基数, 终止, GPIO基数)`的内容。用所记下的十进制硬件引脚号减去`基数`，再加上`GPIO基数`，就是最后的十进制的GPIO引脚号。注意，`X_NO_GPIO`表示此引脚号无效。

```
// CannonPoint(Lake)[Coffee Lake-R]
https://github.com/coolstar/VoodooGPIO/blob/master/VoodooGPIO/CannonLake-LP/VoodooGPIOCannonLakeLP.hpp#L366

// CannonPoint(Lake)[Whiskylake]
https://github.com/coolstar/VoodooGPIO/blob/master/VoodooGPIO/CannonLake-H/VoodooGPIOCannonLakeH.hpp#L414
```

得到十进制的GPIO引脚后，需将数字转换为`十六进制`，然后替换前面SBFG代码中的Pin list值。

###### 确保DSDT通知系统设备已固定好GPIO

定位到代码块的_CRS方法，删除此方法的Return语句并用以下Return语句替换。

```
Return (ConcatenateResTemplate (SBFB, SBFG))
```

###### 驱动安装

驱动的安装包括驱动放置、禁用系统驱动、应用补丁，其中放置驱动操作与轮询模式完全一致。对于系统驱动，需禁用AppleIntelLpssI2C.kext和AppleIntelLpssI2CController.kext，方法见说明。应用补丁需应用Windows补丁和GPIO补丁。

Windows补丁的应用与轮询模式完全一致，GPIO补丁的应用提供以下两种形式。

###### 热补丁法应用GPIO补丁

在OC-little包中取`SSDT-GPIO.aml`并放置在Clover文件夹的ACPI/patched目录下，然后在config.plist下添加以下改名补丁即可。注意，在补丁无效时，可尝试删除`Tgtbridge`值。

```
Comment: change _STA to XSTA in GPI0 
Find: 5F535441
Replace: 58535441
TgtBridge: 47504930
```

###### DSDT修改法应用GPIO补丁

在MaciASL下点击Patch，选择VoodooI2C下的`GPIO Controller`和`SKL I2C Controller`补丁，Apply-Close后保存DSDT，并将新的DSDT放入Clover文件夹的ACPI/patched下即可。

若MaciASL无法加载出补丁源，可直接在Patch界面复制以下代码内容。

```
// GPI0 Controller
into method label _STA parent_label GPI0 replace_content begin
Return (0x0F)
end;


// Skylake controller patches for VoodooI2C
into_all device label I2C0 remove_entry;
into_all device label I2C1 remove_entry;
into_all device label I2C2 remove_entry;
into_all device label I2C3 remove_entry;
into_all device label I2C4 remove_entry;
into_all device label I2C5 remove_entry;

into scope label _SB.PCI0 insert begin

Device (I2C0)\n
{\n
    Name (LINK, "\\_SB.PCI0.I2C0")\n
    Method (_PSC, 0, NotSerialized)  // _PSC: Power State Current\n
    {\n
        Return (GETD (SB10))\n
    }\n
\n
    Method (_PS0, 0, NotSerialized)  // _PS0: Power State 0\n
    {\n
        LPD0 (SB10)\n
    }\n
\n
    Method (_PS3, 0, NotSerialized)  // _PS3: Power State 3\n
    {\n
        LPD3 (SB10)\n
    }\n
}\n
\n\n
Device (I2C1)\n
{\n
    Name (LINK, "\\_SB.PCI0.I2C1")\n
    Method (_PSC, 0, NotSerialized)  // _PSC: Power State Current\n
    {\n
        Return (GETD (SB11))\n
    }\n
\n
    Method (_PS0, 0, NotSerialized)  // _PS0: Power State 0\n
    {\n
        LPD0 (SB11)\n
    }\n
\n
    Method (_PS3, 0, NotSerialized)  // _PS3: Power State 3\n
    {\n
        LPD3 (SB11)\n
    }\n
}\n
\n\n
Device (I2C2)\n
{\n
    Name (LINK, "\\_SB.PCI0.I2C2")\n
    Method (_PSC, 0, NotSerialized)  // _PSC: Power State Current\n
    {\n
        Return (GETD (SB12))\n
    }\n
\n
    Method (_PS0, 0, NotSerialized)  // _PS0: Power State 0\n
    {\n
        LPD0 (SB12)\n
    }\n
\n
    Method (_PS3, 0, NotSerialized)  // _PS3: Power State 3\n
    {\n
        LPD3 (SB12)\n
    }\n
}\n
\n\n
Device (I2C3)\n
{\n
    Name (LINK, "\\_SB.PCI0.I2C3")\n
    Method (_PSC, 0, NotSerialized)  // _PSC: Power State Current\n
    {\n
        Return (GETD (SB13))\n
    }\n
\n
    Method (_PS0, 0, NotSerialized)  // _PS0: Power State 0\n
    {\n
        LPD0 (SB13)\n
    }\n
\n
    Method (_PS3, 0, NotSerialized)  // _PS3: Power State 3\n
    {\n
        LPD3 (SB13)\n
    }\n
}\n
\n
Device (I2C4)\n
{\n
    Name (LINK, "\\_SB.PCI0.I2C4")\n
    Method (_PSC, 0, NotSerialized)  // _PSC: Power State Current\n
    {\n
        Return (GETD (SB14))\n
    }\n
\n
    Method (_PS0, 0, NotSerialized)  // _PS0: Power State 0\n
    {\n
        LPD0 (SB14)\n
    }\n
\n
    Method (_PS3, 0, NotSerialized)  // _PS3: Power State 3\n
    {\n
        LPD3 (SB14)\n
    }\n
}\n
\n
Device (I2C5)\n
{\n
    Name (LINK, "\\_SB.PCI0.I2C5")\n
    Method (_PSC, 0, NotSerialized)  // _PSC: Power State Current\n
    {\n
        Return (GETD (SB15))\n
    }\n
\n
    Method (_PS0, 0, NotSerialized)  // _PS0: Power State 0\n
    {\n
        LPD0 (SB15)\n
    }\n
\n
    Method (_PS3, 0, NotSerialized)  // _PS3: Power State 3\n
    {\n
        LPD3 (SB15)\n
    }\n
}\n
\n
end;

into scope label _SB.PCI0.I2C0 replace_content begin

    Name (_HID, "INT3442")  // _HID: Hardware ID\n
    Method (_HRV, 0, NotSerialized)  // _HRV: Hardware Revision\n
    {\n
        Return (LHRV (SB10))\n
    }\n
\n
    Method (_CRS, 0, NotSerialized)  // _CRS: Current Resource Settings\n
    {\n
        Return (LCRS (SMD0, SB00, SIR0))\n
    }\n
\n
    Method (_STA, 0, NotSerialized)  // _STA: Status\n
    {\n
        Return (LSTA (SMD0))\n
    }\n
}\n
end;

into scope label _SB.PCI0.I2C1 replace_content begin

    Name (_HID, "INT3443")  // _HID: Hardware ID\n
    Method (_HRV, 0, NotSerialized)  // _HRV: Hardware Revision\n
    {\n
        Return (LHRV (SB11))\n
    }\n
\n
    Method (_CRS, 0, NotSerialized)  // _CRS: Current Resource Settings\n
    {\n
        Return (LCRS (SMD1, SB01, SIR1))\n
    }\n
\n
    Method (_STA, 0, NotSerialized)  // _STA: Status\n
    {\n
        Return (LSTA (SMD1))\n
    }\n
}\n
end;

into scope label _SB.PCI0.I2C2 replace_content begin

    Name (_HID, "INT3444")  // _HID: Hardware ID\n
    Method (_HRV, 0, NotSerialized)  // _HRV: Hardware Revision\n
    {\n
        Return (LHRV (SB12))\n
    }\n
\n
    Method (_CRS, 0, NotSerialized)  // _CRS: Current Resource Settings\n
    {\n
        Return (LCRS (SMD2, SB02, SIR2))\n
    }\n
\n
    Method (_STA, 0, NotSerialized)  // _STA: Status\n
    {\n
        Return (LSTA (SMD2))\n
    }\n
}\n
end;
into scope label _SB.PCI0.I2C3 replace_content begin

    Name (_HID, "INT3445")  // _HID: Hardware ID\n
    Method (_HRV, 0, NotSerialized)  // _HRV: Hardware Revision\n
    {\n
        Return (LHRV (SB13))\n
    }\n
\n
    Method (_CRS, 0, NotSerialized)  // _CRS: Current Resource Settings\n
    {\n
        Return (LCRS (SMD3, SB03, SIR3))\n
    }\n
\n
    Method (_STA, 0, NotSerialized)  // _STA: Status\n
    {\n
        Return (LSTA (SMD3))\n
    }\n
}\n
end;

into scope label _SB.PCI0.I2C4 replace_content begin

    Name (_HID, "INT3446")  // _HID: Hardware ID\n
    Method (_HRV, 0, NotSerialized)  // _HRV: Hardware Revision\n
    {\n
        Return (LHRV (SB14))\n
    }\n
\n
    Method (_CRS, 0, NotSerialized)  // _CRS: Current Resource Settings\n
    {\n
        Return (LCRS (SMD4, SB04, SIR4))\n
    }\n
\n
    Method (_STA, 0, NotSerialized)  // _STA: Status\n
    {\n
        Return (LSTA (SMD4))\n
    }\n
}\n
end;

into scope label _SB.PCI0.I2C5 replace_content begin

    Name (_HID, "INT3447")  // _HID: Hardware ID\n
    Method (_HRV, 0, NotSerialized)  // _HRV: Hardware Revision\n
    {\n
        Return (LHRV (SB15))\n
    }\n
\n
    Method (_CRS, 0, NotSerialized)  // _CRS: Current Resource Settings\n
    {\n
        Return (LCRS (SMD5, SB05, SIR5))\n
    }\n
\n
    Method (_STA, 0, NotSerialized)  // _STA: Status\n
    {\n
        Return (LSTA (SMD5))\n
    }\n
}\n
end;
```

###### 确认驱动情况

所有步骤完成后重启。若触控板未驱动或开机卡Busy timeout，则考虑更换不同的Pin list值。

对于本机而言，APIC Pin值为0x6D，采用中断模式的结果如下。进行速度常量修复后卡顿并未改善，故本机适合采用轮询模式。

|               平台               | GPP代号 | APIC引脚号（十进制） |          结果           |
| -------------------------------- | ------- | -------------------- | ----------------------- |
|   SunrisePoint[Kaby Lake(-R)]    |   D13   |          85          |     卡Busy timeout      |
|  CannonPoint(Lake)[Whiskylake]   |   D13   |         102          |     卡Busy timeout      |
|                                  |   G1    |          81          | 触控板卡顿，CPU占用极高 |
| CannonPoint(Lake)[Coffee Lake-R] |   D13   |         116          |     卡Busy timeout      |
|                                  |   G1    |         113          |     卡Busy timeout      |

### 电池修补

#### 修改前的准备

在Clover的kexts中放入VirtualSMC包的`SMCBatteryManager.kext`。打开`MaciASL`，正常情况下自动打开系统的DSDT。若出现错误提示，则检查Clover的config.plist，`Acpi`-`Fixs`下所有选项都应取消勾选。若仍然不行，则换另一个版本的MaciASL。

#### 查找修改字段

打开查找功能，搜索`EmbeddedControl`。可能会有一到多个，注意包含EC字样的那一个，记住其名称，本机为`EC0R`。

搜索`Field (EC名称`，本机为Field (EC0R。查看Field里所有大于8的元素，全部进行记录后，逐个搜索它们的名字。若查找结果只有一处，表明此变量仅被定义但未被使用，因此可以将此字段从记录结果中删除，否则将此字段保留。记录时需注明每个字段的对应位数（16位/32位/大于32位）。

点击`Patch`，复制下列代码，确认下面Change(s)处的数字不为零后点击`Apply`-`Close`。

```
// 添加B1B2方法，用于16位字段拆分
into method label B1B2 remove_entry;
into definitionblock code_regex . insert
begin
Method (B1B2, 2, NotSerialized)\n
{\n
Return(Or(Arg0, ShiftLeft(Arg1, 8)))\n
}\n
end;

// 添加B1B4方法，用于32位字段拆分
into method label B1B4 remove_entry;
into definitionblock code_regex . insert
begin
Method (B1B4, 4, NotSerialized)\n
{\n
	Store(Arg3, Local0)\n
	Or(Arg2, ShiftLeft(Local0, 8), Local0)\n
	Or(Arg1, ShiftLeft(Local0, 8), Local0)\n
	Or(Arg0, ShiftLeft(Local0, 8), Local0)\n
	Return(Local0)\n
}\n
end;
```

16/32位的字段需要拆分成多个8位的字段，而大于32位的字段需要修改其偏移地址。

##### 16/32位字段的修改

对于16位和32位的字段，需要拆分成2个（或4个）8位字段。

以`HWAK, 16`为例。这是个16位的字段，可以拆分成两个8位的字段，分别命名为`AK00`和`AK01`。命名可以随意，但是要确保命名为4个字符，且这个命名没有出现过，如下。

```
// 拆分前
HWAK, 16

// 拆分后
AK00, 8
AK01, 8
```

然后需要写出以下类似正则表达式的代码，以替换DSDT里的字段。

```
into device label EC code_regex HWAK,\s+16, replace_matched begin AK00,8,AK01,8, end;
```

相关含义解释如下，内容均根据自己的DSDT代码替换。

```
// EC
搜索EmbeddedControl，在其上方的Device（不是Field里的参数）

// HWAK,\s+16,
拆分前的字段（\s+表示多个空格，故此句含义为搜索HWAK, 16,）

// AK00,8,AK01,8,
拆分后的字段
```

把写好的正则表达式复制到Patch里，在中间的Before/After和下面的Patch/Change两个地方都显示内容后，点击Apply-Close即完成一个变量的替换。

再以`SBCH, 32`为例。

```
// 拆分前
SBCH, 32

// 拆分后
CH00, 8
CH01, 8
CH02, 8
CH03, 8
```

正则表达式如下。

```
into device label EC code_regex SBCH,\s+32 replace_matched begin CH00,8,CH01,8,CH02,8,CH03,8 end;
```

注意，所替换的`AK00,8,AK01,8,`逗号前后均无空格。若所替换的字段为列表中的最后一个（如图中的XSN1, 16），则Patch时会提示0 change(s)，此时应该把正则表达式中的`XSN1,\s+16,`改为`XSN1,\s+16`（因最后一个字段没有逗号）。

点击`Compile`，此时会出现报错，除Errors以外的不需要理会。对于报错内容为Object does not exist的情况，主要是替换字段名后，没有替换其它访问这个字段的地方所导致。双击报错信息以定位至错误位置，打开搜索，选中刚刚报错的那一行代码，搜索框里输入method，找到这段代码包含在哪个method里。

对于16位的字段，使用下面的正则表达式。

```
into method label \_WAK code_regex \(HWAK, replaceall_matched begin (B1B2(AK00,AK01), end;
```

相关含义解释如下，内容均根据自己的DSDT代码替换。

```
// \_WAK
代码对应的method

// \(HWAK,
此处注意，\只是起始符，(HWAK,才是替换前的字段

// (B1B2(AK00,AK01),
替换后的字段
```

对于32位的字段，则使用下列表达式。

```
into method label GBIF code_regex \(SBCH, replaceall_matched begin (B1B4(CH00,CH01,CH02,CH03), end;
```

若出现`\_SB.PCI0.LPC.EC.HWAK`调用的特殊情况，修改替换字段即可。

```
into method label \_WAK code_regex \(\\\_SB.PCI0.LPC.EC.HWAK replaceall_matched begin \(\\_SB.B1B2(\\_SB.PCI0.LPC.EC.AK00, \\_SB.PCI0.LPC.EC.AK01) end;
```

##### 大于32位字段的修改

先Patch下面的两段代码，其中H_EC改成计算机的Device。

```
// 改的字段挨着左括号时所用代码
into method label RE1B parent_label H_EC remove_entry;
into method label RECB parent_label H_EC remove_entry;

into device label H_EC insert
begin
Method (RE1B, 1, NotSerialized)\n
{\n
	OperationRegion(ERAM, EmbeddedControl, Arg0, 1)\n
	Field(ERAM, ByteAcc, NoLock, Preserve) { BYTE, 8 }\n
	Return(BYTE)\n
}\n
Method (RECB, 2, Serialized)\n
// Arg0 - offset in bytes from zero-based EC\n
// Arg1 - size of buffer in bits\n
{\n
	ShiftRight(Arg1, 3, Arg1)\n
	Name(TEMP, Buffer(Arg1) { })\n
	Add(Arg0, Arg1, Arg1)\n
	Store(0, Local0)\n
	While (LLess(Arg0, Arg1))\n
	{\n
	Store(RE1B(Arg0), Index(TEMP, Local0))\n
	Increment(Arg0)\n
	Increment(Local0)\n
	}\n
	Return(TEMP)\n
}\n
end;

// 改的字段挨着右括号时所用代码
into method label WE1B parent_label H_EC remove_entry;
into method label WECB parent_label H_EC remove_entry;
into device label H_EC insert
begin
Method (WE1B, 2, NotSerialized)\n
{\n
	OperationRegion(ERAM, EmbeddedControl, Arg0, 1)\n
	Field(ERAM, ByteAcc, NoLock, Preserve) { BYTE, 8 }\n
	Store(Arg1, BYTE)\n
}\n
Method (WECB, 3, Serialized)\n
// Arg0 - offset in bytes from zero-based EC\n
// Arg1 - size of buffer in bits\n
// Arg2 - value to write\n
{\n
	ShiftRight(Add(Arg1,7), 3, Arg1)\n
	Name(TEMP, Buffer(Arg1) { })\n
	Store(Arg2, TEMP)\n
	Add(Arg0, Arg1, Arg1)\n
	Store(0, Local0)\n
	While (LLess(Arg0, Arg1))\n
	{\n
	WE1B(Arg0, DerefOf(Index(TEMP, Local0)))\n
	Increment(Arg0)\n
	Increment(Local0)\n
	}\n
}\n
end;
```

计算大于32位字段的偏移量，逢八进一，从起始值往下算。以下面的代码为例子。

```
Offset (0x04),
	CMCM,   8, // 0x04
	CMD1,   8, // 0x05（8位是1字节，所以加1）
	CMD2,   8, // 0x06
	CMD3,   8, // 0x07
	Offset (0x18), // 这里空了一些，按开头所给出的偏移量计算
	Offset (0x19),
	SMST,   8, // 0x19
	MBMN,   80, // 0x1A
	MBPN,   96, // 0x25=0x1A+A+1（0x1A是上一个的起始地址，A的得来：80除以8得10，也就是上一个占了10个字节，16进制表示就是A。0x2A+A是它占到了哪个地址，它的下一个地址才是下一个开始，所以再加1。）
	GPB1,   8, // 0x32=0x25+C（96位占了12个字节+1
	GPB2,   8, // 0x33
	GPB3,   8, // 0x34
	GPB4,   8, // 0x35
```

若所替换字段挨着左括号，以`Store(MBMN, XXXX) `-> `Store(RECB(0x1A, 80), XXXX)`为例，利用下列正则表达式进行替换，Patch即可。

```
into method label XXXX code_regex \(SBMN, replaceall_matched begin (RECB(0xA0,128), end;
```

相关含义解释如下，内容均根据自己的DSDT代码替换。

```
XXXX
代码对应的method

\(SBMN,
替换前的字段

(RECB(0xA0,128),
替换后的字段，其中0xA0为所计算出来的偏移量，128为位数
```

若所替换字段挨着右括号，以`Store (Arg3, \_SB.PCI0.LPCB.EC0.SMD0)` -> `Store (Arg3, \_SB.PCI0.LPCB.EC0.WECB(0x1C,264,Arg3)`为例，则利用下列正则表达式进行替换。

```
into method label SMRW code_regex Store\s\(Arg3,\s\\_SB.PCI0.LPCB.EC0.SMD0\) replaceall_matched begin \\_SB.PCI0.LPCB.EC0.WECB(0x1C,264,Arg3) end;
```

#### 应用修改

再次点击Compile，确认无Error后，点击菜单File-Save As，类型选`ACPI Machine Language Binary`，名称为`DSDT.aml`。保存后复制到CLOVER/ACPI/patched，重启即可。

#### 说明

##### 16/32位正则表达式写法

16位和32位的区别在于，16位会调用B1B2方法，而32位会调用B1B4方法。因此正则表达式的基本格式如下。

```
// 16位字段
into method label [代码对应method] code_regex [替换前的字段，需要包含逗号] replaceall_matched begin (B1B2[替换后的字段，需要包含逗号] end;

// 32位字段
into method label [代码对应method] code_regex [替换前的字段，需要包含逗号] replaceall_matched begin (B1B4[替换后的字段，需要包含逗号] end;

// 特殊字段的处理（把整个地址复制进去即可）
B1B2(特殊字段1，特殊字段2)
B1B4(特殊字段1，特殊字段2，特殊字段3，特殊字段4)
```

##### 补丁结构

将前面所有Patch过的代码放到一起，即称为本机的电量补丁，示例如下。

```
// 字段拆分
into device label EC0 code_regex B0C3,\s+16, replace_matched begin C3HG,8,C3HF,8, end;
into device label EC0 code_regex B0SN,\s+16, replace_matched begin BSVN,8,BSVM,8, end;
into device label EC0 code_regex B1SN,\s+16 replace_matched begin SBUY,8,SBUP,8 end;

// 替换调用方法
into method label _BIX code_regex \(B0C3, replaceall_matched begin (B1B2(C3HG,C3HF), end;
into method label BIFA code_regex \(B0SN, replaceall_matched begin (B1B2(BSVN,BSVM), end;
into method label BIFA code_regex \(B1SN, replaceall_matched begin (B1B2(SBUY,SBUP), end;

// 数据处理方法
into method label B1B2 remove_entry;
into definitionblock code_regex . insert
begin
Method (B1B2, 2, NotSerialized)\n
{\n
Return(Or(Arg0, ShiftLeft(Arg1, 8)))\n
}\n
end;
```

##### Fix Mutex with non-zero SyncLevel补丁

如果打过电量补丁后电池状态显示为0%，则需要打Fix Mutex with non-zero SyncLevel补丁。可一次性打好自己电脑的电量补丁和Fix Mutex with non-zero SyncLevel补丁。

打开MaciASL，点击Patch，在左侧资源树中选择`_RehabMan Laptop`-`[sys]Fix Mutex with non-zero SyncLevel`，点击Apply-Close即可。

## 黑苹果完善

### 休眠相关

#### 通过终端禁止休眠

在终端输入以下命令以禁止。

```
sudo pmset -a hibernatemode 0
sudo pmset -a autopoweroff 0
sudo pmset -a standby 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
```

#### 通过Hackintool禁止休眠

在Hackintool里切换到`电源`选项卡，点击最下方的齿轮按钮即可。

#### 打开Clover的休眠

在终端输入以下命令，可打开Clover为黑苹果修补的休眠功能。

```
sudo pmset -a hibernatemode 29（或21）
```

### 禁用TouchID

无论笔记本是否有指纹传感器，由于SMBIOS设置的机型具有TouchID，而笔记本的指纹传感器无法被驱动，但macOS会误认为有TouchID，所以在输入密码或进行认证时，系统会等待指纹的反馈，从而导致键盘输入卡顿。

在Clover的kexts文件夹下放置`NoTouchID.kext`，重启后即可修复。

### 键位映射调整

安装`Karabiner Elements`，打开后选择Complex Modifications。点击Add rule-Import more rules from the Internet，在网页中搜索并安装`PC-Style Shortcuts`、`PC-Style Modifiers`、`Adobe Photoshop`，并在添加的类别上点击Enable All，完成键位映射调整。

### 注入白苹果三码同时开启HWP

打开终端并输入以下命令以查询支持HWP的机型，在输出内容中选择台式机机型或笔记本机型。注意，需挑选带HWP字样的机型，且尽可能选最新的机型（2017年及以后机型支持Sidebar功能）。若机型为Unknown，可用搜索引擎搜索其代码以确认机型。

```
cd /tmp && curl -s https://raw.githubusercontent.com/Piker-Alpha/freqVectorsEdit.sh/master/freqVectorsEdit.sh > /tmp/freqVectorsEdit.sh && chmod +x freqVectorsEdit.sh && /tmp/freqVectorsEdit.sh && sudo rm -rf /tmp/freqVectorsEdit.sh && sudo rm -rf /tmp/Mac-*.bin
```

部分主板需Clover的Drivers64UEFI文件夹中有`EmuVariableUefi-64.efi`。对于某些计算机，如果使用AptioMemoryFix.efi而不是OsxAptioFix*.efi，则可能可以在没有EmuVariableUefi-64.efi的情况下使本机NVRAM工作，故可自行试验。

打开Clover的config.plist，选择左侧`SMBIOS`，使用右侧上下箭头选择适合上面所确定的机型。

前往以下网站，复制Clover中的`Serial Number`到文本框进行查询（需要翻墙环境）。若出现具体机型，则说明此序列号可用。若提示无效，则此序列码不可使用，需点击Generate New生成新的Serial Number。

```
https://everymac.com/ultimate-mac-lookup/ 
```

前往以下Apple官网查询序列号。如果显示有购买日期和机型等保修信息，则此序列码不可使用，需返回Clover生成新的Serial Number。直到出现提示`很抱歉，这个序列号无效。请检查您的信息并再试一次`，则可使用。

```
https://checkcoverage.apple.com/cn/zh
```

点击左侧`System Parameters`，生成一个`UUID`，系统生成或随机生成都可以，并勾选`Inject System ID`。复制生成的UUID到SMBIOS的`SmUUID`里，复制SMBIOS里的`Board Serial Number`到Rt Variables的`MLB`里。点击获取`ROM`，确保下方三码正常匹配，退出并重启。

重启后打开Hackintool，转到工具选项卡，点击Intel图标，在工具输出中查看`enableHWP`的值，若为1则已成功开启HWP。同时，iCloud/iMassage/FaceTime应已可正常登录。

若出现问题，可打开`iMessageDebug`提取Mac的三码，查看MLB和ROM是否正常显示。若为`Failed`，则应在Clover中加上EmuVariableUefi-64.efi。若正常显示，可把结果保存，重启电脑后再打开iMessageDebug，若两次结果一致，则黑苹果已成功洗白。

### 系统时间调整

在Windows以管理员身份打开命令提示符，输入下列命令并重启以修复两个系统时间不一致的问题。

```
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
```

若仍不一致，则在Windows下修改时间为自动获取，在Mac下把时区改成北京并开启自动获取，重新启动即可。

### 显示修复

#### 开启平滑字体

在终端输入以下命令即可。

```
// 开启LCD平滑字体
defaults write -g CGFontRenderingFontSmoothingDisabled -bool NO

// 设置字体样式，3为最粗，1为最细
defaults -currentHost write -globalDomain AppleFontSmoothing -int 3
defaults -currentHost write -globalDomain AppleFontSmoothing -int 2
defaults -currentHost write -globalDomain AppleFontSmoothing -int 1

// 关闭LCD平滑字体
defaults write -g CGFontRenderingFontSmoothingDisabled -bool YES
```

#### 开启Hidpi

##### 通过一键Hidpi脚本

在终端下输入以下命令打开一键HiDPI脚本，安装以获得原生屏幕缩放体验。

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/xzhih/one-key-hidpi/master/hidpi.sh)"
```

##### 通过RDM

打开`DarwinDumper`，只勾选`EDID`，点击左侧的`Run`，会在桌面生成EDID文件夹。

打开`FixEDID`，点击Open EDID binary file，选择桌面文件夹的`/EDID/EDID.bin`。选择对应显示比例的显示器，此处为`Apple MacBook Pro Display（16:10）`。

在右侧输入想设置的分辨率，点击`Add Resolutions`添加分辨率，点击`Make`在桌面生成文件。参考分辨率如下。

```
3840×2160
3200×1800
2880×1620
2560×1600
2560×1440
1920×1080
1820×1200
1680×1050
1600×900
1440×810
1440×900
1440×810
1344×1008
1344×756
1280×960
1280×800
1280×720
1024×768
1024×576
960×600
960×540
840×525
840×524
800×600
800×450
720×450
640×480
```

将桌面的`DisplayVendorID-xxx`放到System/Library/Displays/Contents/Resources/Overrides。安装`RDM`并重启电脑，即可在RDM看到需要的分辨率。

##### 通过手动设置

打开终端并输入以下命令。

```
sudo defaults write /Library/Preferences/com.apple.windowserver.plist DisplayResolutionEnabled -bool true
ioreg -lw0 | grep IODisplayPrefsKey
```

输出类似如下信息，其中所有外部监视器由AppleDisplay标识，内部监视器由AppleBacklightDisplay标识。在行末可识别出DisplayVendorID和DisplayProductID，此处为`10ac`和`d06e`。

```
"IODisplayPrefsKey" = "IOService:/AppleACPIPlatformExpert/PCI0@0/AppleACPIPCI/IGPU@2/AppleIntelFramebuffer@2/display0/AppleDisplay-10ac-d06e"
```

打开以下网页并输入刚才得到的DisplayVendorID和DisplayProductID，DisplayProductName可以随便填写。注意一个分辨率需填写两个Scale Resolutions，其中第一个是原分辨率，另一个是长宽均为两倍后的分辨率，如添加1920×1080，则需填入3840×2160和1920×1080。注意相近的分辨率将会被系统过滤。

```
https://comsysto.github.io/Display-Override-PropertyList-File-Parser-and-Generator-with-HiDPI-Support-For-Scaled-Resolutions/
```

调整完成后下载plist文件，去掉扩展名并放在DisplayVendorID-10ac文件夹里（更改10ac为自己的VendorID），将文件夹复制到System/Library/Displays/Contents/Resources/Overrides并重启即可。

#### 注入EDID

注入EDID可修复笔记本显示器内屏黑屏/花屏的问题。若前面选择采用一键脚本的方式，由于脚本已有此功能，无需进行此步。

##### 通过patch-edid.rb

打开终端并输入以下命令以下载脚本。

```
curl -O https://gist.github.com/ejdyksen/8302862/raw/patch-edid.rb
```

打开脚本文件并查找`AppleDisplay`，这个代表外接显示器，若为内接显示器则需修改为`AppleBacklightDisplay`。然后继续在终端输入以下命令以运行并生成配置文件。

```
sudo ruby patch-edid.rb
```

将生成的配置文件复制到System/Library/Displays/Contents/Resources/Overrides/DisplayVendorID-xxx，若已存在配置文件，则将两个文件的内容合并，然后重启即可。

##### 通过Clover

在终端分别输入以下三个命令，并记录其输出。其中第一个输出为EDID信息，另外两个分别为屏幕设备的VendorID和ProductID。

```
ioreg -lw0 | grep -i "IODisplayEDID" | sed -e 's/.*<//' -e 's/>//'
ioreg -l | grep "DisplayVendorID"  
ioreg -l | grep "DisplayProductID"  
```

打开config.plist，切换到Graphics，将三者分别粘贴到对应的位置（VendorID和ProductID需要转换为十六进制），并勾选`Inject EDID`，保存并重启即可。

### CPU变频

在终端下输入以下命令以执行一键CPU变频脚本。

```
bash -c "$(curl -fsSL https://raw.githubusercontent.com/stevezhengshiqi/one-key-cpufriend/master/one-key-cpufriend_cn.sh)"
```

完成后把桌面的`CPUFriend.kext`和`CPUFriendDataProvider.kext`放到Clover的kexts中即可。

### 填充PCI列表

打开Clover Configurator，依次点击`Boot.log`-`Generate log`-`Save boot.log to desktop`，在桌面生成bootlog.txt文件。打开Hackintool，记住要添加的设备的设备ID，在bootlog.txt中搜索此ID，并记录同一行下形如`xx:xx.xx`的字符串，称为`pciaddr`。如现有设备NVME SSD SM961，其设备ID为0xA804，pciaddr为04:00.00。

打开config.plist，点击`Devices`-`Arbitrary`，点击PciAddr栏左下角的+号以新建条目。其中PciAddr填写刚才查询的值，Comment可记录下该设备的类型。双击此条目，并点击Value Type右下角的+号，添加两个条目，内容如下。

| Key            | Value                                    | Value Type |
| -------------- | ---------------------------------------- | ---------- |
| AAPL,slot-name | 端口位置，可任意填写，如Slot-1           | STRING     |
| model          | 设备名称，可任意填写，最好与源设备名一致 | STRING     |

完成后保存并重启即可。注意，显卡信息无法添加到PCI列表中，而声卡PCI属性的注入会导致layout-ID的覆盖，从而使声卡不工作。故若需注入声卡属性，需在config.plist下设置Devices-Audio-inject为`No`，在Boot-Arguments下添加`alcid=[layout-id]`。

### 简化Clover配置

在完成以上步骤并保证黑苹果能够正常启动后，可简化clover配置以达到更高效的启动效果。注意，对config.plist的修改需实时备份，以防止配置错误导致的系统不能进入的问题。

#### 系统自动生成

在终端输入以下命令自动生成config.plist，位置默认在个人文件夹目录。

```
/usr/local/bin/clover-genconfig >config.plist
```

#### 手动修改

简化后开机将不再跑码，且对系统的改造更小，有利于后续的进阶完善。

```
// ACPI
禁用所有更名补丁，若仍能进入系统，则直接删除

// Gui
Scan处只勾选Entries和Tool以在引导界面出现UEFI Shell和Boot Options菜单

// Kernel and Kext Patches
尝试禁用四个选项卡中所有的自带项

// System Parameters
BacklightLevel删掉（由于添加Clover显卡注入可能会修改此值，所以请将删除步骤放到最后一步进行）
```

### 热补丁综合修复

为更好发挥黑苹果性能，可根据OC-little包内的配置进行黑苹果功能的修补。注意，在应用热补丁时，需将补丁用MaciASL打开，另存为为ACPI Machine Language Binary类型文件，后缀名为aml，放入Clover的ACPI/patched中，同时注意注释中所提醒的ACPI更名。

#### 加载原生电源管理

打开系统DSDT，搜索CPU名字，一般为`PR.CPU0`、`PR.P000`、`PR.PR00`、`SB.CPU0`、`SB.P000`、`SB.PR00`、`SCK0.C000`、`SCK0.CPU0`中的一个。在OC-little的`X86`文件夹下找到对应的SSDT文件并放入，重启即可。

#### 添加缺失部件

打开OC-little的`添加丢失的部件`文件夹，然后打开系统DSDT，按照以下表格内容操作。

| 丢失的部件                    | 添加的SSDT |
| ----------------------------- | ---------- |
| PNP0200                       | SSDT-DMAC  |
| MCHC                          | SSDT-MCHC  |
| PNP0C01                       | SSDT-MEM2  |
| 0x001F0002（6代/7代机器）     | SSDT-PPMC  |
| PMCR、APP9876（8代/9代+机器） | SSDT-PMCR  |
| PNP0C0C                       | SSDT-PWRB  |
| PNP0C0E                       | SSDT-SLPB  |
| XSPI、0x001F0005（9代+机器）  | SSDT-XSPI  |

#### 睡眠秒醒补丁

打开OC-little的`0D6D补丁`文件夹，在README中有详细介绍。对于本机而言，本机为Method类型的GPRW情况，故使用SSDT-GPRW补丁即可，同时有以下二进制更名。

```
Comment: change GPRW to XPRW
Find: 47505257
Replace: 58505257
```

#### RTC修复

打开IORegistryExplorer，查找`AWAC`。若无，将第一段代码保存为SSDT-ARTC.aml，若有，将第二段代码保存为SSDT-AWAC.aml，然后放入Clover即可。

```
// SSDT-ARTC.aml
DefinitionBlock ("", "SSDT", 2, "ACDT", "RTC0", 0x00000000)
{
    External (_SB_.PCI0.LPCB, DeviceObj)    // (from opcode)
    External (_SB_.PCI0.LPCB.RTC_, DeviceObj)    // (from opcode)
    External (RTC_, DeviceObj)    // (from opcode)

    Scope (_SB.PCI0.LPCB)
    {
        Scope (RTC)
        {
            Method (_STA, 0, NotSerialized)  // _STA: Status
            {
                If (_OSI ("Darwin"))
                {
                    Return (Zero)
                }
                Else
                {
                    Return (0x0F)
                }
            }
        }

        Device (ARTC)
        {
            Name (_HID, EisaId ("PNP0B00"))  // _HID: Hardware ID
            Name (_CRS, ResourceTemplate ()  // _CRS: Current Resource Settings
            {
                IO (Decode16,
                    0x0070,             // Range Minimum
                    0x0070,             // Range Maximum
                    0x01,               // Alignment
                    0x02,               // Length
                    )
            })
            Method (_STA, 0, NotSerialized)  // _STA: Status
            {
                If (_OSI ("Darwin"))
                {
                    Return (0x0F)
                }
                Else
                {
                    Return (Zero)
                }
            }
        }
    }
}
```

```
// SSDT-AWAC.aml
DefinitionBlock ("", "SSDT", 2, "ACDT", "RTCAWAC", 0x00000000)
{
    External (STAS, FieldUnitObj)    // (from opcode)

    Scope (\)
    {
        If (_OSI ("Darwin"))
        {
            Store (One, STAS)
        }
    }
}
```

# 常见问题

## 引导启动

### 无法更改启动路径

在某些情况下（尤其是笔记本电脑），它们被硬编码为启动`EFI/Microsoft/Boot/bootmgfw.efi`而不是`EFI/BOOT/BOOTX64.EFI`，这将导致计算机启动系统的锁定。若遇到以上情况，则需将EFI分区里的EFI/Microsoft/Boot/bootmgfw.efi重命名为其他名称并将Clover的CLOVERX64.efi重命名为bootmgfw.efi，复制替换即可。注意，Windows的每次更新都会覆盖bootmgfw.efi，因此需要重新操作。

## 卡啰嗦模式

### 显示Still waiting for root device并禁行

换一个CLOVER文件夹。若无效，则在CLOVER/kexts里放`no AHCI补丁`及`USBInjectedAll.kext`，仍然无效可尝试启动参数`ahcidisk=1 debug=8`，还无效则可能是USB设备问题，需在Clover的config.plist中勾选FixOwnership并在Clover中添加USBInjectAll.kext。

### 卡在ApplelIntelLpssI2CController

在主机Windows下把SLE下的`AppleInteLpssI2CController.kext`删掉。若无效，再删`AppleIntelLpssI2C.kext`。

或者在Clover配置中禁用。用文本编辑器打开Clover的config.plist，在`KextsToPatch`加入以下代码即可。

```
<dict>
	<key>Comment</key>
	<string>Prevent Apple I2C kexts from attaching to I2C controllers, credit CoolStar</string>
	<key>Name</key>
	<string>com.apple.driver.AppleIntelLpssI2C</string>
	<key>Find</key>
	<data>SU9LaXQ=</data>
	<key>Replace</key>
	<data>SU9LaXM=</data>
	<key>InfoPlistPatch</key>
	<true/>
</dict>
<dict>
	<key>Comment</key>
	<string>Prevent Apple I2C kexts from attaching to I2C controllers, credit CoolStar</string>
	<key>Name</key>
	<string>com.apple.driver.AppleIntelLpssI2CController</string>
	<key>Find</key>
	<data>SU9LaXQ=</data>
	<key>Replace</key>
	<data>SU9LaXM=</data>
	<key>InfoPlistPatch</key>
	<true/>
</dict>
```

### 五国快速走过并提示有panic

关机后再次从移动硬盘启动，加上`Safe Boot（-x）`参数，并点击`Boot macOS without injected kexts`。正常进入以后再重启，此时不勾选`Safe Boot（-x）`，正常情况下可以进入系统。若无效，则可能为`DVMT`的设置问题，用下面IntelGraphicsDVMTFixup的方法修复。

### 卡在IOConsoleUsers:gIOScreenLockState 3或出现Busy Timeout

此错误是由于显卡驱动问题而导致。如果为双显卡，需屏蔽独显，驱动集显。

在Clover主页面中选择Options，点击Graphics injector，取消勾选`InjectIntel`并把`platform-id`修改为`0×12345678`，然后启动系统。若无效，则将LE下所有kexts备份并删除，再次启动。

若仍无效，则找到SLE下所有显卡kexts（AppleIntel，AMD，ATI，Ge，NVD开头），并将除核显外的其它所有kexts备份后删除（假设为Intel核显，则删除除AppleIntel开头外的其余所有显卡kexts）。

### 显示This version of Mac OS X is not supported on this platform!

Clover添加启动参数` -no_compat_check`即可。若不行，则换用其他版本的系统镜像。

## 系统运行

### 内置SATA硬盘显示为橙色图标

打开Clover的config.plist，在Kernel and kext Patches选项卡中的KextsToPatch加入以下代码即可。

```
Name: AppleAHCIPort
Find: 45787465 726E616C
Replace: 496E7465 726E616C
Comment: Define external drivers as internal to fix yellow drive icons
```

# 附录

## 专业术语解释

### 路径

`/System/Library/Extentions（SLE）`为系统主要kexts放置路径，`/Library/Extentions（LE）`为系统拓展kexts放置路径。在SLE或LE安装kexts后需用Kext Utility修复权限并重建缓存。

### Kexts

全称Kernel Extensions，也就是常说的驱动。

### Distros

Distributions的另一个名称，指发行版。不要使用macOS发行版。

### ACPI

高级配置和电源接口（ACPI）提供了一个开放标准，操作系统可以使用它来发现和配置计算机硬件组件，通过（例如）使未使用的组件进入睡眠状态来执行电源管理以及执行状态监视。

### OOB

开机即用，无需安装驱动/修补DSDT等。

### 分区格式

`APFS`为苹果专用格式，Windows下不可见，需安装`APFS for Windows`以挂载。

`NTFS`为Windows专用格式，macOS下只读。

`exFAT`可同时用于macOS和Windows，适用于储存文件和系统间文件交换。

### VID和PID

`VID`即`Vendor ID`，指供应商ID，在Windows下以`VID_`显示。

`PID`即`Device ID`，指设备ID，在Windows下以`PID_`显示。

## FakeSMC与VirtualSMC

两者作用相同，均用于向macOS提供假信息，模拟白苹果启动环境。FakeSMC早于VirtualSMC，且已不再更新，因此推荐使用VirtualSMC替换FakeSMC。注意，使用VirtualSMC时Lilu需为最新版本，否则将无法进入系统。VirtualSMC包作用分别如下。

| Kext名称                               | 作用                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| VirtualSMC                             | 替换FakeSMC.kext，需在Clover的driver64UEFI中删除SMCHelper-64.efi并复制VirtualSMC.efi到此目录 |
| SMCProcessor/SMCSuperIO/SMCLightSensor | 传感器驱动，用于替换CPUSensors/GPUSensors/LPCSensors/ACPISensor |
| SMCBatteryManager                      | 电源管理驱动，用于替换ACPIBatteryManager                     |

## 蓝牙与冷/热启动

`冷启动`指从关机状态下进入系统，`热启动`指从一个系统重启至另一个系统。

少数蓝牙设备中嵌入了蓝牙所需的固件，因此它们开机即可直接使用。但是许多设备依靠`固件上载器`才能正常运行，此过程中，系统启动蓝牙设备的电源，固件上传器检测到该设备已打开电源，然后将固件发送（上传）到`缓冲区`中。

由于Mac无法加载相关蓝牙固件，即使设备被正确识别，也无法与任何其他外围设备配对。而热启动时，Windows/Linux已完成固件上传，重启时缓冲区固件仍存在，因此蓝牙能够起作用。但在macOS 10.13.1中，当我们启动操作系统（热启动/冷启动）时，它将重置蓝牙设备，这样将导致被上载的固件的丢失。

## EFI（ESP）和MSR分区

### 作用

`EFI分区`用于`GUID分区表`形式的引导，需要BIOS支持UEFI启动。电脑启动时，从EFI分区读取启动配置以引导系统启动。EFI分区一般为`FAT格式（FAT16/FAT32）`，大小`200-300M`，可位于硬盘的任意位置，且一块硬盘可以存在多个EFI分区。只需把引导文件放置在EFI分区的`EFI/BOOT/BOOTX64.efi`即可。

MSR分区为Windows系统创建的分区，用于保存某些数据，用途不大，可删除或并入EFI分区，在创建EFI分区时也不必创建MSR分区。

### 分区结构及添加引导项

EFI分区的`EFI/BOOT`下，放置`CLOVER`文件夹引导Clover，放置`Microsoft`文件夹引导Windows。两者可同时放置。

对其它系统添加引导时，BOOT需用Clover的BOOT文件夹，把另一个文件夹放置于/EFI下，与CLOVER同级即可。Clover会自动识别出系统类型。

### 分区损坏的解决方法

若EFI分区已损坏，可在DiskGenius下将EFI分区删除，右键所得空闲空间，点击`建立ESP/MBR分区`，对话框里只选择`建立ESP分区`，固态硬盘需勾选`对齐分区`。若弹出是否删除原有引导记录的提示，则选择`删除`，然后将备份好的EFI文件夹复制进去。

打开`EasyUEFI`添加启动项，类型为`Linux`，路径为`/EFI/BOOT/BOOTX64.efi`。重启电脑并进入BIOS即可看到新建立的引导启动项，从此项启动即可。

## Kext

### Lilu

Lilu为其它kext提供接口支持，许多kext需要依赖Lilu才能运行。

### WhateverGreen

WhateverGreen中已包含`FakePCIID`，`IntelGraphicsFixup`，`NvidiaGraphicsFixup`，`Shiki` 和 `CoreDisplayFixup`。

### BTFirmwareUploader

本kext已失效，现用`BrcmPatchRAM`。

### VoodooI2C

VoodooI2C具有记忆性，由中断模式切换到轮询模式、在中断模式下修改有关触控板的DSDT等均不生效，需将VoodooI2C的相关驱动在Clover的kexts下删除再重新复制才能恢复初始设置。

### IntelGraphicsDVMTFixup

macOS的启动需要DVMT大于`32MB`，否则将导致内核崩溃。DVMT的大小可在BIOS设置，若BIOS无相应选项，可通过grub中的`setup_var`修改。或者使用本kext，但仅用于macOS 10.13及以下，在macOS Mojave下不适用。

在Majove及更新系统，需在Clover的config.plist应用以下补丁以修复。或者使用Hackintool，应用补丁时勾选`DVMT 32MB 预分配`。

```
<key>Properties</key>
<dict>
	<key>PciRoot(0x0)/Pci(0x2,0x0)</key>
		<dict>
			<key>framebuffer-fbmem</key>
			<data>
			AACQAA==
			</data>
			<key>framebuffer-patch-enable</key>
			<data>
			AQAAAA==
			</data>
			<key>framebuffer-stolenmem</key>
			<data>
			AAAwAQ==
			</data>
		</dict>
</dict>
```

### CodecCommander

用于修复唤醒无声问题，已并入AppleALC。

## 驱动（.efi）

### EmuVariableUefi

用于模拟NVRAM，从而将黑苹果洗白。若主板有原生NVARM，则无需放置。

# 相关下载

## Nvidia驱动相关

### WebDriver自动安装脚本项目

```
https://github.com/Benjamin-Dobell/nvidia-update
```

### WebDriver Manager

```
https://hackintosher.com/forums/attachments/webdrivermanager-2-9-4-zip.1289/
```

### CUDA Driver

```
https://www.nvidia.com/en-us/drivers/cuda/mac-driver-archive/
```

## Clover在线编辑网站

```
https://cloudclovereditor.altervista.org/cce/index.php
```

## Clover / OpenCore的config.plist示例

```
https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/
https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/
```

## Vmware虚拟机苹果破解补丁unlocker

```
https://github.com/theJaxon/unlocker/releases
```

## macOS镜像

### 虚拟机安装

```
https://pan.baidu.com/s/13rD1YbYwSKSDVhIxoUVxHw（提取码4nii）
```

### 硬盘安装

```
https://blog.daliansky.net/
```

## OC-little包

```
https://github.com/daliansky/OC-little

// 另附P-little包（较旧）
https://github.com/daliansky/P-little
```

## KEXT / SSDT / efi

大部分kexts可在以下网站找到。

```
https://kext.me/
https://bitbucket.org/RehabMan/
https://github.com/acidanthera
https://www.zdynb.cn/2020/hei-ping-guo-qu-dong.html
https://github.com/ClayMoreBoy/Hackintosh-Kext-Factory
```

### USBInjectAll.kext

```
https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/
```

### USBPower.kext

```
https://blog.xjn819.com/wp-content/uploads/2019/10/USBPower.kext_.zip
```

### 有线网卡

#### Realtek RTL810X（RTL8101E/8102E/8103E/8401E/8105E/8402/8106E/8106EUS）

```
http://www.insanelymac.com/forum/topic/296190-driver-for-realteks-rtl810x-fast-ethernet-series
```

#### Realtek RTL8111X/8168X（X=无/B/C/D/E/F/G）

```
http://www.insanelymac.com/forum/topic/287161-new-driver-for-realtek-rtl8111
https://code.google.com/p/os-x-realtek-network/downloads/list
```

### VirtualSMC

#### VirtualSMC包（含传感器/电池驱动）

```
https://github.com/acidanthera/VirtualSMC
```

#### 替代驱动

##### ACPIBetteryManager.kext

```
https://bitbucket.org/RehabMan/os-x-acpi-battery-driver/downloads/
```

##### FakeSMC包（含传感器驱动）

```
https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads/
```

### Lilu.kext

```
https://github.com/acidanthera/Lilu/releases
```

### WhateverGreen.kext

```
https://github.com/acidanthera/WhateverGreen/releases
```

### AppleALC.kext

```
https://github.com/acidanthera/AppleALC/releases
```

### VoodooHDA.kext

```
https://sourceforge.net/projects/voodoohda/
```

### VoodooI2C

```
https://github.com/alexandred/VoodooI2C/releases
```

### VoodooPS2Controller.kext

```
https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/
```

### NullEthernet.kext

```
https://bitbucket.org/RehabMan/os-x-null-ethernet/downloads/
```

### NoTouchID.kext

```
https://github.com/al3xtjames/NoTouchID/releases
```

### SSDT-PNLF.aml

```
https://bitbucket.org/RehabMan/applebacklightfixup/downloads/
```

### SSDT-GPI0.aml

```
DefinitionBlock ("", "SSDT", 2, "hack", "I2C", 0)
{
    External (_SB.PCI0.GPI0, DeviceObj)
    Scope (_SB.PCI0.GPI0)
    {
        Method (_STA, 0)
        {
            Return (0x0F)
        }
    }
}
```

### SSDT-RMNE.aml

```
https://github.com/RehabMan/OS-X-Null-Ethernet/blob/master/ssdt-rmne.aml
```

### SSDT-XOSI.dsl

```
https://raw.githubusercontent.com/RehabMan/OS-X-Clover-Laptop-Config/master/hotpatch/SSDT-XOSI.dsl
```

### ApfsDriverLoader.efi

```
https://github.com/acidanthera/ApfsSupportPkg
```

### AR9565驱动

```
http://www.mediafire.com/file/2rjm76vro8ha28l/9565.zip/file
```

## 工具

### Hackintool

```
http://headsoft.com.au/download/mac/Hackintool.zip
https://github.com/headkaze/Hackintool
```

### Kext Updater

```
https://bitbucket.org/profdrluigi/kextupdater/downloads/
```

### MaciASL

```
（RehabMan版）https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads/
（最新版）https://github.com/acidanthera/MaciASL/releases
```

### IORegistryExplorer 2.1

```
https://github.com/vulgo/IORegistryExplorer/releases
```

### gfxutil

```
https://github.com/acidanthera/gfxutil
```

### iMessageDebug

```
https://github.com/YCSuperM/ImessageDebug
```

### HoRNDIS

```
https://github.com/jwise/HoRNDIS/releases
```

### Clover

```
https://github.com/Dids/clover-builder/releases
https://sourceforge.net/projects/cloverefiboot/
```

### Clover Configurator

```
https://mackie100projects.altervista.org/download-clover-configurator/
```

### DarwinDumper

```
https://mac.softpedia.com/get/Utilities/DarwinDumper.shtml
```

### FixEDID

```
https://github.com/andyvand/FixEDID
```

### RDM

```
http://avi.alkalay.net/software/RDM/RDM-2.2.dmg
```

### VideoProc

```
https://www.videoproc.com/
```

### Kext Utility

```
https://en.freedownloadmanager.org/Mac-OS/Kext-Utility-FREE.html
```

### VDADecoderChecker

```
https://github.com/cylonbrain/VDADecoderCheck/releases
```

### Karabiner-Elements

```
https://github.com/pqrs-org/Karabiner-Elements
```

### 7z

```
http://www.7-zip.org/download.html
```

### AW EDID Editor

```
https://www.analogway.com/apac/products/software-tools/aw-edid-editor/
```

## 补丁源

### VoodooI2C

```
https://github.com/alexandred/VoodooI2C-Patches
```

### MaciASL

#### 常用补丁源

```
_RehabMan Laptop
http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master

_OS-X-ACPI-Debug
http://raw.github.com/RehabMan/OS-X-ACPI-Debug/master

_VoodooI2C-Patches
http://raw.github.com/alexandred/VoodooI2C-Patches/master

Sourceforge
http://maciasl.sourceforge.net

Gigabyte
http://maciasl.sourceforge.net/pjalm/gigabyte

ASUS
http://maciasl.sourceforge.net/pjalm/asus
```

#### 其它补丁源

```
Audio HDMI 5 Series
https://raw.github.com/toleda/audio_hdmi_5series/master

Audio HDMI HD3000/Sandy Bridge/6 Series
https://raw.github.com/toleda/audio_hdmi_hd3000/master

Audio HDMI HD4000/Ivy Bridge/7 Series
https://raw.github.com/toleda/audio_hdmi_hd4000/master

Audio HDMI UEFI Audio dsdt edits - Desktop/Laptop/Intel NUC
https://raw.github.com/toleda/audio_hdmi_uefi/master

Audio Realtek ALC injection
https://raw.github.com/toleda/audio_ALCInjection/master

Airport PCIe Half Mini
https://raw.github.com/toleda/airport_pcie-hm/master

Audio HDMI HD4600/Haswell/8 Series
https://raw.github.com/toleda/audio_hdmi_8series/master

PJALM General
http://pjalm.info/repos/general/

PJALM Graphics
http://pjalm.info/repos/graphics/

PJALM Intel Series 6
http://pjalm.info/repos/intel6/

PJALM Intel Series 7
http://pjalm.info/repos/intel7/

PJALM ASUS
http://pjalm.info/repos/asus/

PJALM Gigabyte
http://pjalm.info/repos/gigabyte/

PJALM MSI
http://pjalm.info/repos/msi/

PJALM ASRock
http://pjalm.info/repos/asrock/

PJALM Zotac
http://pjalm.info/repos/zotac/

Gigabyte
http://www.tonymacx86.com/DSDT/

HP ProBook 4x30s
https://raw.githubusercontent.com/RehabMan/HP-ProBook-4x30s-DSDT-Patch/master
```

# 参考教程

## Windows下VMware Workstations Pro15.5.0安装dmg镜像

```
https://hestyle.blog.csdn.net/article/details/104672651
```

## FIX BTFIRMWAREUPLOADER IN MACOS HIGH SIERRA

```
https://osxlatitude.com/forums/topic/10127-updated-nov-2017-fix-btfirmwareuploader-in-macos-high-sierra/
```

## alexandred/VoodooI2C-Gitter

```
https://gitter.im/alexandred/VoodooI2C/archives/2019/01/14
```

## Hackintool深度教程

```
https://blog.daliansky.net/Intel-FB-Patcher-tutorial-and-insertion-pose.html
```

## 修改DSDT实现电量显示教程

```
https://www.geekcj.com/333.html
https://blog.gzxiaobai.cn/2020/05/16/%E4%B8%BA%E5%B0%8F%E7%99%BD%E8%AE%BE%E8%AE%A1%E7%9A%84%E7%94%B5%E6%B1%A0%E6%95%99%E7%A8%8B%EF%BC%88DSDT%EF%BC%89/
```

## VoodooI2C驱动教程

```
https://www.penghubingzhou.cn/2019/01/06/VoodooI2C%20DSDT%20Edit/
https://www.penghubingzhou.cn/2019/07/24/VoodooI2C%20DSDT%20Edit%20FAQ/
https://voodooi2c.github.io/#GPIO%20Pinning/GPIO%20Pinning
```

## 英特尔®核芯显卡常见问答（WhateverGreen）

```
https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.cn.md
```

## 黑苹果精华必读：新手常见(五国)(-v图)错误解决

```
https://www.xmyy.com/article-254928.html
```

## Disabling discrete graphics in dual-GPU laptops

```
https://www.tonymacx86.com/threads/guide-disabling-discrete-graphics-in-dual-gpu-laptops.163772/
```

## /r/Hackintosh Vanilla Desktop Guide

```
https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/
```

## SSDT GPU (Graphics Card) Injection

```
https://www.tonymacx86.com/threads/ssdt-gpu-graphics-card-injection.183354/
```

## stevezhengshiqi/one-key-cpufriend

```
https://github.com/stevezhengshiqi/one-key-cpufriend
```

## AR9565 MAC黑苹果Bluetooth蓝牙驱动教程

```
https://www.longzc.cn/index.php/archives/308
```
