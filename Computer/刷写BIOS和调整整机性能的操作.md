---
title: 刷写BIOS和调整整机性能的操作
categories: Computer
abbrlink: BIOS-Editing-And-Flashing
date: 2019-11-26 10:33:29
mathjax: true
tags:
---

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g9baxry2kwj30sg0lctax.jpg)

对BIOS进行修改和刷写，可以解锁BIOS的其它功能。

<!-- more -->

# 常用工具

## BIOS备份

可使用BIOS Backup TooKit。

## 查看BIOS信息

可用DMIScope、RW。

# 资料

## 论坛

```
http://www.smxdiy.com/
https://www.bios-mods.com/
```

## 其它

```
https://github.com/roncapat/W230SD-Unlocked-AMI-BIOS
https://www.tech21century.com/best-free-overclocking-software-cpu-gpu-ram/
https://www.cnblogs.com/simadi/p/4357057.html
http://www.smxdiy.com/forum.php?mod=viewthread&tid=848&extra=page%3D2
https://www.win-raid.com/t3835f16-GUIDE-Fixing-HT-for-Coffee-Lake-CPUs-on-Skylake-and-Kaby-Lake-motherboards-Z-Z.html
https://www.win-raid.com/t154f16-Tool-Guide-News-quot-UEFI-BIOS-Updater-quot-UBU.html
https://www.rodsbooks.com/bios2uefi/
```

# 华硕主板官方UEFI BIOS降级

华硕主板在Windows下可用WinFlash进行BIOS刷写操作，但只能升级而不能降级。为绕过限制，可新建WinFlash的快捷方式，在快捷方式下右键点击属性，在目标一栏的最后加上参数`/nodate`，再双击打开快捷方式即可。注意，此法只能降级官方BIOS而不能刷修改版BIOS。

# EFI Shell与grub

## 制作引导盘

准备一个U盘，格式化为`FAT32`格式。打开以下链接，将整个项目Download下来后解压，将所有内容复制到U盘内。

```
https://github.com/holoto/efi_shell_flash_bios
```

打开以下链接并下载`modGRUBShell.efi`，改名为`bootx64.efi`，并复制到EFI/BOOT/grub efi shell for setup_var替换。

```
https://github.com/datasone/grub-mod-setup_var/releases
```

打开EFI/BOOT，把`efi shell for flash bios`目录里的文件复制到此目录即可启动EFI Shell，把 `grub efi shell for setup_var`目录里的文件复制到此目录即可启动grub。

重启电脑，在BIOS里选择U盘启动即可。

## 修改UEFI BIOS隐藏设置

以本机华硕笔记本为例。

### 提取BIOS条目

用`UEFITool`打开原BIOS，通过搜索功能搜索`BIOS Lock`（或`DVMT`，理论上指向同一位置），注意搜索框需切换到`Text`选项卡。在搜索结果所指向的`PE32`模块点击右键（一般并非整个GUID），选择`Extract body`，保存为bin文件，完成提取。

用`IFR Extractor`打开所提取的文件，保存为txt。打开txt后可以看到BIOS所有可调整的条目及选项，现以BIOS Lock条目说明如下。

```
// One Of表示条目名称，VarStoreInfo表示对应地址，VarStore表示值
One of: BIOS Lock, VarStoreInfo (VarOffset/VarName): 0x8A6, VarStore: 0x1, QuestionID: 0x931, size: 1, Min: 0x0, Max 0x1, Step: 0x0 {05 91 1C 08 1D 08 31 09 01 00 A6 08 10 10 00 01 00}

// 可选项及对应值
    Default: DefaultId: 0x0, value (8 bit): 0x1 {5B 06 00 00 00 01}
    One Of Option: Disabled, value (8 bit): 0x0 {09 07 04 00 00 00 00}
    One Of Option: Enabled, value (8 bit): 0x1 {09 07 03 00 00 00 01}

End One Of {29 02}
```

搜索并记录所需要修改的条目。如需修改BIOS Lock，则记下其地址`0x8A6`和要修改成的值`0x0[Disabled]`。

注意如果是如下的`Numeric`形式，则表明此条目为填写数字的内容，该地址的值在`Min`和`Max`之间即可。

```
Numeric: Wake up hour, VarStoreInfo (VarOffset/VarName): 0xF, VarStore: 0x1, QuestionId: 0x30, Size: 1, Min: 0x0, Max 0x17, Step: 0x1 {07 91 DC 00 DD 00 30 00 01 00 0F 00 10 10 00 17 01}
                 Default: DefaultId: 0x0, Value (8 bit): 0x0 {5B 06 00 00 00 00}
             End {29 02}
```

### 用grub修改BIOS里的条目

从U盘进入grub，输入以下命令以修改。下面以`BIOS Lock`为例。

```
setup_var_3 0x8A6 // 查找BIOS Lock地址
setup_var_3 0x8A6 0x0 // 更改BIOS Lock地址值为0x0[Disabled]
```

### 无法修改条目的解决方法

具体可参照以下链接。

```
https://habr.com/en/post/190354/
```

## 刷入修改版UEFI BIOS

### 修改BIOS文件

在Windows下打开`AMIWIN`，点击`Save`提取BIOS。用`AMIBCP`打开所提取的rom文件，左侧目录栏切换到`Setup`。在Setup内部为BIOS的具体条目，将需要显示的条目由Default改为`Supervisor`或`USER`，修改完成后保存为新的rom文件。对于本机而言，有两个Setup菜单，其中第二个才是华硕的UEFI BIOS菜单，且需要修改为Supervisor才能使显示生效。

### 用EFI Shell刷写BIOS

将修改好的BIOS修改为`bios.rom`并放入U盘根目录。从U盘进入EFI Shell，输入以下命令以备份并刷写BIOS。

```
map // 列出所有硬盘设备
fs0: // 选择U盘（根据上面的设备编号输入）
ls // 列出U盘内容
Fpt.efi -d backup.rom -bios // 备份BIOS到backup.rom
Fpt.efi -f bios.rom -bios // 刷写bios.rom
```

若提示Error 368错误，则需先按照上面Grub修改BIOS设置的流程，将BIOS Lock设为Disabled。注意，本机刷入修改版的BIOS后并无作用。

## 手动删减UEFI BIOS引导条目

进入EFI Shell并输入以下命令，记录硬盘信息和引导条目信息。

```
map // 列出所有硬盘设备
bcfg boot dump // 列出所有引导条目
```

### 删除条目

一般而言保留DevPath为HD开头的设备，而删除PciRoot开头的设备。记录选项编号并输入以下命令删除。

```
bcfg boot rm XX // XX是选项编号
```

### 增加条目

```
// XX为要添加的选项编号，FSX:\为Clover所在的EFI分区名称，CloverBoot为条目名称
bcfg boot add XX FSX:\EFI\CLOVER\CLOVERX64.EFI "CloverBoot"
```

# 解锁CPU性能

## 查看被限制性能的处理器

在Windows下打开`事件查看器`，选择`Windows日志`-`系统`，然后点击右侧栏的`筛选当前日志`，类型选择`Kernel-Processor-Power`即可。

## CPU超频工具

### XTU

安装完成后打开，点击左边`Advanced Tuning`，会出现提示警告。点击中间的那个`I agree,don't show again`即可打开高级选项。主要选项及调整方法如下。设定之后先用Stress Test进行压力测试，看看CPU能否稳定运行。

一般调节Turbo Boost Short Power Max、Turbo Boost Power Max、Turbo Boost Power Time Window。控制温度使用Core Voltage Offset。

| 项目                                    | 中文名               | 注释                                                         | 调整方法                                                     |
| --------------------------------------- | -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Core**                                |                      |                                                              |                                                              |
| Refrence Clock                          | 外频                 |                                                              | 保持Default                                                  |
| Max Non Turbo Boost Ratio               | 默频                 | 最大非睿频下倍频                                             |                                                              |
| Core Voltage Mode                       | CPU电压模式          | 分为 Adaptive（自适应）和 Static（静态）                     | 保持默认的Adaptive                                           |
| Core Voltage                            | CPU电压              | 设置CPU的绝对电压                                            | 保持Default                                                  |
| Core Voltage Offset                     | CPU偏移电压          | 相对于绝对电压偏移（增大或减小）的电压                       | 降压的Offset是负值。建议以0.01V（10mV）为单位降压，先调整Offset至-0.01V，然后转至Stress Test，勾选CPU Stress Test并测试10分钟，若无异常继续降0.01V。在降压0.05V之后以0.005V（5mV）为单位进行降压，并且每次降压之后应该至少有20分钟的P95或者FPU的测试并且保证显卡有负载（比如GPUZ的Render Test或者甜甜圈）。若意外关机，则把最后一次降压幅度减小0.01V，并测试两小时，无异常即可 |
| Enhanced Intel SpeedStep Technology     | 电源管理相关         | 控制CPU电压和频率关系，使得不同场景下CPU可以在高性能和电池优化模式中来回切换 |                                                              |
| Intel Turbo Boost Technology            | 睿频加速技术         |                                                              |                                                              |
| Processor Core IccMax                   | 核心最大电流         | 调节核心电流                                                 | 保持Default                                                  |
| AVX Ratio Offset                        | AVX倍频补偿          | 控制CPU跑AVX指令集（比如浮点运算）的最大频率。比如设置x3的AVX频率补偿，倍频45x外频100MHz，那么在浮点运算的时候，CPU的主频只会到（45-3）*100=4.2GHz | 保持Default                                                  |
| Turbo Boost Short Power Max Enable      | 短时间超功耗睿频模式 | 可以让CPU短时间睿频到一个超过标定频率很多的一个频率，但代价是温度高，而且CPU可能承受不住太长时间 |                                                              |
| Turbo Boost Short Power Max             | 短时间最大睿频功耗   | PL2，决定瞬间性能（在瞬间负载下这个数值会限制CPU的功耗）     | 拉至最大                                                     |
| Turbo Boost Power Max                   | 长时间睿频最大功耗   | PL1，决定稳定性能（PL1数值上肯定不会超过PL2）                | 拉至最大                                                     |
| Turbo Boost Power Time Window           | 睿频加速时间         | PL2时间                                                      | 拉至最大                                                     |
| Turbo Boost Short Power Time Windows    | 睿频最大功耗持续时间 | PL1时间（在这个此时间内，CPU功耗可以突破PL1但不高于PL2）     |                                                              |
| Multipliers                             | 多核倍频             | 超频专用                                                     | 拉至最大                                                     |
| **Cache**                               |                      |                                                              |                                                              |
| Cache Voltage Mode                      | 缓存电压模式         |                                                              |                                                              |
| Cache Voltage                           | 缓存绝对电压         |                                                              |                                                              |
| Cache Voltage Offset                    | 缓存偏移电压         |                                                              |                                                              |
| Cacha IccMac                            |                      |                                                              |                                                              |
| **Graphics**                            |                      |                                                              |                                                              |
| Precessor Graphics Ratio Limit          |                      |                                                              |                                                              |
| Processor Graphics Valtage Mode         |                      |                                                              |                                                              |
| Processor Graphics Valtage              |                      |                                                              |                                                              |
| Processor Gtaphics Valtage Offset       |                      |                                                              |                                                              |
| Processor Graphics IccMax               |                      |                                                              |                                                              |
| Processor Graphics Media Voltage        |                      |                                                              |                                                              |
| Processor Graphics Media Voltage Offset |                      |                                                              |                                                              |
| Processor Graphics Unslice Iccmax       |                      |                                                              |                                                              |
| **Other**                               |                      |                                                              |                                                              |
| System Agent IccMax                     |                      |                                                              |                                                              |

XTU可用命令行进行控制。打开记事本，复制一下代码并保存，后缀名改为ps1，即可通过PowerShell运行。各种参数ID可以打开C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XtuCLI.exe查看。

```
$status = Get-Service -Name "XTU3SERVICE" | Select-Object "status" | Format-Wide
if ($status -ne "Running") {start-service -name "XTU3SERVICE"}
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 48 -v 45.00 #TDP
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 47 -v 60.00 #短时间功耗墙
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 66 -v 28.00 #睿频时间
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 34 -v -125 #所降电压
sleep 4
stop-process -id $PID -force
```

### Throttlestop

XP下可运行的版本为2.99.6和8.40。

打开软件后勾选`Set Multiplier`和`Speed Shift EPP`，取消勾选`BD PROCHOT`，将Set Multiplier后面的数字拉到最大。点击`Limit Reasons`可查看功耗被限制的原因。

点击BCLK可设置PL1/PL2，可先尝试200。点击FIVR可设置电压，首先选择CPU Core/Cache，然后勾选Adjustable Voltage，再将下方的Offset Voltage拉到最大。

其余选项解释如下。

| 项目                     | 中文名        | 注释                                                         | 调整方法 |
| ------------------------ | ------------- | ------------------------------------------------------------ | -------- |
| Clock/Chipset Modulation | 时钟/芯片调整 | 大多数新芯片不开启，开启后ThrottleStop中的功能有的不起作用   | 关闭     |
| Set Multipliet           | 乘数调整      |                                                              | 拉到最大 |
| Speed Shift - EPP        | 转换速度      | 如果勾选需要到TPL选项里面开启，开启了后根据CPU负载率来快速的降低和提升频率，数字可填，0是保持较大的性能上下浮动，可以向下浮动很低的频率，一般用于老CPU |          |
| Power Saver              | 节电调整      | 让机器用总线速率的一半运行，使相对老的CPU实现降低频率来降低功耗 |          |
| Disable Turbo            | 关闭睿频      |                                                              |          |
| BD PROCHOT               | 温度墙        |                                                              |          |
| C1E                      |               | 空闲时随机上下数值频率                                       |          |

### MemSet

调节内存参数。在打开软件时需先手动安装Drivers文件夹中的Setup.inf。

| 项目                         | 中文名           | 注释                                                       | 调整方法 |
| ---------------------------- | ---------------- | ---------------------------------------------------------- | -------- |
| Clock/Chipset Modulation     | CAS#延迟         | 大多数新芯片不开启，开启后ThrottleStop中的功能有的不起作用 | 调至最低 |
| Precharge Delay              | 预充电延迟       |                                                            | 5-11之间 |
| RAS# Precharge               | tRP行预充电时间  | 越低表示在不同行切换的速度越快                             |          |
| Refresh Mode Select          | 内存更新模式选择 |                                                            |          |
| Refresh Cycle Time           | 更新周期         |                                                            |          |
| Performance Mode             | 增强模式         |                                                            |          |
| Write Recovery Time          | 写到读的延迟     |                                                            |          |
| Read-Write Turnaround clocks | 读到写的转变时钟 |                                                            |          |

### SetFSB

需要使用2.1.91.1版本，否则将提示无法使用。

使用前需知道CPU所用的时钟发生器的型号，可尝试搜索得到。若仍未知，则首先打开CPU-Z，记录总线速度和额定FSB，然后打开SetFSB，逐个时钟发生器进行尝试，点击Get FSB，查看下面的FSB值是否与CPU-Z中得到的一致，若一致则有可能为该时钟发生器。

确定时钟发生器的型号后，拖动滑块即可进行调整。本机可能的时钟发生器为ICS9LPR501HGLF和ICSLP505-2HGLF。

### 其它工具

可用CPU-Tweaker、ClockGen等。

## 显卡超频工具

### ATI Tray Tools

ATI显卡超频工具。

## 修改MSR寄存器的值

CPU的功率限制被写在MSR寄存器中。通过修改MSR寄存器的值，可以提升CPU的功率。

### 开启CFG功能

在BIOS中，若CFG Lock处于Enabled状态，将不能对MSR寄存器进行读写。故须先用Grub将CFG Lock设为Disabled。

### 查看CPU信息和寄存器地址

#### 通过Hackintool（推荐）

进入macOS系统，打开Hackintool，切换到工具选项卡。点击Intel图标，将输出复制到文本文档中，该输出即有CPU的相关信息，包括MSR寄存器的地址。

#### 通过AppleIntelInfo.kext

下载压缩包并用Xcode编译，编译完成后切换到kext所在目录，输入以下命令以获取CPU信息。

```
sudo chown -R root:wheel AppleIntelInfo.kext
sudo chmod -R 755 AppleIntelInfo.kext
sudo kextload AppleIntelInfo.kext
// 在终端查看
sudo cat /tmp/AppleIntelInfo.dat | more
// 或将包含CPU信息的文件复制到当前目录下
sudo cp /tmp/AppleIntelInfo.dat AppleIntelInfo.txt
sudo kextunload AppleIntelInfo.kext
```

#### 通过官方文档

在Linux的终端下输入以下命令以获取CPU信息。

```
cat /proc/cpuinfo | less
```

根据输出判定CPU类型，主要用到的是cpu family和model，将其转为十六进制后，可得到CPUID。CPUID由DisplayFamily_DisplayModel构成，以本机为例，本机的DisplayFamily为06h，DisplayModel为8Eh（均为十六进制），故CPUID为06_8EH。

```
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 142
model name      : Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
stepping        : 10
......
```

在Intel官方文档下载页面中下载Four-Volume Set of Intel® 64 and IA-32 Architectures Software Developer’s Manuals中的第四章，链接如下。

```
https://software.intel.com/en-us/download/intel-64-and-ia-32-architectures-software-developers-manual-volume-4-model-specific-registers
```

在文档中搜索CPUID（06_8EH）、CPU架构（Kaby Lake）或CPU代数（8TH GENERATION），查询其MSR地址。如本CPU在2.17一节中。

### 查看寄存器内容含义

在Intel官方文档下载页面中下载Four-Volume Set of Intel® 64 and IA-32 Architectures Software Developer’s Manuals中的第三章，链接如下。

```
https://software.intel.com/en-us/download/intel-64-and-ia-32-architectures-sdm-combined-volumes-3a-3b-3c-and-3d-system-programming-guide
```

打开文档并搜索需要更改的寄存器（如MSR_PKG_POWER_LIMIT），即可查询其对应含义。

### 准备修改环境

进入Linux系统并执行以下命令，以安装msr-tools并加载系统MSR模块。

```
sudo apt-get install msr-tools
sudo modprobe msr
```

然后便可利用msr-tools完成MSR寄存器的读取与修改。主要用到以下两个命令，均为十六进制。 

```
// 读取0x[A]地址的值
sudo rdmsr 0x[A]

// 将0x[A]地址的值改为0x[B]
sudo wrmsr 0x[A] 0x[B]
```

### 修改寄存器内容

以下以修改Package power limits为例进行说明。

首先需要读取MSR_RAPL_POWER_UNIT寄存器的值，此寄存器存储功率/能量/时间单位。在本CPU中，该寄存器的地址为0x606，故输入以下命令。

```
sudo rdmsr 0x606
// 输出A0E03
```

从官方文档（第三章）可得到关于此寄存器的说明，原文如下。

```
MSR_RAPL_POWER_UNIT (Figure 14-35) provides the following information across all RAPL domains: 
Power Units (bits 3:0): Power related information (in Watts) is based on the multiplier, 1/ 2^PU; where PU is an unsigned integer represented by bits 3:0. Default value is 0011b, indicating power unit is in 1/8 Watts increment. 
Energy Status Units (bits 12:8): Energy related information (in Joules) is based on the multiplier, 1/2^ESU; where ESU is an unsigned integer represented by bits 12:8. Default value is 10000b, indicating energy status unit is in 15.3 micro-Joules increment. 
Time Units (bits 19:16): Time related information (in Seconds) is based on the multiplier, 1/ 2^TU; where TU is an unsigned integer represented by bits 19:16. Default value is 1010b, indicating time unit is in 976 micro- seconds increment. 
```

将A0E03转换为二进制为1010 0000 1110 0000 0011。bits A:B表示二进制的第B到A位（从低位开始数，个位是第0位），故按照说明可得下表。

| 原文                              | 含义   | 内容    | 含义                | 说明                                                           |
|---------------------------------|------|-------|-------------------|--------------------------------------------------------------|
| Power Units (bits 3:0)          | 功率单位 | 0011  | 0.125W            | 0011转换为十进制为3，故功率以$(\frac{1}{2})^3$=0.125W为单位                 |
| Energy Status Units (bits 12:8) | 能量单位 | 01110 | 0.00006103515625J | 01110转换为十进制为14，故能量以$(\frac{1}{2})^{14}$=0.00006103515625J为单位 |
| Time Units (bits 19:16)         | 时间单位 | 1010  | 0.0009765625s     | 01110转换为十进制为10，故时间以$(\frac{1}{2})^{10}$=0.0009765625s为单位     |

Package power limits位于MSR_PKG_POWER_LIMIT寄存器中，本CPU对应的地址为0x610。输入以下指令得到该地址的值，并对应进行修改。

```
sudo rdmsr 0x610
// 输出42816000DD8160
（二进制为0000 0000 0100 0010 1000 0001 0110 0000 0000 0000 1101 1101 1000 0001 0110 0000）
```

| 原文                                          | 内容              | 更改为 | 说明                                                                                |
|---------------------------------------------|-----------------| --- |-----------------------------------------------------------------------------------|
| Package Power Limit #1(bits 14:0)           | 000000101100000 |     | 原值转换为十进制为352，因功率以0.125W为单位，故为352×0.125=44W                                        |
| Enable Power Limit #1(bit 15)               | 1               | 0   | 0为关闭，1为开启                                                                         |
| Package Clamping Limitation #1 (bit 16)     | 1               |     |                                                                                   |
| Time Window for Power Limit #1 (bits 23:17) | 1101110         |     | 时间限制值= $(2^Y*(1+Z/4)*TimeUnit)$，其中Y是寄存器的第21:17位，Z是寄存器的第23:22位，TimeUnit是前面所定义的时间单位 |
| Package Power Limit #2(bits 46:32)          | 000000101100000 |     |                                                                                   |
| Enable Power Limit #2(bit 47)               | 1               |     | 0为关闭，1为开启                                                                         |
| Package Clamping Limitation #2 (bit 48)     | 0               |     |                                                                                   |
| Time Window for Power Limit #2 (bits 55:49) | 0100001         |     | 时间限制值= $(2^Y*(1+Z/4)*TimeUnit)$，其中Y是寄存器的第21:17位，Z是寄存器的第23:22位，TimeUnit是前面所定义的时间单位 |
| Lock (bit 63)                               | 0               |     | CFG锁，若为1则无法写入                                                                     |

将更改后的内容从低位到高位组成一个64位的二进制数并转换为十六进制 ，然后用wrmsr写入即可。写入后用rdmsr读取，检验结果是否正确。

### 可修改的寄存器

#### MSR_PKG_POWER_LIMIT

```
0x610H
  42819800FC80C8h
  00000000 01000010 10000001 10011000
  00000000 11111100 10000000 11001000

  14:0 = Pkg power limit = 11001000b (200d, c8h) = 25
  15:15 = Pkg power enabled (bool) = 1b
  16:16 = Pkg clamping limit (bool) = 0b
  23:17 = Pkg power limit time window = 11b(3d) 11110b(30d) = 2^30*(1+3/4)*(1/2)^10=1073741824

  46:32 = Pkg power limit 2 = 110011000b (408d, 198h) = 51
  47:47 = Pkg power 2 enabled (bool) = 1b
  48:48 = Pkg clamping limit 2 (bool) = 0b
  55:49 = Pkg power limit time window = 01b(1d) 00001b(1d) = 2^1*(1+1/4)*(1/2)^10=0.00244140625
  63:63 = MSR lock (bool) = 0b
```

#### MSR_PLATFORM_INFO

第28位为1时可修改MSR_TURBO_RATIO_LIMIT的内容。

```
    15:8 = Maximum non-turbo (RO) bool
    28 = Programmable ratio limit for turbo (RO) bool
    29 = Programmable TDP limit for turbo (RO) bool
    30 = Programmable TJ offset (RO) bool
```

#### MSR_TURBO_RATIO_LIMIT

```
0x1ADH
  28282828h

  7:0 = Ratio 1C = 40
  15:8 = Ratio 2C = 40
  23:16 = Ratio 3C = 40
  31:24 = Ratio 4C = 40
```

# 修改Phoenix BIOS显示隐藏菜单

## 检查BIOS长度

下载Phoenix BIOS Editor以及要修改的原版Phoenix BIOS文件。用Phoenix BIOS Editor打开BIOS文件，若提示`Invaild rom length`，则用十六进制编辑器打开BIOS文件，把00100010h至结尾的内容清除后保存即可，这一部分是BIOS文件的附加信息。

## 提取BIOS模块

打开BIOS文件后进入C:\Program Files (x86)\Phoenix Technologies Ltd\BIOS Editor\TEMP目录，复制OLD1.RLS，STRINGS0.ROM（或STRINGS00.ROM），TEMPLAT0.ROM（或TEMPLAT00.ROM）到任意位置。

用十六进制编辑器打开以上复制出来的文件，复制OLD1.RLS的头两个字节（如D6 70）并在STRING0.ROM中查找这两个HEX值，取搜索结果较前的一个。在STRING0.ROM中，这两个字节之前（不含）的被称为header，长度应该为0x1C。删掉header并保存为STRING.ROM。

## 查找要显示的菜单位置

在Phoenix BIOS Editor的STRING窗口中搜索要解锁的菜单名称。此处以Intel VT菜单为例，可搜索Virtualization、VT Feature等关键词，可确定菜单名称为Intel(R) Virtualization Technology。在STRING.ROM中搜索前面所找到的名称，记下字符串开头对应的地址值，此处以0x00002CFD为例。忽略高四位，将地址按小端规则重新排列，并作为HEX值在STRING.ROM中搜索，此处为FD 2C。记下这两个字节所对应的地址值，此处为0x0000030A。

忽略高四位，将地址按小端规则重新排列，并作为HEX值在TEMPLAT0.ROM中搜索，此处为0A 03。从0A 03的前两个字节开始，摘录二十个字节如下。

| 地址       | HEX值                                           |
| ---------- | ----------------------------------------------- |
| 0x000012D5 | 00 14 0A 03 0C 03 F5 46 EA 46 D4 46 DF 46 81 03 |
| 0x000012E5 | CA 05 CC 05                                     |

前两个字节为00 14，称为menu header，其中00代表类型，14代表长度。不同代号所表示的类型如下表所示。

| 代号    | 类型               |
| ------- | ------------------ |
| 00 / 01 | 选择（Pick Field） |
| 10      | 文本               |
| 11      | 信息               |
| 21      | 时间               |
| 22      | 日期               |
| 23      | /                  |

0A 03和0C 03可以作为菜单的标志。如果这是一个菜单，则0A和0C之间应当相差2，同理结尾的CA 05和CC 05也满足CA+2=CC。

将81 03按小端规则处理后可知Token ID为0381，是BIOS中开关此功能的寄存器，可用于在不刷入修改版BIOS的情况下直接修改条目的值。具体做法可参照附录。

包含menu header后字符串的地址为0x000012D5，至此已找到所需菜单的位置，记住此地址。

## 放置菜单到适宜位置

在Phoenix BIOS Editor的Setup Table窗口中点击Emulate，显示模拟BIOS。有些菜单尽管在模拟器中可见，刷入后却不可见，对于这些菜单，最好的方法是将其移至其它有空位的标签页中。

记录原标签页的排序，经查看后可知在Main标签页中有空位，则在STRINGS.ROM中搜索Main，注意需要全匹配。记录字符串的地址，忽略高四位，将地址按小端规则重新排列，并作为HEX值在STRINGS.ROM中搜索，取结果的地址值。

忽略高四位，将地址按小端规则重新排列，并作为HEX值在TEMPLAT0.ROM中搜索，摘录它们的前两个字节，即为menu header，理论上种类为10（Generic Text）。包含menu header后取字符串地址，这里为0x00000AA1。


忽略高四位，将地址按小端规则重新排列，并作为HEX值在TEMPLAT0.ROM中搜索，此处为A1 0A。查看前面所记录的标签页的顺序，此处为Main-Advanced-Security-Boot-Exit，Main为第一个标签页，其HEX值为A1 0A，故Advanced及以后的标签页的HEX值为从Main开始往后取。每个标签页占两个字节，且每个标签页之间相距两个字节。取Main的前两个字节、Main本身以及Main后的20个字节，如下所示。

| 地址       | HEX值                                           |
| ---------- | ----------------------------------------------- |
| 0x0000015A | 00 00 A1 0A 74 01 A5 09 B8 01 D3 0A 04 02 3F 0B |
| 0x0000016A | 5E 04 17 0A 6E 02 00 00                         |

Main的HEX值为A1 0A，其余分别为Advanced（A5 09），Security（D3 0A），Boot（3F 0B），Exit（17 0A）。每个菜单HEX值后的两个字节表示这一菜单对应的地址，此处为74 01。跳到地址0x00000174，此处即为Main选项卡中包含的所有菜单。

留意到Advanced选项卡的地址为0x000001B8，则Main选项卡的菜单不能超出该位置。在选项卡内部，每条菜单用两个字节表示，这两个字节为该菜单的地址，每条菜单之间用00 00分隔。如果两条菜单之间用00 00 00 00分隔（不小于四组00），则00 00 00 00后的菜单将不可见。因此，直接向00 00 00 00处填充菜单即可解锁新菜单。注意，每组选项卡之间有00 00 00 00作为分隔。

为验证所找到的位置是否正确，可以看到该选项卡的首两个字节为E9 0A，对应的菜单应该是System Time。跳到地址0x00000AE9，可以看到首两个字节为21 0A，为菜单的menu header，其中21表示时间，故所找到的位置是正确的。

在标签页内寻找可以插入的位置，一般为00 00 00 00，替换前面的00 00即可。前面需要解锁的菜单地址为0x000012D5，忽略高四位，将地址按小端规则重新排列并填入，变为D5 12 00 00即可。注意，修改时不能改变文件大小，即只能替换、不能删除文件中的任何一个字节。如果没有空位，可以将一些平时不会用到的菜单替换掉。

## 打包成新的BIOS文件

将需要解锁的菜单都写入后，保存TEMPLAT0.ROM文件，并复制到C:\Program Files (x86)\Phoenix Technologies Ltd\BIOS Editor\TEMP覆盖。回到Phoenix BIOS Editor，由于没有在其界面中操作，此时并不能保存文件。故在Setup Table中点击Main选项卡，任意双击一个`*`号，此时已经可以保存，点击保存即可。

注意，每个BIOS只能在原版的基础上修改一次，在已经修改过的BIOS文件中不能二次修改，否则Phoenix BIOS Editor会报错。

# 刷入修改版Phoenix BIOS

注意，理论上Phoenix BIOS的后缀名可以改为任意名称，因此可以直接把BIOS.rom改为BIOS.wph。

## 通过Phoenix Secure WinFlash

直接按照软件操作即可。

## 通过phlash16

phlash16为DOS下的Phoenix BIOS刷写工具，进入DOS环境后通过以下命令刷写即可，其中bios.rom为要刷写的BIOS。由于E2ROM寄存器才可被刷写，因此提示Invaild flashint.bin说明该BIOS无法被刷入。关于phlash16的具体参数，可通过输入`phlash16 /?`查询。

```
phlash16 bios.rom /x /s /c /mfg /mode=3
```

# 修改Phoenix BIOS隐藏条目的值

从以下网站下载symcmos。将BIOS设置重置为出厂默认值。

```
http://xdel.ru/downloads/bios-mods.com-tools/SYMCMOS/
```

重启进入DOS环境，切换到symcmos所在目录，执行以下命令。

```
symcmos -v2 -lDefault.txt
```

使用文本编辑器，打开并修改条目，其中条目地址在修改Phoenix BIOS时可以得出。完成后引导至DOS，输入以下命令以刷写，其中NameOfModifiedFile为修改后的文本文档名称，注意与`-u`之间没有空格。

```
symcmos -v2 -uNameOfModifiedFile
```

执行完成后重启即可。

# 附录

## Intel官方文档全集

```
https://software.intel.com/en-us/articles/intel-sdm?wapkw=software+developer
```

## 本机Phoenix BIOS菜单地址

| 选项                               | 地址       |
| ---------------------------------- | ---------- |
| Main                               | 0x00000174 |
| Frequency Radio                    | 0x000008B5 |
| Intel(R) Virtualization Technology | 0x0000088D |

## 本机华硕笔记本CPU信息（由Hackintool输出）

```
AppleIntelInfo.kext v3.0 Copyright © 2012-2017 Pike R. Alpha. All rights reserved.

Settings:
------------------------------------------
enableHWP............................... : 1
logMSRs................................. : 1
logIGPU................................. : 1
logIntelRegs............................ : 0
logCStates.............................. : 1
logIPGStyle............................. : 1
InitialTSC.............................. : 0x9fd6f3b73 (2 MHz)
MWAIT C-States.......................... : 286531872

Processor Brandstring................... : Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz

Processor Signature..................... : 0x806EA
------------------------------------------
 - Family............................... : 6
 - Stepping............................. : 10
 - Model................................ : 0x8E (142)

Model Specific Registers (MSRs)
------------------------------------------

MSR_IA32_PLATFORM_ID..............(0x17) : 0x1C000000000000
------------------------------------------
 - Processor Flags...................... : 7

MSR_CORE_THREAD_COUNT.............(0x35) : 0x40008
------------------------------------------
 - Core Count........................... : 4
 - Thread Count......................... : 8

MSR_PLATFORM_INFO.................(0xCE) : 0x4043DF1011400
------------------------------------------
 - Maximum Non-Turbo Ratio.............. : 0x14 (2000 MHz)
 - Ratio Limit for Turbo Mode........... : 1 (programmable)
 - TDP Limit for Turbo Mode............. : 1 (programmable)
 - Low Power Mode Support............... : 1 (LPM supported)
 - Number of ConfigTDP Levels........... : 2 (additional TDP level(s) available)
 - Maximum Efficiency Ratio............. : 4
 - Minimum Operating Ratio.............. : 4

MSR_PMG_CST_CONFIG_CONTROL........(0xE2) : 0x7E000008
------------------------------------------
 - I/O MWAIT Redirection Enable......... : 0 (not enabled)
 - CFG Lock............................. : 0 (MSR not locked)
 - C3 State Auto Demotion............... : 1 (enabled)
 - C1 State Auto Demotion............... : 1 (enabled)
 - C3 State Undemotion.................. : 1 (enabled)
 - C1 State Undemotion.................. : 1 (enabled)
 - Package C-State Auto Demotion........ : 1 (enabled)
 - Package C-State Undemotion........... : 1 (enabled)

MSR_PMG_IO_CAPTURE_BASE...........(0xE4) : 0x51814
------------------------------------------
 - LVL_2 Base Address................... : 0x1814
 - C-state Range........................ : 5 (C-States not included, I/O MWAIT redirection not enabled)

IA32_MPERF........................(0xE7) : 0x1A1BB41C1
IA32_APERF........................(0xE8) : 0x1B512EEF7

MSR_FLEX_RATIO...................(0x194) : 0x11400
------------------------------------------

MSR_IA32_PERF_STATUS.............(0x198) : 0x274B00002400
------------------------------------------
 - Current Performance State Value...... : 0x2400 (3600 MHz)

MSR_IA32_PERF_CONTROL............(0x199) : 0x1100
------------------------------------------
 - Target performance State Value....... : 0x1100 (1700 MHz)
 - Intel Dynamic Acceleration........... : 0 (IDA engaged)

IA32_CLOCK_MODULATION............(0x19A) : 0x0

IA32_THERM_INTERRUPT.............(0x19B) : 0x10
------------------------------------------
 - High-Temperature Interrupt Enable.... : 0 (disabled)
 - Low-Temperature Interrupt Enable..... : 0 (disabled)
 - PROCHOT# Interrupt Enable............ : 0 (disabled)
 - FORCEPR# Interrupt Enable............ : 0 (disabled)
 - Critical Temperature Interrupt Enable : 1 (enabled)
 - Threshold #1 Value................... : 0
 - Threshold #1 Interrupt Enable........ : 0 (disabled)
 - Threshold #2 Value................... : 0
 - Threshold #2 Interrupt Enable........ : 0 (disabled)
 - Power Limit Notification Enable...... : 0 (disabled)

IA32_THERM_STATUS................(0x19C) : 0x882C2800
------------------------------------------
 - Thermal Status....................... : 0
 - Thermal Log.......................... : 0
 - PROCHOT # or FORCEPR# event.......... : 0
 - PROCHOT # or FORCEPR# log............ : 0
 - Critical Temperature Status.......... : 0
 - Critical Temperature log............. : 0
 - Thermal Threshold #1 Status.......... : 0
 - Thermal Threshold #1 log............. : 0
 - Thermal Threshold #2 Status.......... : 0
 - Thermal Threshold #2 log............. : 0
 - Power Limitation Status.............. : 0
 - Power Limitation log................. : 1
 - Current Limit Status................. : 0
 - Current Limit log.................... : 1
 - Cross Domain Limit Status............ : 0
 - Cross Domain Limit log............... : 0
 - Digital Readout...................... : 44
 - Resolution in Degrees Celsius........ : 1
 - Reading Valid........................ : 1 (valid)

MSR_THERM2_CTL...................(0x19D) : 0x0

IA32_MISC_ENABLES................(0x1A0) : 0x850089
------------------------------------------
 - Fast-Strings......................... : 1 (enabled)
 - FOPCODE compatibility mode Enable.... : 0
 - Automatic Thermal Control Circuit.... : 1 (enabled)
 - Split-lock Disable................... : 0
 - Performance Monitoring............... : 1 (available)
 - Bus Lock On Cache Line Splits Disable : 0
 - Hardware prefetch Disable............ : 0
 - Processor Event Based Sampling....... : 0 (PEBS supported)
 - GV1/2 legacy Enable.................. : 0
 - Enhanced Intel SpeedStep Technology.. : 1 (enabled)
 - MONITOR FSM.......................... : 1 (MONITOR/MWAIT supported)
 - Adjacent sector prefetch Disable..... : 0
 - CFG Lock............................. : 0 (MSR not locked)
 - xTPR Message Disable................. : 1 (disabled)

MSR_TEMPERATURE_TARGET...........(0x1A2) : 0xB640000
------------------------------------------
 - Turbo Attenuation Units.............. : 0 
 - Temperature Target................... : 100
 - TCC Activation Offset................ : 11

MSR_MISC_PWR_MGMT................(0x1AA) : 0x401CC1
------------------------------------------
 - EIST Hardware Coordination........... : 1 (hardware coordination disabled)
 - Energy/Performance Bias support...... : 1
 - Energy/Performance Bias.............. : 0 (disabled/MSR not visible to software)
 - Thermal Interrupt Coordination Enable : 1 (thermal interrupt routed to all cores)
 - SpeedShift Technology Enable......... : 1 (enabled)
 - SpeedShift Interrupt Coordination.... : 1 (enabled)
 - SpeedShift Energy Efficient Perf..... : 1 (enabled)
 - SpeedShift Technology Setup for HWP.. : Yes (setup for HWP)

MSR_TURBO_RATIO_LIMIT............(0x1AD) : 0x25252828
------------------------------------------
 - Maximum Ratio Limit for C01.......... : 28 (4000 MHz) 
 - Maximum Ratio Limit for C02.......... : 28 (4000 MHz) 
 - Maximum Ratio Limit for C03.......... : 25 (3700 MHz) 
 - Maximum Ratio Limit for C04.......... : 25 (3700 MHz) 

IA32_ENERGY_PERF_BIAS............(0x1B0) : 0x5
------------------------------------------
 - Power Policy Preference.............. : 5 (balanced performance and energy saving)

MSR_POWER_CTL....................(0x1FC) : 0x4005E
------------------------------------------
 - Bi-Directional Processor Hot......... : 0 (disabled)
 - C1E Enable........................... : 1 (enabled)

MSR_RAPL_POWER_UNIT..............(0x606) : 0xA0E03
------------------------------------------
 - Power Units.......................... : 3 (1/8 Watt)
 - Energy Status Units.................. : 14 (61 micro-Joules)
 - Time Units .......................... : 10 (976.6 micro-Seconds)

MSR_PKG_POWER_LIMIT..............(0x610) : 0x42816000DD8160
------------------------------------------
 - Package Power Limit #1............... : 44 Watt
 - Enable Power Limit #1................ : 1 (enabled)
 - Package Clamping Limitation #1....... : 1 (allow going below OS-requested P/T state during Time Window for Power Limit #1)
 - Time Window for Power Limit #1....... : 110 (163840 milli-Seconds)
 - Package Power Limit #2............... : 44 Watt
 - Enable Power Limit #2................ : 1 (enabled)
 - Package Clamping Limitation #2....... : 0 (disabled)
 - Time Window for Power Limit #2....... : 33 (10 milli-Seconds)
 - Lock................................. : 0 (MSR not locked)

MSR_PKG_ENERGY_STATUS............(0x611) : 0x262E65
------------------------------------------
 - Total Energy Consumed................ : 152 Joules (Watt = Joules / seconds)

MSR_PP0_POWER_LIMIT..............(0x638) : 0x0

MSR_PP0_ENERGY_STATUS............(0x639) : 0x1ADE34
------------------------------------------
 - Total Energy Consumed................ : 107 Joules (Watt = Joules / seconds)

MSR_PP0_POWER_LIMIT..............(0x638) : 0x0

MSR_PP0_ENERGY_STATUS............(0x639) : 0x1AE008
------------------------------------------
 - Total Energy Consumed................ : 107 Joules (Watt = Joules / seconds)

MSR_PP1_POWER_LIMIT..............(0x640) : 0x0

MSR_PP1_ENERGY_STATUS............(0x641) : 0x295B
------------------------------------------
 - Total Energy Consumed................ : 0 Joules (Watt = Joules / seconds)

MSR_PP1_POLICY...................(0x642) : 0x18
------------------------------------------
 - Priority Level....................... : 24

MSR_CONFIG_TDP_NOMINAL...........(0x648) : 0x12
MSR_CONFIG_TDP_LEVEL1............(0x649) : 0x80050
MSR_CONFIG_TDP_LEVEL2............(0x64a) : 0x1400C8
MSR_CONFIG_TDP_CONTROL...........(0x64b) : 0x0
MSR_TURBO_ACTIVATION_RATIO.......(0x64c) : 0x0
MSR_PKGC3_IRTL...................(0x60a) : 0x884E
MSR_PKGC6_IRTL...................(0x60b) : 0x8876
MSR_PKGC7_IRTL...................(0x60c) : 0x8894
MSR_PKG_C2_RESIDENCY.............(0x60d) : 0x40F8F2A24
MSR_PKG_C3_RESIDENCY.............(0x3f8) : 0x0
MSR_PKG_C2_RESIDENCY.............(0x60d) : 0x40F8F2A24
MSR_PKG_C3_RESIDENCY.............(0x3f8) : 0x0
MSR_PKG_C6_RESIDENCY.............(0x3f9) : 0x0
MSR_PKG_C7_RESIDENCY.............(0x3fa) : 0x0
MSR_PKG_C8_RESIDENCY.............(0x630) : 0x0
MSR_PKG_C9_RESIDENCY.............(0x631) : 0x0
MSR_PKG_C10_RESIDENCY............(0x632) : 0x0
MSR_PKG_C8_LATENCY...............(0x633) : 0x0
MSR_PKG_C9_LATENCY...............(0x634) : 0x0
MSR_PKG_C10_LATENCY..............(0x635) : 0x0

MSR_PLATFORM_ENERGY_COUNTER......(0x64D) : 0x0 (not supported by hardware/BIOS)

MSR_PPERF........................(0x64E) : 0x1FA2D5C7D
------------------------------------------
 - Hardware workload scalability........ : 8492244093

MSR_CORE_PERF_LIMIT_REASONS......(0x64F) : 0x3D001000
------------------------------------------
 - PROCHOT Status....................... : 0
 - Thermal Status....................... : 0
 - Residency State Regulation Status.... : 0
 - Running Average Thermal Limit Status. : 0
 - VR Therm Alert Status................ : 0
 - VR Therm Design Current Status....... : 0
 - Other Status......................... : 0
 - Package/Platform-Level #1 Power Limit : 0
 - Package/Platform-Level #2 Power Limit : 0
 - Max Turbo Limit Status............... : 1 (frequency reduced below OS request due to multi-core turbo limits)
 - Turbo Transition Attenuation Status.. : 0
 - PROCHOT Log.......................... : 0
 - Thermal Log.......................... : 0
 - Residency State Regulation Log....... : 0
 - Running Average Thermal Limit Log.... : 0
 - VR Therm Alert Log................... : 0
 - VR Thermal Design Current Log........ : 0
 - Other Status Log..................... : 1 (status bit has asserted)
 - Package/Platform-Level #1 Power Limit : 1 (status bit has asserted)
 - Package/Platform-Level #2 Power Limit : 1 (status bit has asserted)
 - Max Turbo Limit Log.................. : 1 (status bit has asserted)
 - Turbo Transition Attenuation Log..... : 1 (status bit has asserted)
HDC Supported

IA32_PKG_HDC_CTL.................(0xDB0) : 0x0

IA32_PM_CTL1.....................(0xDB1) : 0x1
------------------------------------------
HDC Allow Block..................(0xDB1) : 1 (HDC blocked)

IA32_THREAD_STALL................(0xDB2) : 0x0

MSR_PKG_HDC_CONFIG...............(0x652) : 0x2
------------------------------------------
Pkg Cx Monitor ..................(0x652) : 2 (count package C3 and deeper)
MSR_CORE_HDC_RESIDENCY...........(0x653) : 0x0

MSR_PKG_HDC_SHALLOW_RESIDENCY....(0x655) : 0x0

MSR_PKG_HDC_DEEP_RESIDENCY.......(0x656) : 0x0

IA32_TSC_DEADLINE................(0x6E0) : 0xA0B7474ED
MSR_PPERF........................(0x63E) : 0x1 (19)

IA32_PM_ENABLE...................(0x770) : 0x1 (HWP Supported and Enabled)

IA32_HWP_CAPABILITIES............(0x771) : 0x1081228
------------------------------------------
 - Highest Performance.................. : 40
 - Guaranteed Performance............... : 18
 - Most Efficient Performance........... : 8
 - Lowest Performance................... : 1

IA32_HWP_INTERRUPT...............(0x773) : 0x1
------------------------------------------
 - Guaranteed Performance Change........ : 1 (Interrupt generated on change of)
 - Excursion Minimum.................... : 0 (Interrupt generation disabled)

IA32_HWP_REQUEST.................(0x774) : 0x282804
------------------------------------------
 - Minimum Performance.................. : 4
 - Maximum Performance.................. : 40
 - Desired Performance.................. : 40
 - Energy Efficient Performance......... : 0
 - Activity Window...................... : 0, 0
 - Package Control...................... : 0

IA32_HWP_STATUS..................(0x777) : 0x0
------------------------------------------
 - Guaranteed Performance Change........ : 0 (has not occured)
 - Excursion To Minimum................. : 0 (has not occured)

CPU Ratio Info:
------------------------------------------
Base Clock Frequency (BLCK)............. : 100 MHz
Maximum Efficiency Ratio/Frequency...... :  4 ( 400 MHz)
Maximum non-Turbo Ratio/Frequency....... : 20 (2000 MHz)
Maximum Turbo Ratio/Frequency........... : 40 (4000 MHz)

IGPU Info:
------------------------------------------
IGPU Current Frequency.................. :    0 MHz
IGPU Minimum Frequency.................. :  300 MHz
IGPU Maximum Non-Turbo Frequency........ :  300 MHz
IGPU Maximum Turbo Frequency............ : 1150 MHz
IGPU Maximum limit...................... : No Limit

P-State ratio * 100 = Frequency in MHz
------------------------------------------
CPU P-States [ (8) 18 37 ] iGPU P-States [ ]
CPU C3-Cores [ 1 6 7 ]
CPU C6-Cores [ 0 1 3 4 5 ]
CPU P-States [ (8) 18 37 ] iGPU P-States [ ]
CPU C3-Cores [ 0 1 5 6 7 ]
CPU C6-Cores [ 0 1 2 3 4 5 ]
CPU C6-Cores [ 0 1 2 3 4 5 6 7 ]
CPU P-States [ (8) 18 21 37 ] iGPU P-States [ ]
CPU C3-Cores [ 0 1 3 4 5 6 7 ]
CPU C3-Cores [ 0 1 2 3 4 5 6 7 ]
CPU P-States [ 8 18 21 (36) 37 ] iGPU P-States [ ]
CPU P-States [ (8) 18 19 21 36 37 ] iGPU P-States [ ]
CPU P-States [ 8 18 19 21 (29) 36 37 ] iGPU P-States [ ]
CPU P-States [ (8) 18 19 20 21 29 36 37 ] iGPU P-States [ ]
CPU P-States [ (8) 18 19 20 21 29 36 37 ] iGPU P-States [ ]
CPU P-States [ (8) 18 19 20 21 29 36 37 ] iGPU P-States [ ]
CPU P-States [ 8 18 19 20 21 29 36 37 (40) ] iGPU P-States [ ]
CPU P-States [ 8 18 19 20 21 22 29 36 (37) 40 ] iGPU P-States [ ]
CPU P-States [ 8 (9) 18 19 20 21 22 29 36 37 40 ] iGPU P-States [ ]
CPU P-States [ 8 9 18 19 20 21 22 29 36 (37) 40 ] iGPU P-States [ ]
```

## 本机华硕笔记本BIOS推荐设置

注意，Override为手动设置的意思。

| 选项                                                                            | 本机BIOS地址 | 设定值  | 设置                    | 说明                                                                                                                                                                                                        |
|-------------------------------------------------------------------------------|----------|------|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Intel RC ACPI Settings**                                                    |          |      |                       |                                                                                                                                                                                                           |
| PTID Support                                                                  |          |      |                       |                                                                                                                                                                                                           |
| PECI Access Method                                                            |          |      |                       |                                                                                                                                                                                                           |
| Native PCIE Enable                                                            |          |      |                       |                                                                                                                                                                                                           |
| Native ASPM                                                                   |          |      |                       | 启用Windows Vista操作系统控制设备的ASPM支持                                                                                                                                                                            |
| Wake system from S5                                                           |          |      |                       |                                                                                                                                                                                                           |
| Wake up hour                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Wake up minute                                                                |          |      |                       |                                                                                                                                                                                                           |
| Wake up second                                                                |          |      |                       |                                                                                                                                                                                                           |
| ACPI Debug                                                                    |          |      |                       |                                                                                                                                                                                                           |
| Low Power S0 Idle Capability                                                  |          |      |                       |                                                                                                                                                                                                           |
| 10sec Power Button OVR                                                        |          |      |                       |                                                                                                                                                                                                           |
| EC Notification                                                               |          |      |                       |                                                                                                                                                                                                           |
| EC CS Debug Light                                                             |          |      |                       | Caps Lock指示灯                                                                                                                                                                                              |
| EC Low Power Mode                                                             |          |      |                       |                                                                                                                                                                                                           |
| EC Debug LED                                                                  |          |      |                       |                                                                                                                                                                                                           |
| EC Base CS Pwr Policy                                                         |          |      |                       |                                                                                                                                                                                                           |
| Sensor Standby                                                                |          |      |                       |                                                                                                                                                                                                           |
| CS PL1 Limit                                                                  |          |      |                       |                                                                                                                                                                                                           |
| CS PL1 Value                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Lpit Recidency Counter                                                        |          |      |                       |                                                                                                                                                                                                           |
| Intel Ready Mode Technology                                                   |          |      |                       |                                                                                                                                                                                                           |
| HW Notification                                                               |          |      |                       |                                                                                                                                                                                                           |
| Intel RMT State                                                               |          |      |                       |                                                                                                                                                                                                           |
| PCI Delay Optimization                                                        |          |      |                       |                                                                                                                                                                                                           |
| Type C Support                                                                |          |      |                       |                                                                                                                                                                                                           |
| **PEP Constraints Configuration**                                             |          |      |                       |                                                                                                                                                                                                           |
| PEP CPU                                                                       |          |      |                       |                                                                                                                                                                                                           |
| PEP Graphics                                                                  |          |      |                       |                                                                                                                                                                                                           |
| PEP ISP                                                                       |          |      |                       |                                                                                                                                                                                                           |
| PEP SATA Controller                                                           |          |      |                       |                                                                                                                                                                                                           |
| PEP RAID VOL0                                                                 |          |      |                       |                                                                                                                                                                                                           |
| PEP SATA PORT0                                                                |          |      |                       |                                                                                                                                                                                                           |
| PEP SATA PORT1                                                                |          |      |                       |                                                                                                                                                                                                           |
| PEP SATA PORT2                                                                |          |      |                       |                                                                                                                                                                                                           |
| PEP SATA PORT3                                                                |          |      |                       |                                                                                                                                                                                                           |
| PEP SATA PORT4                                                                |          |      |                       |                                                                                                                                                                                                           |
| PEP SATA PORT5                                                                |          |      |                       |                                                                                                                                                                                                           |
| PEP SATA NVM1                                                                 |          |      |                       |                                                                                                                                                                                                           |
| PEP SATA NVM2                                                                 |          |      |                       |                                                                                                                                                                                                           |
| PEP SATA NVM3                                                                 |          |      |                       |                                                                                                                                                                                                           |
| PEP UART                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PEP I2C0                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PEP I2C1                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PEP I2C2                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PEP I2C3                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PEP I2C4                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PEP I2C5                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PEP SPI                                                                       |          |      |                       |                                                                                                                                                                                                           |
| PEP XHCI                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PEP Audio                                                                     |          |      |                       |                                                                                                                                                                                                           |
| PEP EMMC                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PEP SDXC                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PEP WIGIG F1                                                                  |          |      |                       |                                                                                                                                                                                                           |
| **CPU Configuration**                                                         |          |      |                       |                                                                                                                                                                                                           |
| SW Guard Extensions (SGX)                                                     |          |      |                       |                                                                                                                                                                                                           |
| Select Owner EPOCH input type                                                 |          |      |                       |                                                                                                                                                                                                           |
| Software Guard Extensions Epoch 0                                             |          |      |                       |                                                                                                                                                                                                           |
| Software Guard Extensions Epoch 1                                             |          |      |                       |                                                                                                                                                                                                           |
| PRMRR Size                                                                    |          |      |                       |                                                                                                                                                                                                           |
| CPU Flex Ratio Override                                                       | 0x4EE    | 0x1  | Enabled               | CPU倍频手动设置                                                                                                                                                                                                 |
| CPU Flex Ratio Settings                                                       | 0x4EC    | 0x3F | Maximum               | CPU倍频手动设置                                                                                                                                                                                                 |
| Hardware Prefetcher                                                           | 0x5BE    | 0x1  | Enabled               | 让CPU在L2 Cache进行预取反馈和数据                                                                                                                                                                                    |
| Adjacent Cache Line Prefetch                                                  | 0x5BF    | 0x1  | Enabled               | 让L2 Cache的中间缓存线运行相邻缓存线预取功能                                                                                                                                                                                |
| Intel (VMX) Virtualization Technology                                         | 0x5B8    | 0x1  | Enabled               | Intel硬件辅助虚拟化技术                                                                                                                                                                                            |
| PECI                                                                          |          |      |                       |                                                                                                                                                                                                           |
| Active Processor Cores                                                        | 0x4F1    | 0x0  | All                   | 激活处理器的硬件核心数                                                                                                                                                                                               |
| Execute Disable Bit                                                           |          |      |                       |                                                                                                                                                                                                           |
| Limit CPUID Maximum                                                           | 0x4EA    | 0x0  | Disabled              |                                                                                                                                                                                                           |
| Hyper-Threading                                                               | 0x4F0    | 0x1  | Enabled               |                                                                                                                                                                                                           |
| BIST                                                                          |          |      |                       |                                                                                                                                                                                                           |
| JTAG C10 Power                                                                |          |      |                       |                                                                                                                                                                                                           |
| AP threads Idle Manner                                                        |          |      |                       |                                                                                                                                                                                                           |
| AP threads Handoff Manner                                                     |          |      |                       |                                                                                                                                                                                                           |
| AES                                                                           |          |      |                       |                                                                                                                                                                                                           |
| MachineCheck                                                                  |          |      |                       |                                                                                                                                                                                                           |
| MonitorMWait                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Intel Trusted Execution Technology                                            |          |      |                       |                                                                                                                                                                                                           |
| Alias Check Request                                                           |          |      |                       |                                                                                                                                                                                                           |
| DPR Memory Size (MB)                                                          |          |      |                       |                                                                                                                                                                                                           |
| Reset AUX Content                                                             |          |      |                       |                                                                                                                                                                                                           |
| Flash Wear Out Protection                                                     |          |      |                       |                                                                                                                                                                                                           |
| Current Debug Interface Status                                                |          |      |                       |                                                                                                                                                                                                           |
| Debug Interface                                                               |          |      |                       |                                                                                                                                                                                                           |
| Direct Connect Interface                                                      |          |      |                       |                                                                                                                                                                                                           |
| Debug Interface Lock                                                          |          |      |                       |                                                                                                                                                                                                           |
| Processor trace memory allocation                                             |          |      |                       |                                                                                                                                                                                                           |
| Processor trace                                                               |          |      |                       |                                                                                                                                                                                                           |
| SMM Processor Trace                                                           |          |      |                       |                                                                                                                                                                                                           |
| FCLK Frequency for Early Power On                                             | 0x5F9    | 0x1  | 1Ghz                  | 减少PCIE的延迟，稍微提升PCIE设备性能                                                                                                                                                                                    |
| Voltage Optimization                                                          |          |      |                       |                                                                                                                                                                                                           |
| **CPU SMM Enhancement**                                                       |          |      |                       |                                                                                                                                                                                                           |
| SMM MSR Save State Enable                                                     |          |      |                       |                                                                                                                                                                                                           |
| SMM Code Access Check                                                         |          |      |                       |                                                                                                                                                                                                           |
| SMM Use Delay Indication                                                      |          |      |                       |                                                                                                                                                                                                           |
| SMM Use Block Indication                                                      |          |      |                       |                                                                                                                                                                                                           |
| SMM Use SMM en-US Indication                                                  |          |      |                       |                                                                                                                                                                                                           |
| **CPU - Power Management Control**                                            |          |      |                       |                                                                                                                                                                                                           |
| Boot performance mode                                                         | 0x4F7    | 0x2  | Turbo Performance     | 设置启动模式                                                                                                                                                                                                    |
| Intel SpeedStep                                                               | 0x4F4    | 0x1  | Enabled               |                                                                                                                                                                                                           |
| Race To Halt (RTH)                                                            |          |      |                       |                                                                                                                                                                                                           |
| Intel Speed Shift Technology                                                  | 0x4F6    | 0x1  | Enabled               | Intel CPU 节电技术、状态调整，大大提高响应速度                                                                                                                                                                              |
| HDC Control                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Turbo Mode                                                                    | 0x4FA    | 0x1  | Enabled               | 睿频模式                                                                                                                                                                                                      |
| Platform PL1 Enable                                                           | 0x515    | 0x0  | Disabled              | Platform 1功耗限制                                                                                                                                                                                            |
| Platform PL1 Power                                                            |          |      |                       |                                                                                                                                                                                                           |
| Power Limit 1 Time Window                                                     | 0x500    | 0x80 | Maximum               | 超频最长时间                                                                                                                                                                                                    |
| Platform PL2 Enable                                                           | 0x51B    | 0x0  | Disabled              | Platform 2功耗限制                                                                                                                                                                                            |
| Platform PL2 Power                                                            |          |      |                       |                                                                                                                                                                                                           |
| Power Limit 4 Override                                                        | 0x50E    | 0x1  | Enabled               | 无视超频功率4限制                                                                                                                                                                                                 |
| Power Limit 4                                                                 |          |      | Maximum               | Platform 4功耗限制值                                                                                                                                                                                           |
| Power Limit 4 Lock                                                            | 0x513    | 0x0  | Disabled              | Platform 4功耗限制锁                                                                                                                                                                                           |
| C states                                                                      |          |      |                       | CPU核心睡眠深度（CPU全速运行时候会工作在C0，任务量较时会工作在C1，CPU核心休眠的时候会工作在C7，当CPU核心从睡眠状态唤醒需要比较高的延迟，通常比内存延迟高得多）                                                                                                                  |
| Enhanced C-states                                                             |          |      |                       | 让处理器在闲置时降低电力消耗                                                                                                                                                                                            |
| C-State Auto Demotion                                                         |          |      |                       |                                                                                                                                                                                                           |
| C-State Un-demotion                                                           |          |      |                       |                                                                                                                                                                                                           |
| Package C-State Demotion                                                      |          |      |                       |                                                                                                                                                                                                           |
| Package C-State Un-demotion                                                   |          |      |                       |                                                                                                                                                                                                           |
| CState Pre-Wake                                                               |          |      |                       |                                                                                                                                                                                                           |
| IO MWAIT Redirection                                                          |          |      |                       |                                                                                                                                                                                                           |
| Package C State Limit                                                         |          |      |                       | 设置CPU Package的C-State支持                                                                                                                                                                                   |
| **View/Configure Turbo Options**                                              |          |      |                       |                                                                                                                                                                                                           |
| Energy Efficient P-state                                                      |          |      |                       |                                                                                                                                                                                                           |
| Package Power Limit MSR Lock                                                  | 0x514    | 0x0  | Disabled              | MSR锁                                                                                                                                                                                                      |
| Power Limit 1 Override                                                        | 0x4FF    | 0x1  | Enabled               | 无视超频功率1限制                                                                                                                                                                                                 |
| Power Limit 1                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Power Limit 1 Time Window                                                     | 0x54A    | 0x80 | Maximum               | 超频最长时间                                                                                                                                                                                                    |
| Power Limit 2 Override                                                        | 0x501    | 0x1  | Enabled               | 无视超频功率2限制                                                                                                                                                                                                 |
| Power Limit 2                                                                 |          |      |                       |                                                                                                                                                                                                           |
| 1-Core Ratio Limit Override                                                   | 0x5D1    | 0x53 | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                |
| 2-Core Ratio Limit Override                                                   | 0x5D2    | 0x53 | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                |
| 3-Core Ratio Limit Override                                                   | 0x5D3    | 0x53 | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                |
| 4-Core Ratio Limit Override                                                   | 0x5D4    | 0x53 | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                |
| 5-Core Ratio Limit Override                                                   | 0x6D3    | 0x53 | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                |
| 6-Core Ratio Limit Override                                                   | 0x6D4    | 0x53 | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                |
| 7-Core Ratio Limit Override                                                   | 0x6D5    | 0x53 | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                |
| 8-Core Ratio Limit Override                                                   | 0x6D6    | 0x53 | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                |
| Energy Efficient Turbo                                                        |          |      |                       |                                                                                                                                                                                                           |
| **CPU VR Settings**                                                           |          |      |                       |                                                                                                                                                                                                           |
| PSYS Slope                                                                    |          |      |                       |                                                                                                                                                                                                           |
| PSYS Offset                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PSYS PMax Power                                                               |          |      |                       |                                                                                                                                                                                                           |
| VR Mailbox Command options                                                    |          |      |                       |                                                                                                                                                                                                           |
| **Acoustic Noise Settings**                                                   |          |      |                       |                                                                                                                                                                                                           |
| Acoustic Noise Mitigation                                                     |          |      |                       |                                                                                                                                                                                                           |
| Disable Fast PKG C State Ramp for IA Domain                                   |          |      |                       |                                                                                                                                                                                                           |
| Slow Slew Rate for IA Domain                                                  |          |      |                       |                                                                                                                                                                                                           |
| Disable Fast PKG C State Ramp for GT Domain                                   |          |      |                       |                                                                                                                                                                                                           |
| Slow Slew Rate for GT Domain                                                  |          |      |                       |                                                                                                                                                                                                           |
| Disable Fast PKG C State Ramp for SA Domain                                   |          |      |                       |                                                                                                                                                                                                           |
| Slow Slew Rate for SA Domain                                                  |          |      |                       |                                                                                                                                                                                                           |
| **System Agent VR Settings**                                                  |          |      |                       |                                                                                                                                                                                                           |
| **Core/IA VR Settings**                                                       |          |      |                       |                                                                                                                                                                                                           |
| **GT-UnSliced VR Settings**                                                   |          |      |                       |                                                                                                                                                                                                           |
| **GT-Sliced VR Settings**                                                     |          |      |                       |                                                                                                                                                                                                           |
| VR Config Enable                                                              |          |      | Enabled               |                                                                                                                                                                                                           |
| AC Loadline                                                                   |          |      |                       | 设置AC负载线，以1/10mOhms定义                                                                                                                                                                                      |
| DC Loadline                                                                   |          |      |                       | 设置DC负载线，以1/10mOhms定义                                                                                                                                                                                      |
| PS Current Threshold1                                                         |          |      | 120                   |                                                                                                                                                                                                           |
| PS Current Threshold2                                                         |          |      | 40                    |                                                                                                                                                                                                           |
| PS Current Threshold3                                                         |          |      | 0                     |                                                                                                                                                                                                           |
| PS3 Enable                                                                    |          |      |                       |                                                                                                                                                                                                           |
| PS4 Enable                                                                    |          |      |                       |                                                                                                                                                                                                           |
| IMON Slope                                                                    |          |      |                       | 倍率（CPU真实功率乘以倍率后报给EC控制，例如输入80，则20W限制的CPU会被报道目前只有16W，EC会继续控制它达到25W，这样EC认为它目前跑在20W上，实际上已经跑到25W了）                                                                                                             |
| IMON Offset                                                                   |          |      |                       | 调整会影响电池模式消耗                                                                                                                                                                                               |
| IMON Prefix                                                                   |          |      |                       |                                                                                                                                                                                                           |
| VR Current Limit                                                              |          |      | 400                   |                                                                                                                                                                                                           |
| VR Voltage Limit                                                              |          |      | 1100                  |                                                                                                                                                                                                           |
| TDC Enable                                                                    |          |      |                       |                                                                                                                                                                                                           |
| TDC Current Limit                                                             |          |      |                       |                                                                                                                                                                                                           |
| TDC Time Window                                                               |          |      |                       |                                                                                                                                                                                                           |
| TDC Lock                                                                      |          |      |                       |                                                                                                                                                                                                           |
| **Power Limit 3 Settings**                                                    |          |      |                       |                                                                                                                                                                                                           |
| Power Limit 3 Override                                                        | 0x506    | 0x1  | Enabled               | 无视超频功率3限制                                                                                                                                                                                                 |
| Power Limit 3                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Power Limit 3 Time Window                                                     | 0x50B    | 0x40 | Maximum               | 超频最长时间                                                                                                                                                                                                    |
| Powet Limit 3 Duty Cycle                                                      |          |      |                       |                                                                                                                                                                                                           |
| Power Limit 3 Lock                                                            | 0x50D    | 0x0  | Disabled              | Platform 3功耗限制锁                                                                                                                                                                                           |
| **Config TDP Configurations**                                                 |          |      |                       |                                                                                                                                                                                                           |
| Configurable TDP Boot Mode                                                    | 0x528    | 0x2  | Up                    |                                                                                                                                                                                                           |
| Configurable TDP Lock                                                         | 0x529    | 0x0  |                       | TDP锁                                                                                                                                                                                                      |
| CTDP BIOS control                                                             |          |      |                       |                                                                                                                                                                                                           |
| **Custom Settings Nominal**                                                   |          |      |                       |                                                                                                                                                                                                           |
| **Custom Settings Down**                                                      |          |      |                       |                                                                                                                                                                                                           |
| Power Limit 1                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Power Limit 2                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Power Limit 1 Time Window                                                     |          |      |                       |                                                                                                                                                                                                           |
| ConfigTDP Turbo Activation Ratio                                              |          |      |                       |                                                                                                                                                                                                           |
| **View/Configure CPU Lock Options**                                           |          |      |                       |                                                                                                                                                                                                           |
| CFG Lock                                                                      |          |      |                       | 关闭或开启MSR 0xe2寄存器，CPU的P-State/C-State电源管理是放在MSR寄存器里的（用黑苹果时候须解锁）                                                                                                                                            |
| Overclocking Lock                                                             | 0x5D6    | 0x0  | Disabled              | 超频锁                                                                                                                                                                                                       |
| **GT - Power Management Control**                                             |          |      |                       |                                                                                                                                                                                                           |
| RC6(Render Standby)                                                           | 0x740    | 0x1  | Enabled               | 集成显卡待机                                                                                                                                                                                                    |
| Maximum GT frequency                                                          | 0x752    | 0xFF | Default Max Frequency | 显卡最大频率                                                                                                                                                                                                    |
| **PCH-FW Configuration**                                                      |          |      |                       |                                                                                                                                                                                                           |
| ME State                                                                      |          |      |                       |                                                                                                                                                                                                           |
| Manageability Features State                                                  |          |      |                       |                                                                                                                                                                                                           |
| AMT BIOS Features                                                             | 0x6F1    | 0x1  | Enabled               |                                                                                                                                                                                                           |
| ME Unconfig on RTC Clear                                                      |          |      |                       |                                                                                                                                                                                                           |
| NFC Feature State                                                             |          |      |                       |                                                                                                                                                                                                           |
| Comms Hub Support                                                             |          |      |                       |                                                                                                                                                                                                           |
| JHI Support                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Core Bios Done Message                                                        |          |      |                       |                                                                                                                                                                                                           |
| **PTT Configuration**                                                         |          |      |                       |                                                                                                                                                                                                           |
| TPM Device Selection                                                          |          |      |                       |                                                                                                                                                                                                           |
| PTP aware OS                                                                  |          |      |                       |                                                                                                                                                                                                           |
| TPM 1.2 Deactivate                                                            |          |      |                       |                                                                                                                                                                                                           |
| **Firmware Update Configuration**                                             |          |      |                       |                                                                                                                                                                                                           |
| Me FW Image Re-Flash                                                          |          |      |                       |                                                                                                                                                                                                           |
|                                                                               |          |      |                       |                                                                                                                                                                                                           |
| **ME Debug Configuration**                                                    |          |      |                       |                                                                                                                                                                                                           |
| HECI Timeouts                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Force ME DID Init Status                                                      |          |      |                       |                                                                                                                                                                                                           |
| CPU Replaced Polling Disable                                                  |          |      |                       |                                                                                                                                                                                                           |
| ME DID Message                                                                |          |      |                       |                                                                                                                                                                                                           |
| HECI Retry Disable                                                            |          |      |                       |                                                                                                                                                                                                           |
| HECI Message check Disable                                                    |          |      |                       |                                                                                                                                                                                                           |
| MBP HOB Skip                                                                  |          |      |                       |                                                                                                                                                                                                           |
| KT Device                                                                     |          |      |                       |                                                                                                                                                                                                           |
| IDER Device                                                                   |          |      |                       |                                                                                                                                                                                                           |
| End Of Post Message                                                           |          |      |                       |                                                                                                                                                                                                           |
| D0I3 Setting for HECI Disable                                                 |          |      |                       |                                                                                                                                                                                                           |
| **AMT Configuration**                                                         |          |      |                       |                                                                                                                                                                                                           |
| ASF support                                                                   |          |      |                       |                                                                                                                                                                                                           |
| USB Provisioning of AMT                                                       |          |      |                       |                                                                                                                                                                                                           |
| **CIRA Configuration**                                                        |          |      |                       |                                                                                                                                                                                                           |
| CIRA Timeout                                                                  |          |      |                       |                                                                                                                                                                                                           |
| **ASF Configuration**                                                         |          |      |                       |                                                                                                                                                                                                           |
| PET Progress                                                                  |          |      |                       |                                                                                                                                                                                                           |
| WatchDog                                                                      |          |      |                       |                                                                                                                                                                                                           |
| OS Timer                                                                      |          |      |                       |                                                                                                                                                                                                           |
| BIOS Timer                                                                    |          |      |                       |                                                                                                                                                                                                           |
| **Secure Erase Configuration**                                                |          |      |                       |                                                                                                                                                                                                           |
| Secure Erase mode                                                             |          |      |                       |                                                                                                                                                                                                           |
| Force Secure Erase                                                            |          |      |                       |                                                                                                                                                                                                           |
| **MEBx Resolution Settings**                                                  |          |      |                       |                                                                                                                                                                                                           |
| Non-UI Mode Resolution                                                        |          |      |                       |                                                                                                                                                                                                           |
| UI Mode Resolution                                                            |          |      |                       |                                                                                                                                                                                                           |
| Graphics Mode Resolution                                                      |          |      |                       |                                                                                                                                                                                                           |
| **Cpu Thermal Configuration**                                                 |          |      |                       |                                                                                                                                                                                                           |
| DTS SMM                                                                       |          |      |                       |                                                                                                                                                                                                           |
| Tcc Offset Clamp Enable                                                       |          |      |                       |                                                                                                                                                                                                           |
| Tcc Offset Lock Enable                                                        |          |      |                       | 开启温度墙调整锁                                                                                                                                                                                                  |
| Bi-directional PROCHOT#                                                       | 0x561    | 0x0  | Disabled              | 温度高时强制降频                                                                                                                                                                                                  |
| Disable PROCHOT# Output                                                       |          |      |                       |                                                                                                                                                                                                           |
| Disable VR Thermal Alert                                                      |          |      |                       |                                                                                                                                                                                                           |
| PROCHOT Response                                                              |          |      |                       |                                                                                                                                                                                                           |
| PROCHOT Lock                                                                  |          |      |                       |                                                                                                                                                                                                           |
| PECI Reset                                                                    |          |      |                       |                                                                                                                                                                                                           |
| PECI C10 Reset                                                                |          |      |                       |                                                                                                                                                                                                           |
| **Platform Thermal Configuration**                                            |          |      |                       |                                                                                                                                                                                                           |
| Automatic Thermal Reporting                                                   |          |      |                       |                                                                                                                                                                                                           |
| Critical Trip Point                                                           | 0x2B2    | 0x7F | 127 C                 | 系统关闭的温度                                                                                                                                                                                                   |
| Active Trip Point 0                                                           |          |      |                       |                                                                                                                                                                                                           |
| Active Trip Point 0 Fan Speed                                                 |          |      |                       |                                                                                                                                                                                                           |
| Active Trip Point 1                                                           |          |      |                       |                                                                                                                                                                                                           |
| Active Trip Point 1 Fan Speed                                                 |          |      |                       |                                                                                                                                                                                                           |
| Passive Trip Point                                                            | 0x2B1    | 0x77 | 119 C (POR)           | CPU被动降温点                                                                                                                                                                                                  |
| Passive TC1 Value                                                             |          |      |                       |                                                                                                                                                                                                           |
| Passive TC2 Value                                                             |          |      |                       |                                                                                                                                                                                                           |
| Passive TSP Value                                                             |          |      |                       |                                                                                                                                                                                                           |
| Active Trip Points                                                            |          |      |                       |                                                                                                                                                                                                           |
| Passive Trip Points                                                           | 0x2B7    | 0x1  | Enabled               | 打开Passive Trip                                                                                                                                                                                            |
| Critical Trip Points                                                          | 0x2B8    | 0x1  | Enabled               |                                                                                                                                                                                                           |
| PCH Thermal Device                                                            |          |      |                       |                                                                                                                                                                                                           |
| PCH Temp Read                                                                 |          |      |                       |                                                                                                                                                                                                           |
| CPU Energy Read                                                               |          |      |                       |                                                                                                                                                                                                           |
| CPU Temp Read                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Alert Enable Lock                                                             |          |      |                       |                                                                                                                                                                                                           |
| **DPTF Configuration**                                                        |          |      |                       |                                                                                                                                                                                                           |
| DPTF                                                                          | 0x2C5    | 0x0  | Disabled              |                                                                                                                                                                                                           |
| DPTF Configuration                                                            |          |      |                       |                                                                                                                                                                                                           |
| Processor Thermal Device                                                      |          |      |                       |                                                                                                                                                                                                           |
| Active Thermal Trip Point                                                     |          |      |                       |                                                                                                                                                                                                           |
| Passive Thermal Trip Point                                                    |          |      |                       |                                                                                                                                                                                                           |
| Critical Thermal Trip Point                                                   |          |      |                       |                                                                                                                                                                                                           |
| S3/CS Thermal Trip Point                                                      |          |      |                       |                                                                                                                                                                                                           |
| Hot Thermal Trip Point                                                        |          |      |                       |                                                                                                                                                                                                           |
| Thermal Sampling Period                                                       |          |      |                       |                                                                                                                                                                                                           |
| PPCC Step Size                                                                |          |      |                       |                                                                                                                                                                                                           |
| Minimum Power Limit 0                                                         |          |      |                       |                                                                                                                                                                                                           |
| Minimum Power Limit 1                                                         |          |      |                       |                                                                                                                                                                                                           |
| Minimum Power Limit 2                                                         |          |      |                       |                                                                                                                                                                                                           |
| CLPO Enable                                                                   |          |      |                       |                                                                                                                                                                                                           |
| CLPO Start PState                                                             |          |      |                       |                                                                                                                                                                                                           |
| CLPO Step Size                                                                |          |      |                       |                                                                                                                                                                                                           |
| CLPO Power Control                                                            |          |      |                       |                                                                                                                                                                                                           |
| CLPO Performance Control                                                      |          |      |                       |                                                                                                                                                                                                           |
| ConfigTDP                                                                     |          |      |                       |                                                                                                                                                                                                           |
| LPM                                                                           |          |      |                       | 支持 LPM (link power management) 以获得更好的省电效果                                                                                                                                                                 |
| **Policy Configuration**                                                      |          |      |                       |                                                                                                                                                                                                           |
| Active Policy                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Passive Policy                                                                |          |      |                       |                                                                                                                                                                                                           |
| TRT Revision                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Critical Policy                                                               |          |      |                       |                                                                                                                                                                                                           |
| Cooling Mode Policy                                                           |          |      |                       |                                                                                                                                                                                                           |
| Platform Noise Mitigation                                                     |          |      |                       |                                                                                                                                                                                                           |
| Power Boss                                                                    |          |      |                       |                                                                                                                                                                                                           |
| Hardware Duty Cycling                                                         |          |      |                       |                                                                                                                                                                                                           |
| Adaptive Performance                                                          |          |      |                       |                                                                                                                                                                                                           |
| Virtual Sensor                                                                |          |      |                       |                                                                                                                                                                                                           |
| PID Policy                                                                    |          |      |                       |                                                                                                                                                                                                           |
| Design Variable 0                                                             |          |      |                       |                                                                                                                                                                                                           |
| Design Variable 1                                                             |          |      |                       |                                                                                                                                                                                                           |
| Design Variable 2                                                             |          |      |                       |                                                                                                                                                                                                           |
| Design Variable 3                                                             |          |      |                       |                                                                                                                                                                                                           |
| Design Variable 0                                                             |          |      |                       |                                                                                                                                                                                                           |
| Design Variable 5                                                             |          |      |                       |                                                                                                                                                                                                           |
| PPCC Object                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PDRT Object                                                                   |          |      |                       |                                                                                                                                                                                                           |
| ARTG Object                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PMAX Object                                                                   |          |      |                       |                                                                                                                                                                                                           |
| _TMP 1 Object                                                                 |          |      |                       |                                                                                                                                                                                                           |
| _TMP 2 Object                                                                 |          |      |                       |                                                                                                                                                                                                           |
| _TMP 3 Object                                                                 |          |      |                       |                                                                                                                                                                                                           |
| _TMP 4 Object                                                                 |          |      |                       |                                                                                                                                                                                                           |
| _TMP 5 Object                                                                 |          |      |                       |                                                                                                                                                                                                           |
| _TMP 6 Object                                                                 |          |      |                       |                                                                                                                                                                                                           |
| _TMP 7 Object                                                                 |          |      |                       |                                                                                                                                                                                                           |
| _TMP 8 Object                                                                 |          |      |                       |                                                                                                                                                                                                           |
| **Platform Settings**                                                         |          |      |                       |                                                                                                                                                                                                           |
| Firmware Configuration                                                        |          |      |                       |                                                                                                                                                                                                           |
| PS2 Keyboard and Mouse                                                        |          |      |                       |                                                                                                                                                                                                           |
| Pmic Vcc IO Level                                                             |          |      |                       |                                                                                                                                                                                                           |
| Pmic Vddq Level                                                               |          |      |                       |                                                                                                                                                                                                           |
| HEBC value                                                                    |          |      |                       |                                                                                                                                                                                                           |
| SLP_S0# Voltage Margining                                                     |          |      |                       |                                                                                                                                                                                                           |
| Audio Connector                                                               |          |      |                       |                                                                                                                                                                                                           |
| Enable EInk DFU device                                                        |          |      |                       |                                                                                                                                                                                                           |
| Power Sharing Manager                                                         |          |      |                       |                                                                                                                                                                                                           |
| Domain Type SPLC 1                                                            |          |      |                       |                                                                                                                                                                                                           |
| Default Power Limit 1 SPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Default Time Window 1 SPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Domain Type SPLC 2                                                            |          |      |                       |                                                                                                                                                                                                           |
| Default Power Limit 2 SPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Default Time Window 2 SPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Domain Type DPLC 1                                                            |          |      |                       |                                                                                                                                                                                                           |
| Domain Preference DPLC 1                                                      |          |      |                       |                                                                                                                                                                                                           |
| Power Limit Index 1 DPLC                                                      |          |      |                       |                                                                                                                                                                                                           |
| Default Power Limit 1 DPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Default Time Window 1 DPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Minimum Power Limit 1 DPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Maximum Power Limit 1 DPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Maximum Time Window 1 DPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Domain Type DPLC 2                                                            |          |      |                       |                                                                                                                                                                                                           |
| Domain Preference DPLC 2                                                      |          |      |                       |                                                                                                                                                                                                           |
| Power Limit Index 2 DPLC                                                      |          |      |                       |                                                                                                                                                                                                           |
| Default Power Limit 2 DPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Default Time Window 2 DPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Minimum Power Limit 2 DPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Maximum Power Limit 2 DPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Default Time Window 2 SPLC                                                    |          |      |                       |                                                                                                                                                                                                           |
| Wireless device                                                               |          |      |                       |                                                                                                                                                                                                           |
| Domain Type SPLC 1                                                            |          |      |                       |                                                                                                                                                                                                           |
| Default Power Limit                                                           |          |      |                       |                                                                                                                                                                                                           |
| Default Time Window                                                           |          |      |                       |                                                                                                                                                                                                           |
| Domain Type SPLC 2                                                            |          |      |                       |                                                                                                                                                                                                           |
| Default Power Limit                                                           |          |      |                       |                                                                                                                                                                                                           |
| Default Time Window                                                           |          |      |                       |                                                                                                                                                                                                           |
| Domain Type SPLC 3                                                            |          |      |                       |                                                                                                                                                                                                           |
| Default Power Limit                                                           |          |      |                       |                                                                                                                                                                                                           |
| Default Time Window                                                           |          |      |                       |                                                                                                                                                                                                           |
| TRxDelay_A                                                                    |          |      |                       |                                                                                                                                                                                                           |
| TRxCableLength_A                                                              |          |      |                       |                                                                                                                                                                                                           |
| TRxDelay_B                                                                    |          |      |                       |                                                                                                                                                                                                           |
| TRxCableLength_B                                                              |          |      |                       |                                                                                                                                                                                                           |
| Domain Type                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Country Identifier                                                            |          |      |                       |                                                                                                                                                                                                           |
| **Realsense 3D Camera Settings**                                              |          |      |                       |                                                                                                                                                                                                           |
| RTD3 Camera Support                                                           |          |      |                       |                                                                                                                                                                                                           |
| Select Camera                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Delay needed for Ivcam power on                                               |          |      |                       |                                                                                                                                                                                                           |
| Delay needed for Ivcam power off                                              |          |      |                       |                                                                                                                                                                                                           |
| Rotation                                                                      |          |      |                       |                                                                                                                                                                                                           |
| DFU support                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Wake support                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Delay needed for DS camera power on                                           |          |      |                       |                                                                                                                                                                                                           |
| Delay needed for DS camera power off                                          |          |      |                       |                                                                                                                                                                                                           |
| Rotation                                                                      |          |      |                       |                                                                                                                                                                                                           |
| **OverClocking Performance Menu**                                             |          |      |                       |                                                                                                                                                                                                           |
| OverClocking Feature                                                          | 0x696    | 0x1  | Enabled               | 超频特性                                                                                                                                                                                                      |
| WDT Enable                                                                    |          |      |                       |                                                                                                                                                                                                           |
| XTU Interface                                                                 |          |      |                       |                                                                                                                                                                                                           |
| BCLK Aware Adaptive Voltage                                                   |          |      |                       | 外频/电压比率调整，当启用时，CPU通常会小心计算外频与电压之间的比率，这个选项通常适用于外频超频而防止电压过高                                                                                                                                                  |
| **Processor**                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Core Max OC Ratio                                                             |          |      |                       |                                                                                                                                                                                                           |
| Core Voltage Mode                                                             |          |      |                       | CPU核心电压模式                                                                                                                                                                                                 |
| Core Voltage Override                                                         |          |      |                       | CPU核心电压固定值                                                                                                                                                                                                |
| Core Extra Turbo Voltage                                                      |          |      |                       | CPU核心电压睿频固定值                                                                                                                                                                                              |
| Core Voltage Offset                                                           |          |      |                       | 加压/减压                                                                                                                                                                                                     |
| Offset Prefix                                                                 |          |      | -                     |                                                                                                                                                                                                           |
| Ring Down Bin                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Ring Max OC Ratio                                                             |          |      |                       |                                                                                                                                                                                                           |
| Ring Min OC Ratio                                                             |          |      |                       |                                                                                                                                                                                                           |
| AVX Ratio Offset                                                              | 0x6A1    | 0x3  | 3                     | AVX倍频补偿，可以大幅度提高超频稳定性，建议设置在3。设置在1可以让CPU在浮点运算的时候比整数低100Mhz，比如你的CPU一般情况下跑在4.5Ghz，那么在跑AVX指令集时就会降低到4.4Ghz，如果设置为3就会降低300Mhz，以此类推。这个选项对于超频十分重要，因为AVX指令集调用时，CPU的功耗发热都非常大，适当降低AVX频率有助于超频成功率的提升                     |
| **GT**                                                                        |          |      |                       | 核显                                                                                                                                                                                                        |
| GT OverClocking Frequency                                                     |          |      |                       |                                                                                                                                                                                                           |
| GT Voltage Mode                                                               |          |      |                       | 核显电压模式                                                                                                                                                                                                    |
| GT Voltage Override                                                           |          |      |                       | 核显电压固定值                                                                                                                                                                                                   |
| GT Extra Turbo Voltage                                                        |          |      |                       | 核显电压睿频固定值                                                                                                                                                                                                 |
| GT Voltage Offset                                                             |          |      |                       | 核显电压偏移值                                                                                                                                                                                                   |
| Offset Prefix                                                                 |          |      |                       | 偏移量正负                                                                                                                                                                                                     |
| **Uncore**                                                                    |          |      |                       |                                                                                                                                                                                                           |
| Uncore Voltage Offset                                                         |          |      |                       |                                                                                                                                                                                                           |
| Offset Prefix                                                                 |          |      |                       |                                                                                                                                                                                                           |
| **Memory Overclocking Menu**                                                  |          |      |                       |                                                                                                                                                                                                           |
| Realtime Memory Timing                                                        |          |      |                       |                                                                                                                                                                                                           |
| Memory profile                                                                |          |      |                       |                                                                                                                                                                                                           |
| Memory Reference Clock                                                        |          |      |                       | 内存参考源时钟，比如 100Mhz 可以实现 DDR 3000、3100 等频率，133Mhz 可以实现 DDR 3333 等频率。因为内存频率是按照与 CPU 外频和 Home Agent 的比率来实现的，通常可以设置为 Auto                                                                                      |
| Memory Ratio                                                                  |          |      |                       | 内存频率                                                                                                                                                                                                      |
| QCLK Odd Ratio                                                                |          |      |                       | 间隔频率                                                                                                                                                                                                      |
| tCL                                                                           |          |      |                       |                                                                                                                                                                                                           |
| tRCD/tRP                                                                      |          |      |                       |                                                                                                                                                                                                           |
| tRAS                                                                          |          |      |                       |                                                                                                                                                                                                           |
| tCWL                                                                          |          |      |                       |                                                                                                                                                                                                           |
| tFAW                                                                          |          |      |                       |                                                                                                                                                                                                           |
| tREFI                                                                         |          |      |                       |                                                                                                                                                                                                           |
| tRFC                                                                          |          |      |                       |                                                                                                                                                                                                           |
| tRRD                                                                          |          |      |                       |                                                                                                                                                                                                           |
| tRTP                                                                          |          |      |                       |                                                                                                                                                                                                           |
| tWR                                                                           |          |      |                       |                                                                                                                                                                                                           |
| tWTR                                                                          |          |      |                       |                                                                                                                                                                                                           |
| NMode                                                                         |          |      |                       |                                                                                                                                                                                                           |
| Memory Voltage                                                                |          |      |                       |                                                                                                                                                                                                           |
| DllBwEn[0]                                                                    |          |      |                       |                                                                                                                                                                                                           |
| DllBwEn[1]                                                                    |          |      |                       |                                                                                                                                                                                                           |
| DllBwEn[2]                                                                    |          |      |                       |                                                                                                                                                                                                           |
| DllBwEn[3]                                                                    |          |      |                       |                                                                                                                                                                                                           |
| **Voltage PLL Trim Controls**                                                 |          |      |                       |                                                                                                                                                                                                           |
| Core PLL Voltage Offset                                                       |          |      |                       |                                                                                                                                                                                                           |
| GT PLL Voltage Offset                                                         |          |      |                       |                                                                                                                                                                                                           |
| Ring PLL Voltage Offset                                                       |          |      |                       | 环形总线锁相环电压                                                                                                                                                                                                 |
| System Agent PLL Voltage Offset                                               |          |      |                       | Home Agent锁相环电压                                                                                                                                                                                           |
| Memory Controller PLL Voltage Offset                                          |          |      |                       | 内存控制器锁相环电压                                                                                                                                                                                                |
| **Intel ICC**                                                                 |          |      |                       |                                                                                                                                                                                                           |
| ICC/OC Watchdog Timer                                                         |          |      |                       |                                                                                                                                                                                                           |
| ICC Locks after EOP                                                           |          |      |                       |                                                                                                                                                                                                           |
| ICC Profile                                                                   |          |      |                       |                                                                                                                                                                                                           |
| ICC PLL Shutdown                                                              |          |      |                       |                                                                                                                                                                                                           |
| **DMI CLOCK placeholder for clock 2 title**                                   |          |      |                       |                                                                                                                                                                                                           |
| Clock Frequency                                                               |          |      |                       |                                                                                                                                                                                                           |
| Bclk Change Type                                                              |          |      |                       |                                                                                                                                                                                                           |
| Spread %                                                                      |          |      |                       |                                                                                                                                                                                                           |
| **PCI CLOCK placeholder for clock 3 title**                                   |          |      |                       |                                                                                                                                                                                                           |
| Clock Frequency                                                               |          |      |                       |                                                                                                                                                                                                           |
| Spread %                                                                      |          |      |                       |                                                                                                                                                                                                           |
| **Trusted Computing**                                                         |          |      |                       |                                                                                                                                                                                                           |
| Security Device Support                                                       |          |      |                       |                                                                                                                                                                                                           |
| SHA-1 PCR Bank                                                                |          |      |                       |                                                                                                                                                                                                           |
| SHA256 PCR Bank                                                               |          |      |                       |                                                                                                                                                                                                           |
| SHA384 PCR Bank                                                               |          |      |                       |                                                                                                                                                                                                           |
| SHA512 PCR Bank                                                               |          |      |                       |                                                                                                                                                                                                           |
| SM3_256 PCR Bank                                                              |          |      |                       |                                                                                                                                                                                                           |
| TPM State                                                                     |          |      |                       |                                                                                                                                                                                                           |
| Pending operation                                                             |          |      |                       |                                                                                                                                                                                                           |
| Platform Hierarchy                                                            |          |      |                       |                                                                                                                                                                                                           |
| Storage Hierarchy                                                             |          |      |                       |                                                                                                                                                                                                           |
| Endorsement Hierarchy                                                         |          |      |                       |                                                                                                                                                                                                           |
| TPM2.0 UEFI Spec Version                                                      |          |      |                       |                                                                                                                                                                                                           |
| Physical Presence Spec Version                                                |          |      |                       |                                                                                                                                                                                                           |
| TPM 20 InterfaceType                                                          |          |      |                       |                                                                                                                                                                                                           |
| Device Select                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Security Device Support                                                       |          |      |                       |                                                                                                                                                                                                           |
| TCM State                                                                     |          |      |                       |                                                                                                                                                                                                           |
| Pending operation                                                             |          |      |                       |                                                                                                                                                                                                           |
| **ACPI Settings**                                                             |          |      |                       |                                                                                                                                                                                                           |
| Enable ACPI Auto Configuration                                                |          |      |                       |                                                                                                                                                                                                           |
| Enable Hibernation                                                            |          |      |                       |                                                                                                                                                                                                           |
| ACPI Sleep State                                                              |          |      |                       |                                                                                                                                                                                                           |
| Lock Legacy Resources                                                         |          |      |                       |                                                                                                                                                                                                           |
| S3 Video Repost                                                               |          |      |                       |                                                                                                                                                                                                           |
| **Intel Bios Guard Support**                                                  |          |      |                       |                                                                                                                                                                                                           |
| Intel Bios Guard Support                                                      |          |      |                       |                                                                                                                                                                                                           |
| **AMI Graphic Output Protocol Policy**                                        |          |      |                       |                                                                                                                                                                                                           |
| Output Select                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Brightnesst Setting                                                           |          |      |                       |                                                                                                                                                                                                           |
| BIST Enable                                                                   |          |      |                       |                                                                                                                                                                                                           |
| **Network Stack Configuration**                                               |          |      |                       |                                                                                                                                                                                                           |
| Network Stack                                                                 |          |      |                       | UEFI网络堆栈(network stack)功能                                                                                                                                                                                 |
| Ipv4 PXE Support                                                              |          |      |                       | Ipv4 PXE启动项支持                                                                                                                                                                                             |
| Ipv4 HTTP Support                                                             |          |      |                       |                                                                                                                                                                                                           |
| Ipv6 PXE Support                                                              |          |      |                       | Ipv6 PXE启动项支持                                                                                                                                                                                             |
| Ipv6 HTTP Support                                                             |          |      |                       |                                                                                                                                                                                                           |
| IP6 Configuration Policy                                                      |          |      |                       |                                                                                                                                                                                                           |
| PXE boot wait time                                                            |          |      |                       |                                                                                                                                                                                                           |
| Media detect count                                                            |          |      |                       |                                                                                                                                                                                                           |
| **CSM Configuration**                                                         |          |      |                       |                                                                                                                                                                                                           |
| CSM Support                                                                   |          |      |                       | 兼容性支持模块（CSM）是UEFI固件的一个组件，它通过模拟BIOS环境提供传统BIOS兼容性，允许传统操作系统和一些不支持UEFI的选项ROM继续使用                                                                                                                              |
| GateA20 Active                                                                |          |      |                       |                                                                                                                                                                                                           |
| Option ROM Messages                                                           |          |      |                       | [Force BIOS]选购设备固件程序会在开机显示，[Keep Current]开箱时只显示ASUS标志                                                                                                                                                     |
| INT19 Trap Response                                                           |          |      |                       |                                                                                                                                                                                                           |
| Boot option filter                                                            |          |      |                       |                                                                                                                                                                                                           |
| Network                                                                       |          |      |                       |                                                                                                                                                                                                           |
| Storage                                                                       |          |      |                       |                                                                                                                                                                                                           |
| Video                                                                         |          |      |                       |                                                                                                                                                                                                           |
| Other PCI devices                                                             |          |      |                       |                                                                                                                                                                                                           |
| **SDIO Configuration**                                                        |          |      |                       |                                                                                                                                                                                                           |
| SDIO Access Mode                                                              |          |      |                       |                                                                                                                                                                                                           |
| **Switchable Graphics**                                                       |          |      |                       |                                                                                                                                                                                                           |
| SG Mode Select                                                                |          |      |                       |                                                                                                                                                                                                           |
| **USB Configuration**                                                         |          |      |                       |                                                                                                                                                                                                           |
| USB Support                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Legacy USB Support                                                            |          |      |                       | 传统USB支持                                                                                                                                                                                                   |
| USB 2.0 Controller Mode                                                       |          |      |                       |                                                                                                                                                                                                           |
| XHCI Legacy Support                                                           |          |      |                       |                                                                                                                                                                                                           |
| XHCI Hand-off                                                                 |          |      |                       | 让操作系统接管XHCI控制器                                                                                                                                                                                            |
| EHCI Hand-off                                                                 |          |      |                       |                                                                                                                                                                                                           |
| USB Mass Storage Driver Support                                               |          |      |                       |                                                                                                                                                                                                           |
| Port 60/64 Emulation                                                          |          |      |                       |                                                                                                                                                                                                           |
| USB transfer time-out                                                         |          |      |                       |                                                                                                                                                                                                           |
| Device reset time-out                                                         |          |      |                       |                                                                                                                                                                                                           |
| Device power-up delay                                                         |          |      |                       |                                                                                                                                                                                                           |
| Device power-up delay in seconds                                              |          |      |                       |                                                                                                                                                                                                           |
| **Demo Board**                                                                |          |      |                       |                                                                                                                                                                                                           |
| CRB Test                                                                      |          |      |                       |                                                                                                                                                                                                           |
| **System Agent (SA) Configuration**                                           |          |      |                       |                                                                                                                                                                                                           |
| Stop Grant Configuration                                                      |          |      |                       |                                                                                                                                                                                                           |
| Number of Stop Grant Cycles                                                   |          |      |                       |                                                                                                                                                                                                           |
| VT-d                                                                          |          |      |                       |                                                                                                                                                                                                           |
| CHAP Device (B0:D7:F0)                                                        |          |      |                       |                                                                                                                                                                                                           |
| Thermal Device (B0:D4:F0)                                                     |          |      |                       |                                                                                                                                                                                                           |
| GMM Device (B0:D8:F0)                                                         |          |      |                       |                                                                                                                                                                                                           |
| CRID Support                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Above 4GB MMIO BIOS assignment                                                |          |      | Enabled               | 4G以上内存映射的IO BIOS分配                                                                                                                                                                                        |
| X2APIC Opt Out                                                                |          |      |                       |                                                                                                                                                                                                           |
| SKY CAM Device (B0:D5:F0)                                                     |          |      |                       |                                                                                                                                                                                                           |
| eDRAM Mode                                                                    |          |      |                       |                                                                                                                                                                                                           |
| **Graphics Configuration**                                                    |          |      |                       |                                                                                                                                                                                                           |
| Graphics Turbo IMON Current                                                   |          |      |                       |                                                                                                                                                                                                           |
| Skip Scaning of External Gfx Card                                             | 0x8AE    | 0x0  | Disabled              | 跳过外部显卡扫描                                                                                                                                                                                                  |
| Primary Display                                                               | 0x80D    | 0x4  | SG                    | 不同显示类型的优先级，Auto：自动选择显示类型。 IGFX：从主板上的核心显卡输出端口输出视频。 PEG：从CPU上的PCIe插槽输出视频。 PCI：从南桥上的PCIe插槽输出视频。 SG：自动切换，此模式两种显示都会保留。 SG Delay After Power Enable：SG模式在开机后的时延（毫秒）。 SG Delay After Hold Reset：SG模式在重启后的时延（毫秒）。 |
| Select PCIE Card                                                              | 0x80E    | 0x2  | Auto                  |                                                                                                                                                                                                           |
| SG Delay After Power Enable                                                   |          |      |                       |                                                                                                                                                                                                           |
| SG Delay After Hold Reset                                                     |          |      |                       |                                                                                                                                                                                                           |
| Internal Graphics                                                             | 0x813    | 0x2  | Auto                  | 设置内部集成显卡显示，菜单选项为： Auto：当插入外部显卡时，自动屏蔽内部集成显示。 Disabled：关闭内部集成显卡显示。 Enabled：开启内部集成显卡显示。                                                                                                                      |
| GTT Size                                                                      | 0x74A    | 0x3  |                       |                                                                                                                                                                                                           |
| Aperture Size                                                                 | 0x74B    | 0x1  | 256MB                 | 显示数据处理的临时缓冲区大小                                                                                                                                                                                            |
| DVMT Pre-Allocated                                                            | 0x7E8    | 0x2  | 64M                   | 动态共享显存预设值                                                                                                                                                                                                 |
| DVMT Total Gfx Mem                                                            | 0x7E9    | 0x2  | 256M                  | 设置动态共享显存分配最大值。有三种选择：128M，256M，MAX，缺省值256M。MAX值与驱动及操作系统有关，在windows操作系统下，为主板内存的一半。由于集显本身是没有内存的，所以需要共享主板的内存。当GPU不再需要占用这些内存资源时，会自动归还给系统内存，将其返还操作系统供其它应用程序或系统功能使用。                                             |
| Intel Graphics Pei Display Peim                                               |          |      |                       |                                                                                                                                                                                                           |
| Gfx Low Power Mode                                                            |          |      |                       |                                                                                                                                                                                                           |
| ALS Support                                                                   |          |      |                       |                                                                                                                                                                                                           |
| VDD Enable                                                                    |          |      |                       |                                                                                                                                                                                                           |
| PM Support                                                                    |          |      |                       |                                                                                                                                                                                                           |
| PAVP Enable                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Cdynmax Clamping Enable                                                       |          |      |                       |                                                                                                                                                                                                           |
| Cd Clock Frequency                                                            |          |      |                       |                                                                                                                                                                                                           |
| IUER Button Enable                                                            |          |      |                       |                                                                                                                                                                                                           |
| **LCD Control**                                                               |          |      |                       |                                                                                                                                                                                                           |
| Primary IGFX Boot Display                                                     |          |      |                       |                                                                                                                                                                                                           |
| Secondary IGFX Boot Display                                                   |          |      |                       |                                                                                                                                                                                                           |
| LCD Panel Type                                                                |          |      |                       |                                                                                                                                                                                                           |
| Panel Scaling                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Backlight Control                                                             |          |      |                       |                                                                                                                                                                                                           |
| Active LFP                                                                    |          |      |                       |                                                                                                                                                                                                           |
| Backlight Brightness                                                          |          |      |                       |                                                                                                                                                                                                           |
| **Intel(R) Ultrabook Event Support**                                          |          |      |                       |                                                                                                                                                                                                           |
| IUER Slate Enable                                                             |          |      |                       |                                                                                                                                                                                                           |
| Slate Mode boot value                                                         |          |      |                       |                                                                                                                                                                                                           |
| Slate Mode on S3 and S4 resume                                                |          |      |                       |                                                                                                                                                                                                           |
| IUER Dock Enable                                                              |          |      |                       |                                                                                                                                                                                                           |
| Dock Mode boot value                                                          |          |      |                       |                                                                                                                                                                                                           |
| Dock Mode upon S3 and S4 resume                                               |          |      |                       |                                                                                                                                                                                                           |
| **External Gfx Card Primary Display Configuration**                           |          |      |                       |                                                                                                                                                                                                           |
| Primary PEG                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Primary PCIE                                                                  |          |      |                       |                                                                                                                                                                                                           |
| **DMI/OPI Configuration**                                                     |          |      |                       |                                                                                                                                                                                                           |
| DMI Max Link Speed                                                            |          |      |                       | 设置DMI速度                                                                                                                                                                                                   |
| DMI Gen3 Eq Phase 2                                                           |          |      |                       |                                                                                                                                                                                                           |
| DMI Gen3 Eq Phase 3 Method                                                    |          |      |                       |                                                                                                                                                                                                           |
| DMI Vc1 Control                                                               |          |      |                       |                                                                                                                                                                                                           |
| DMI Vcm Control                                                               |          |      |                       |                                                                                                                                                                                                           |
| Program Static Phase1 Eq                                                      |          |      |                       |                                                                                                                                                                                                           |
| DMI Link ASPM Control                                                         |          |      |                       | 设置DMI Link上北桥与南桥的ASPM (Active State Power Management)功能                                                                                                                                                   |
| DMI Extended Sync Control                                                     |          |      |                       |                                                                                                                                                                                                           |
| DMI De-emphasis Control                                                       |          |      |                       |                                                                                                                                                                                                           |
| DMI IOT                                                                       |          |      |                       |                                                                                                                                                                                                           |
| **Gen3 Root Port Preset value for each Lane**                                 |          |      |                       |                                                                                                                                                                                                           |
| **Gen3 Endpoint Preset value for each Lane**                                  |          |      |                       |                                                                                                                                                                                                           |
| **Gen3 Endpoint Hint value for each Lane**                                    |          |      |                       |                                                                                                                                                                                                           |
| Lane 0                                                                        |          |      |                       |                                                                                                                                                                                                           |
| Lane 1                                                                        |          |      |                       |                                                                                                                                                                                                           |
| Lane 2                                                                        |          |      |                       |                                                                                                                                                                                                           |
| Lane 3                                                                        |          |      |                       |                                                                                                                                                                                                           |
| **Gen3 RxCTLE Control**                                                       |          |      |                       |                                                                                                                                                                                                           |
| Bundle0                                                                       |          |      |                       |                                                                                                                                                                                                           |
| Bundle1                                                                       |          |      |                       |                                                                                                                                                                                                           |
| **PEG 0:1:0**                                                                 |          |      |                       |                                                                                                                                                                                                           |
| **PEG 0:1:1**                                                                 |          |      |                       |                                                                                                                                                                                                           |
| **PEG 0:1:2**                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Enable Root Port                                                              |          |      |                       |                                                                                                                                                                                                           |
| Max Link Speed                                                                |          |      |                       |                                                                                                                                                                                                           |
| Max Link Width                                                                |          |      |                       |                                                                                                                                                                                                           |
| Power Down Unused Lanes                                                       |          |      |                       |                                                                                                                                                                                                           |
| Gen3 Eq Phase 2                                                               |          |      |                       |                                                                                                                                                                                                           |
| Gen3 Eq Phase 3 Method                                                        |          |      |                       |                                                                                                                                                                                                           |
| ASPM                                                                          |          |      |                       |                                                                                                                                                                                                           |
| ASPM L0s                                                                      |          |      |                       |                                                                                                                                                                                                           |
| De-emphasis Control                                                           |          |      |                       |                                                                                                                                                                                                           |
| OBFF                                                                          |          |      |                       |                                                                                                                                                                                                           |
| LTR                                                                           |          |      |                       |                                                                                                                                                                                                           |
| PEG0 Slot Power Limit Value                                                   |          |      |                       |                                                                                                                                                                                                           |
| PEG0 Slot Power Limit Scale                                                   |          |      |                       |                                                                                                                                                                                                           |
| PEG0 Physical Slot Number                                                     |          |      |                       |                                                                                                                                                                                                           |
| PEG0 Hotplug                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Extra Bus Reserved                                                            |          |      |                       |                                                                                                                                                                                                           |
| Reserved Memory                                                               |          |      |                       |                                                                                                                                                                                                           |
| Reserved I/O                                                                  |          |      |                       |                                                                                                                                                                                                           |
| **PEG Port Configuration**                                                    |          |      |                       |                                                                                                                                                                                                           |
| PEG0 Max Payload size                                                         |          |      |                       |                                                                                                                                                                                                           |
| PEG1 Max Payload size                                                         |          |      |                       |                                                                                                                                                                                                           |
| PEG2 Max Payload size                                                         |          |      |                       |                                                                                                                                                                                                           |
| Program PCIe ASPM after OpROM                                                 |          |      |                       |                                                                                                                                                                                                           |
| Program Static Phase1 Eq                                                      |          |      |                       |                                                                                                                                                                                                           |
| Always Attempt SW EQ                                                          |          |      |                       |                                                                                                                                                                                                           |
| Number of Presets to test                                                     |          |      |                       |                                                                                                                                                                                                           |
| Allow PERST# GPIO Usage                                                       |          |      |                       |                                                                                                                                                                                                           |
| SW EQ Enable VOC                                                              |          |      |                       |                                                                                                                                                                                                           |
| Jitter Dwell Time                                                             |          |      |                       |                                                                                                                                                                                                           |
| Jitter Error Target                                                           |          |      |                       |                                                                                                                                                                                                           |
| VOC Dwell Time                                                                |          |      |                       |                                                                                                                                                                                                           |
| VOC Error Target                                                              |          |      |                       |                                                                                                                                                                                                           |
| Generate BDAT PEG Margin Data                                                 |          |      |                       |                                                                                                                                                                                                           |
| PCIe Rx CEM Test Mode                                                         |          |      |                       |                                                                                                                                                                                                           |
| PEG Lane number for Test                                                      |          |      |                       |                                                                                                                                                                                                           |
| Non-Protocol Awareness                                                        |          |      |                       |                                                                                                                                                                                                           |
| PCIe Spread Spectrum Clocking                                                 |          |      |                       |                                                                                                                                                                                                           |
| **Gen3 Root Port Preset value for each Lane**                                 |          |      |                       |                                                                                                                                                                                                           |
| **Gen3 Endpoint Preset value for each Lane**                                  |          |      |                       |                                                                                                                                                                                                           |
| **Gen3 Endpoint Hint value for each Lane**                                    |          |      |                       |                                                                                                                                                                                                           |
| Lane 0-15                                                                     |          |      |                       |                                                                                                                                                                                                           |
| **Gen3 RxCTLE Control**                                                       |          |      |                       |                                                                                                                                                                                                           |
| Bundle0-7                                                                     |          |      |                       |                                                                                                                                                                                                           |
| RxCTLE Override                                                               |          |      |                       |                                                                                                                                                                                                           |
| **PEG Port Feature Configuration**                                            |          |      |                       |                                                                                                                                                                                                           |
| Detect Non-Compliance Device                                                  |          |      |                       |                                                                                                                                                                                                           |
| **PCH-IO Configuration**                                                      |          |      |                       |                                                                                                                                                                                                           |
| DCI enable (HDCIEN)                                                           |          |      |                       |                                                                                                                                                                                                           |
| USB/UART                                                                      |          |      |                       |                                                                                                                                                                                                           |
| Debug Port Selection                                                          |          |      |                       |                                                                                                                                                                                                           |
| GNSS                                                                          |          |      |                       |                                                                                                                                                                                                           |
| GNSS Device Model                                                             |          |      |                       |                                                                                                                                                                                                           |
| PCH LAN Controller                                                            |          |      |                       |                                                                                                                                                                                                           |
| Sensor Hub Type                                                               |          |      |                       |                                                                                                                                                                                                           |
| DeepSx Power Policies                                                         |          |      |                       |                                                                                                                                                                                                           |
| LAN Wake From DeepSx                                                          |          |      |                       |                                                                                                                                                                                                           |
| Wake on LAN Enable                                                            |          |      |                       |                                                                                                                                                                                                           |
| SLP_LAN# Low on DC Power                                                      |          |      |                       |                                                                                                                                                                                                           |
| K1 off                                                                        |          |      |                       |                                                                                                                                                                                                           |
| Wake on WLAN and BT Enable                                                    |          |      |                       |                                                                                                                                                                                                           |
| DeepSx Wake on WLAN and BT Enable                                             |          |      |                       |                                                                                                                                                                                                           |
| Disable DSX ACPRESENT PullDown                                                |          |      |                       |                                                                                                                                                                                                           |
| CLKRUN# logic                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Serial IRQ Mode                                                               |          |      |                       |                                                                                                                                                                                                           |
| Port 61h Bit-4 Emulation                                                      |          |      |                       |                                                                                                                                                                                                           |
| High Precision Timer                                                          |          |      |                       |                                                                                                                                                                                                           |
| State After G3                                                                |          |      |                       |                                                                                                                                                                                                           |
| Port 80h Redirection                                                          |          |      |                       |                                                                                                                                                                                                           |
| Enhance Port 80h LPC Decoding                                                 |          |      |                       |                                                                                                                                                                                                           |
| Compatible Revision ID                                                        |          |      |                       |                                                                                                                                                                                                           |
| PCH Cross Throttling                                                          |          |      |                       |                                                                                                                                                                                                           |
| Disable Energy Reporting                                                      |          |      |                       |                                                                                                                                                                                                           |
| Enable TCO Timer                                                              |          |      |                       |                                                                                                                                                                                                           |
| Pcie Pll SSC                                                                  |          |      |                       |                                                                                                                                                                                                           |
| IOAPIC 24-119 Entries                                                         |          |      |                       | IRQ24-119硬件中断性访问入口，建议保持开启以提升芯片组的响应速度                                                                                                                                                                      |
| Unlock PCH P2SB                                                               |          |      |                       |                                                                                                                                                                                                           |
| PMC READ DISABLE                                                              |          |      |                       |                                                                                                                                                                                                           |
| Flash Protection Range Registers (FPRR)                                       |          |      |                       |                                                                                                                                                                                                           |
| SPD Write Disable                                                             |          |      |                       |                                                                                                                                                                                                           |
| ChipsetInit HECI Message                                                      |          |      |                       |                                                                                                                                                                                                           |
| Bypass ChipsetInit sync reset                                                 |          |      |                       |                                                                                                                                                                                                           |
| **PCI Express Configuration**                                                 |          |      |                       |                                                                                                                                                                                                           |
| PCI Express Clock Gating                                                      |          |      |                       |                                                                                                                                                                                                           |
| Legacy IO Low Latency                                                         |          |      |                       |                                                                                                                                                                                                           |
| DMI Link ASPM Control                                                         |          |      |                       |                                                                                                                                                                                                           |
| Port8xh Decode                                                                |          |      |                       |                                                                                                                                                                                                           |
| Port8xh Decode Port#                                                          |          |      |                       |                                                                                                                                                                                                           |
| Peer Memory Write Enable                                                      |          |      |                       |                                                                                                                                                                                                           |
| Compliance Test Mode                                                          |          |      |                       |                                                                                                                                                                                                           |
| PCIe-USB Glitch W/A                                                           |          |      |                       |                                                                                                                                                                                                           |
| PCIe function swap                                                            |          |      |                       |                                                                                                                                                                                                           |
| **PCI Express Gen3 Eq Lanes**                                                 |          |      |                       |                                                                                                                                                                                                           |
| PCIE1-20  Cm                                                                  |          |      |                       |                                                                                                                                                                                                           |
| PCIE1-20  Cp                                                                  |          |      |                       |                                                                                                                                                                                                           |
| **Override SW EQ settings**                                                   |          |      |                       |                                                                                                                                                                                                           |
| Coeff0-4 Cm                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Coeff0-4 Cp                                                                   |          |      |                       |                                                                                                                                                                                                           |
| **PCI Express Root Port 1-24**                                                |          |      |                       |                                                                                                                                                                                                           |
| PCI Express Root Port 1-24                                                    |          |      |                       |                                                                                                                                                                                                           |
| Topology                                                                      |          |      |                       |                                                                                                                                                                                                           |
| ASPM                                                                          |          |      |                       |                                                                                                                                                                                                           |
| L1 Substates                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Gen3 Eq Phase3 Method                                                         |          |      |                       |                                                                                                                                                                                                           |
| UPTP                                                                          |          |      |                       |                                                                                                                                                                                                           |
| DPTP                                                                          |          |      |                       |                                                                                                                                                                                                           |
| ACS                                                                           |          |      |                       |                                                                                                                                                                                                           |
| URR                                                                           |          |      |                       |                                                                                                                                                                                                           |
| FER                                                                           |          |      |                       |                                                                                                                                                                                                           |
| NFER                                                                          |          |      |                       |                                                                                                                                                                                                           |
| CER                                                                           |          |      |                       |                                                                                                                                                                                                           |
| CTO                                                                           |          |      |                       |                                                                                                                                                                                                           |
| SEFE                                                                          |          |      |                       |                                                                                                                                                                                                           |
| SENFE                                                                         |          |      |                       |                                                                                                                                                                                                           |
| SECE                                                                          |          |      |                       |                                                                                                                                                                                                           |
| PME SCI                                                                       |          |      |                       |                                                                                                                                                                                                           |
| Hot Plug                                                                      |          |      |                       |                                                                                                                                                                                                           |
| Advanced Error Reporting                                                      |          |      |                       |                                                                                                                                                                                                           |
| PCIe Speed                                                                    |          |      |                       | PCIE设备速度（低模式省电，高模式提供高性能）                                                                                                                                                                                  |
| Transmitter Half Swing                                                        |          |      |                       |                                                                                                                                                                                                           |
| Extra Bus Reserved                                                            |          |      |                       |                                                                                                                                                                                                           |
| Reserved Memory                                                               |          |      |                       |                                                                                                                                                                                                           |
| Reserved I/O                                                                  |          |      |                       |                                                                                                                                                                                                           |
| PCH PCIE1 LTR                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Snoop Latency Override                                                        |          |      |                       |                                                                                                                                                                                                           |
| Snoop Latency Value                                                           |          |      |                       |                                                                                                                                                                                                           |
| Snoop Latency Multiplier                                                      |          |      |                       |                                                                                                                                                                                                           |
| Non Snoop Latency Override                                                    |          |      |                       |                                                                                                                                                                                                           |
| Non Snoop Latency Value                                                       |          |      |                       |                                                                                                                                                                                                           |
| Non Snoop Latency Multiplier                                                  |          |      |                       |                                                                                                                                                                                                           |
| Force LTR Override                                                            |          |      |                       |                                                                                                                                                                                                           |
| PCIE1 LTR Lock                                                                |          |      |                       |                                                                                                                                                                                                           |
| PCIE1 CLKREQ Mapping Override                                                 |          |      |                       |                                                                                                                                                                                                           |
| CLKREQ Number                                                                 |          |      |                       |                                                                                                                                                                                                           |
| **USB Configuration**                                                         |          |      |                       |                                                                                                                                                                                                           |
| SSIC Port 1                                                                   |          |      |                       |                                                                                                                                                                                                           |
| SSIC Port 2                                                                   |          |      |                       |                                                                                                                                                                                                           |
| XHCI Disable Compliance Mode                                                  |          |      |                       |                                                                                                                                                                                                           |
| xDCI Support                                                                  |          |      |                       |                                                                                                                                                                                                           |
| USB Port Disable Override                                                     |          |      |                       | USB接口禁用复写功能                                                                                                                                                                                               |
| USB SS Physical Connector #0-13                                               |          |      |                       |                                                                                                                                                                                                           |
| USB Sensor Hub                                                                |          |      |                       |                                                                                                                                                                                                           |
| **SATA And RST Configuration**                                                |          |      |                       |                                                                                                                                                                                                           |
| SATA Controller(s)                                                            |          |      |                       | SATA控制器的启用和禁用                                                                                                                                                                                             |
| SATA Mode Selection                                                           |          |      |                       | SATA控制器模式。AHCI模式（SATA 模式）/Intel PST Premium（RAID）模式/Optane 模式/IDE模式（模仿古老的大宽硬盘并行接口）                                                                                                                        |
| PCIe Storage Dev On Port 1/3/5/7/9/11/13/15/17/21/23                          |          |      |                       |                                                                                                                                                                                                           |
| SATA Test Mode                                                                |          |      |                       |                                                                                                                                                                                                           |
| RAID Device ID                                                                |          |      |                       |                                                                                                                                                                                                           |
| SATA Controller Speed                                                         |          |      |                       | SATA控制器速度                                                                                                                                                                                                 |
| **Serial ATA Port 0-7**                                                       |          |      |                       |                                                                                                                                                                                                           |
| Port 0-7                                                                      |          |      |                       |                                                                                                                                                                                                           |
| Hot Plug                                                                      |          |      |                       | SATA热插拔功能                                                                                                                                                                                                 |
| Mechanical Presence Switch                                                    |          |      |                       |                                                                                                                                                                                                           |
| Spin Up Device                                                                |          |      |                       | 硬盘交错旋转加速功能                                                                                                                                                                                                |
| SATA Device Type                                                              |          |      |                       | 所安装的SATA设备类型                                                                                                                                                                                              |
| Topology                                                                      |          |      |                       | SATA拓撲                                                                                                                                                                                                    |
| SATA Port 0 DevSlp                                                            |          |      |                       |                                                                                                                                                                                                           |
| DITO Configuration                                                            |          |      |                       |                                                                                                                                                                                                           |
| DITO Value                                                                    |          |      |                       |                                                                                                                                                                                                           |
| DM Value                                                                      |          |      |                       |                                                                                                                                                                                                           |
| **Software Feature Mask Configuration**                                       |          |      |                       |                                                                                                                                                                                                           |
| HDD Unlock                                                                    |          |      | Enabled               | 打开本项目后，激活硬盘密度解锁功能                                                                                                                                                                                         |
| LED Locate                                                                    |          |      |                       |                                                                                                                                                                                                           |
| Use RST Legacy OROM                                                           |          |      |                       |                                                                                                                                                                                                           |
| RAID0/1/5/10                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Intel Rapid Recovery Technology                                               |          |      |                       | 设置Intel Rapid Recovery Technology                                                                                                                                                                         |
| OROM UI and BANNER                                                            |          |      |                       | 打开本项目后，OROM UI会显示                                                                                                                                                                                         |
| IRRT Only on eSATA                                                            |          |      |                       |                                                                                                                                                                                                           |
| Smart Response Technology                                                     |          |      |                       | 开启或关闭快速反应技术                                                                                                                                                                                               |
| OROM UI Normal Delay                                                          |          |      |                       | OROM UI Splash画面在一般状态下的延迟时间                                                                                                                                                                               |
| RST Force Form                                                                |          |      |                       |                                                                                                                                                                                                           |
| System Acceleration with Intel(R) Optane(TM) Memory                           |          |      |                       |                                                                                                                                                                                                           |
| **HD Audio Subsystem Configuration Settings**                                 |          |      |                       |                                                                                                                                                                                                           |
| HD Audio                                                                      |          |      |                       |                                                                                                                                                                                                           |
| Audio DSP                                                                     |          |      |                       |                                                                                                                                                                                                           |
| Audio DSP Compliance Mode                                                     |          |      |                       |                                                                                                                                                                                                           |
| HDA-Link Codec Select                                                         |          |      |                       |                                                                                                                                                                                                           |
| iDisplay Audio Disconnect                                                     |          |      |                       |                                                                                                                                                                                                           |
| PME Enable                                                                    |          |      |                       |                                                                                                                                                                                                           |
| **HD Audio Subsystem Advanced Configuration Settings**                        |          |      |                       |                                                                                                                                                                                                           |
| I/O Buffer Ownership                                                          |          |      |                       |                                                                                                                                                                                                           |
| I2S Configuration Select                                                      |          |      |                       |                                                                                                                                                                                                           |
| I/O Buffer Voltage Select                                                     |          |      |                       |                                                                                                                                                                                                           |
| HD Audio Link Frequency                                                       |          |      |                       |                                                                                                                                                                                                           |
| iDisplay Link T-Mode                                                          |          |      |                       |                                                                                                                                                                                                           |
| **Audio DSP NHLT Endpoints Configuration**                                    |          |      |                       |                                                                                                                                                                                                           |
| DMIC                                                                          |          |      |                       |                                                                                                                                                                                                           |
| Bluetooth                                                                     |          |      |                       |                                                                                                                                                                                                           |
| I2S                                                                           |          |      |                       |                                                                                                                                                                                                           |
| **Audio DSP Feature Support**                                                 |          |      |                       |                                                                                                                                                                                                           |
| WoV (Wake on Voice)                                                           |          |      |                       |                                                                                                                                                                                                           |
| DSP based Speech Pre-Processing Disabled                                      |          |      |                       |                                                                                                                                                                                                           |
| Voice Activity Detection                                                      |          |      |                       |                                                                                                                                                                                                           |
| **Audio DSP Pre/Post-Processing Module Support**                              |          |      |                       |                                                                                                                                                                                                           |
| IntelSST Speech                                                               |          |      |                       |                                                                                                                                                                                                           |
| Intel WoV                                                                     |          |      |                       |                                                                                                                                                                                                           |
| **Security Configuration**                                                    |          |      |                       |                                                                                                                                                                                                           |
| RTC Lock                                                                      |          |      |                       |                                                                                                                                                                                                           |
| BIOS Lock                                                                     | 0x8FB    | 0x0  | Disabled              | BIOS锁                                                                                                                                                                                                     |
| **SerialIo Configuration**                                                    |          |      |                       |                                                                                                                                                                                                           |
| Touch Panel                                                                   |          |      |                       |                                                                                                                                                                                                           |
| BT/UART Mux Select                                                            |          |      |                       |                                                                                                                                                                                                           |
| I2C0-I2C5 Controller                                                          |          |      |                       |                                                                                                                                                                                                           |
| SPI0 Controller                                                               |          |      |                       |                                                                                                                                                                                                           |
| SPI1 Controller                                                               |          |      |                       |                                                                                                                                                                                                           |
| UART0 Controller                                                              |          |      |                       |                                                                                                                                                                                                           |
| UART1 Controller                                                              |          |      |                       |                                                                                                                                                                                                           |
| UART2 Controller                                                              |          |      |                       |                                                                                                                                                                                                           |
| GPIO Controller                                                               |          |      |                       |                                                                                                                                                                                                           |
| WITT/MITT Test Device                                                         |          |      |                       |                                                                                                                                                                                                           |
| WITT/MITT Device selection                                                    |          |      |                       |                                                                                                                                                                                                           |
| UART Test Device                                                              |          |      |                       |                                                                                                                                                                                                           |
| UCSI/UCMC device                                                              |          |      |                       |                                                                                                                                                                                                           |
| **SerialIO timing parameters**                                                |          |      |                       |                                                                                                                                                                                                           |
| StandardSpeed SCL High for I2C1                                               |          |      |                       |                                                                                                                                                                                                           |
| StandardSpeed SCL Low for I2C1                                                |          |      |                       |                                                                                                                                                                                                           |
| StandardSpeed SDA Hold for I2C1                                               |          |      |                       |                                                                                                                                                                                                           |
| FastSpeed SCL High for I2C1                                                   |          |      |                       |                                                                                                                                                                                                           |
| FastSpeed SCL Low for I2C1                                                    |          |      |                       |                                                                                                                                                                                                           |
| FastSpeed SDA Hold for I2C1                                                   |          |      |                       |                                                                                                                                                                                                           |
| FastSpeedPlus SCL High for I2C1                                               |          |      |                       |                                                                                                                                                                                                           |
| FastSpeedPlus SCL Low for I2C1                                                |          |      |                       |                                                                                                                                                                                                           |
| FastSpeedPlus SDA Hold for I2C1                                               |          |      |                       |                                                                                                                                                                                                           |
| D0->D3 idle timeout for I2C1 (screen off)                                     |          |      |                       |                                                                                                                                                                                                           |
| D0->D3 idle timeout for I2C1 (screen on)                                      |          |      |                       |                                                                                                                                                                                                           |
| D0->D3 idle timeout for SPI1 (screen off)                                     |          |      |                       |                                                                                                                                                                                                           |
| D0->D3 idle timeout for SPI1 (screen on)                                      |          |      |                       |                                                                                                                                                                                                           |
| D0->D3 idle timeout for UART1 (screen off)                                    |          |      |                       |                                                                                                                                                                                                           |
| D0->D3 idle timeout for UART1 (screen on)                                     |          |      |                       |                                                                                                                                                                                                           |
| **Serial IO I2C0/1 Settings**                                                 |          |      |                       |                                                                                                                                                                                                           |
| I2C IO Voltage Select                                                         |          |      |                       |                                                                                                                                                                                                           |
| Connected device                                                              |          |      |                       |                                                                                                                                                                                                           |
| Interrupt mode                                                                |          |      |                       |                                                                                                                                                                                                           |
| Device's bus address                                                          |          |      |                       |                                                                                                                                                                                                           |
| Device's HID address                                                          |          |      |                       |                                                                                                                                                                                                           |
| Device's bus speed                                                            |          |      |                       |                                                                                                                                                                                                           |
| **Serial IO I2C2-5 Settings**                                                 |          |      |                       |                                                                                                                                                                                                           |
| I2C IO Voltage Select                                                         |          |      |                       |                                                                                                                                                                                                           |
| **Serial IO SPI0 Settings**                                                   |          |      |                       |                                                                                                                                                                                                           |
| ChipSelect polarity                                                           |          |      |                       |                                                                                                                                                                                                           |
| **Serial IO SPI1 Settings**                                                   |          |      |                       |                                                                                                                                                                                                           |
| Enable Finger Print Sensor                                                    |          |      |                       |                                                                                                                                                                                                           |
| Finger Print Sensor                                                           |          |      |                       |                                                                                                                                                                                                           |
| Interrupt mode                                                                |          |      |                       |                                                                                                                                                                                                           |
| **Serial IO UART0 Settings**                                                  |          |      |                       |                                                                                                                                                                                                           |
| Bluetooth Device                                                              |          |      |                       |                                                                                                                                                                                                           |
| BT Interrupt Mode                                                             |          |      |                       |                                                                                                                                                                                                           |
| Wireless Charging Mode                                                        |          |      |                       |                                                                                                                                                                                                           |
| Hardware Flow Control                                                         |          |      |                       |                                                                                                                                                                                                           |
| **Serial IO UART1/2 Settings**                                                |          |      |                       |                                                                                                                                                                                                           |
| Hardware Flow Control                                                         |          |      |                       |                                                                                                                                                                                                           |
| **GPIO Configuration**                                                        |          |      |                       |                                                                                                                                                                                                           |
| GPIO IRQ Route                                                                |          |      |                       |                                                                                                                                                                                                           |
| **SkyCam Configuration**                                                      |          |      |                       |                                                                                                                                                                                                           |
| SkyCam CIO2 Device                                                            |          |      |                       |                                                                                                                                                                                                           |
| Control Logic 0-3                                                             |          |      |                       |                                                                                                                                                                                                           |
| Link0-3                                                                       |          |      |                       |                                                                                                                                                                                                           |
| PORT-A HS-RXEN / TEM-EN Override                                              |          |      |                       |                                                                                                                                                                                                           |
| PORT-B HS-RXEN / TEM-EN Override                                              |          |      |                       |                                                                                                                                                                                                           |
| PORT-C HS-RXEN / TEM-EN Override                                              |          |      |                       |                                                                                                                                                                                                           |
| PORT-D HS-RXEN / TEM-EN Override                                              |          |      |                       |                                                                                                                                                                                                           |
| PORT-A CTLE                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PORT-B CTLE                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PORT-C/D CTLE                                                                 |          |      |                       |                                                                                                                                                                                                           |
| PORT-A CTLE CAP Value                                                         |          |      |                       |                                                                                                                                                                                                           |
| PORT-A CTLE RES Value                                                         |          |      |                       |                                                                                                                                                                                                           |
| PORT-B CTLE CAP Value                                                         |          |      |                       |                                                                                                                                                                                                           |
| PORT-B CTLE RES Value                                                         |          |      |                       |                                                                                                                                                                                                           |
| PORT-C/D CTLE CAP Value                                                       |          |      |                       |                                                                                                                                                                                                           |
| PORT-C/D CTLE RES Value                                                       |          |      |                       |                                                                                                                                                                                                           |
| PORT-A TRIM                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PORT-B TRIM                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PORT-C TRIM                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PORT-D TRIM                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PORT-A Data Trim Value                                                        |          |      |                       |                                                                                                                                                                                                           |
| PORT-B Data Trim Value                                                        |          |      |                       |                                                                                                                                                                                                           |
| PORT-C/D Data Trim Value                                                      |          |      |                       |                                                                                                                                                                                                           |
| PORT-A Clk Trim Value                                                         |          |      |                       |                                                                                                                                                                                                           |
| PORT-B Clk Trim Value                                                         |          |      |                       |                                                                                                                                                                                                           |
| PORT-C Clk Trim Value                                                         |          |      |                       |                                                                                                                                                                                                           |
| PORT-D Clk Trim Value                                                         |          |      |                       |                                                                                                                                                                                                           |
| **Link0-3**                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Sensor Model                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Lanes Clock division                                                          |          |      |                       |                                                                                                                                                                                                           |
| GPIO control                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Flash Support                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Privacy LED                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Rotation                                                                      |          |      |                       |                                                                                                                                                                                                           |
| PMIC Position                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Voltage Rail                                                                  |          |      |                       |                                                                                                                                                                                                           |
| LaneUsed                                                                      |          |      |                       |                                                                                                                                                                                                           |
| MCLK                                                                          |          |      |                       |                                                                                                                                                                                                           |
| EEPROM Type                                                                   |          |      |                       |                                                                                                                                                                                                           |
| VCM Type                                                                      |          |      |                       |                                                                                                                                                                                                           |
| Number of I2C Components                                                      |          |      |                       |                                                                                                                                                                                                           |
| I2C Channel                                                                   |          |      |                       |                                                                                                                                                                                                           |
| **Device 0-11**                                                               |          |      |                       |                                                                                                                                                                                                           |
| I2C Address                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Device Type                                                                   |          |      |                       |                                                                                                                                                                                                           |
| **Control Logic options**                                                     |          |      |                       |                                                                                                                                                                                                           |
| Control Logic Type                                                            |          |      |                       |                                                                                                                                                                                                           |
| CRD Version                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PMIC Flash Panel                                                              |          |      |                       |                                                                                                                                                                                                           |
| I2C Channel                                                                   |          |      |                       |                                                                                                                                                                                                           |
| I2C Address                                                                   |          |      |                       |                                                                                                                                                                                                           |
| WLED1 Type                                                                    |          |      |                       |                                                                                                                                                                                                           |
| WLED1 Flash Max Current                                                       |          |      |                       |                                                                                                                                                                                                           |
| WLED1 Torch Max Current                                                       |          |      |                       |                                                                                                                                                                                                           |
| WLED2 Type                                                                    |          |      |                       |                                                                                                                                                                                                           |
| WLED2 Flash Max Current                                                       |          |      |                       |                                                                                                                                                                                                           |
| WLED2 Torch Max Current                                                       |          |      |                       |                                                                                                                                                                                                           |
| Number of GPIO Pins                                                           |          |      |                       |                                                                                                                                                                                                           |
| **GPIO 0-3**                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Group Pad Number                                                              |          |      |                       |                                                                                                                                                                                                           |
| Group Number                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Function                                                                      |          |      |                       |                                                                                                                                                                                                           |
| Active Value                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Initial Value                                                                 |          |      |                       |                                                                                                                                                                                                           |
| **SCS Configuration**                                                         |          |      |                       |                                                                                                                                                                                                           |
| eMMC 5.0 Controller                                                           |          |      |                       |                                                                                                                                                                                                           |
| eMMC 5.0 HS400 Mode                                                           |          |      |                       |                                                                                                                                                                                                           |
| Driver Strength                                                               |          |      |                       |                                                                                                                                                                                                           |
| SDCard 3.0 Controller                                                         |          |      |                       |                                                                                                                                                                                                           |
| SDCard Sideband Events                                                        |          |      |                       |                                                                                                                                                                                                           |
| **ISH Configuration**                                                         |          |      |                       |                                                                                                                                                                                                           |
| ISH Controller                                                                |          |      |                       |                                                                                                                                                                                                           |
| **TraceHub Configuration Menu**                                               |          |      |                       |                                                                                                                                                                                                           |
| TraceHub Enable Mode                                                          |          |      |                       |                                                                                                                                                                                                           |
| MemRegion 0 Buffer Size                                                       |          |      |                       |                                                                                                                                                                                                           |
| MemRegion 1 Buffer Size                                                       |          |      |                       |                                                                                                                                                                                                           |
| **Pch Thermal Throttling Control**                                            |          |      |                       |                                                                                                                                                                                                           |
| Thermal Throttling Level                                                      |          |      |                       |                                                                                                                                                                                                           |
| T0 Level                                                                      |          |      |                       |                                                                                                                                                                                                           |
| T1 Level                                                                      |          |      |                       |                                                                                                                                                                                                           |
| T2 Level                                                                      |          |      |                       |                                                                                                                                                                                                           |
| DMI Thermal Setting                                                           |          |      |                       |                                                                                                                                                                                                           |
| Thermal Sensor 0 Width                                                        |          |      |                       |                                                                                                                                                                                                           |
| Thermal Sensor 1 Width                                                        |          |      |                       |                                                                                                                                                                                                           |
| Thermal Sensor 2 Width                                                        |          |      |                       |                                                                                                                                                                                                           |
| Thermal Sensor 3 Width                                                        |          |      |                       |                                                                                                                                                                                                           |
| SATA Thermal Setting                                                          |          |      |                       |                                                                                                                                                                                                           |
| **Port 0-1**                                                                  |          |      |                       |                                                                                                                                                                                                           |
| T1 Multipler                                                                  |          |      |                       |                                                                                                                                                                                                           |
| T2 Multipler                                                                  |          |      |                       |                                                                                                                                                                                                           |
| T3 Multipler                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Tdispatch                                                                     |          |      |                       |                                                                                                                                                                                                           |
| Tinactive                                                                     |          |      |                       |                                                                                                                                                                                                           |
| Tdispatch                                                                     |          |      |                       |                                                                                                                                                                                                           |
| Tinactive                                                                     |          |      |                       |                                                                                                                                                                                                           |
| **Boot**                                                                      |          |      |                       |                                                                                                                                                                                                           |
| Setup Prompt Timeout                                                          |          |      |                       | 进入系统前BIOS等待按Delete的时间                                                                                                                                                                                     |
| Bootup NumLock State                                                          |          |      |                       | 开机时开启小键盘                                                                                                                                                                                                  |
| Quiet Boot                                                                    |          |      |                       |                                                                                                                                                                                                           |
| Fast Boot                                                                     |          |      | Disabled              | 快速启动                                                                                                                                                                                                      |
| SATA Support                                                                  |          |      |                       |                                                                                                                                                                                                           |
| VGA Support                                                                   |          |      |                       |                                                                                                                                                                                                           |
| USB Support                                                                   |          |      |                       |                                                                                                                                                                                                           |
| PS2 Devices Support                                                           |          |      |                       | PS2设备支持                                                                                                                                                                                                   |
| NetWork Stack Driver Support                                                  |          |      |                       |                                                                                                                                                                                                           |
| Redirection Support                                                           |          |      |                       |                                                                                                                                                                                                           |
| **Advanced**                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Internal Pointing Device                                                      |          |      |                       |                                                                                                                                                                                                           |
| Wake On Lid Open                                                              |          |      |                       |                                                                                                                                                                                                           |
| Intel Virtualization Technology                                               |          |      |                       | CPU会开放一些CPU缓存接口来提升虚拟机的IO性能，但会降低CPU缓存性能                                                                                                                                                                    |
| Intel AES-NI                                                                  |          |      |                       |                                                                                                                                                                                                           |
| VT-d                                                                          |          |      |                       |                                                                                                                                                                                                           |
| **Network Stack Configuration**                                               |          |      |                       |                                                                                                                                                                                                           |
| Network Stack                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Ipv4 PXE Support                                                              |          |      |                       |                                                                                                                                                                                                           |
| Ipv6 PXE Support                                                              |          |      |                       |                                                                                                                                                                                                           |
| **USB Configuration**                                                         |          |      |                       |                                                                                                                                                                                                           |
| Legacy USB Support                                                            |          |      |                       |                                                                                                                                                                                                           |
| USB Mass Storage Driver Support                                               |          |      |                       |                                                                                                                                                                                                           |
| **Trusted Computing**                                                         |          |      |                       |                                                                                                                                                                                                           |
| Security Device Support                                                       |          |      |                       |                                                                                                                                                                                                           |
| Pending operation                                                             |          |      |                       |                                                                                                                                                                                                           |
| **Graphics Configuration**                                                    |          |      |                       |                                                                                                                                                                                                           |
| DVMT Pre-Allocated                                                            |          |      |                       |                                                                                                                                                                                                           |
| **SATA Configuration**                                                        |          |      |                       |                                                                                                                                                                                                           |
| SATA Mode Selection                                                           |          |      |                       |                                                                                                                                                                                                           |
| **Boot**                                                                      |          |      |                       |                                                                                                                                                                                                           |
| Launch PXE OpROM policy                                                       |          |      |                       | 运行预启动执行环境兼容支持                                                                                                                                                                                             |
| **I/O Interface Security**                                                    |          |      |                       |                                                                                                                                                                                                           |
| Wireless Network Interface                                                    |          |      |                       |                                                                                                                                                                                                           |
| LAN Network Interface                                                         |          |      |                       |                                                                                                                                                                                                           |
| SATA ODD Interface                                                            |          |      |                       |                                                                                                                                                                                                           |
| HD Audio Interface                                                            |          |      |                       |                                                                                                                                                                                                           |
| **USB Interface Security**                                                    |          |      |                       |                                                                                                                                                                                                           |
| USB Interface                                                                 |          |      |                       |                                                                                                                                                                                                           |
| External Ports                                                                |          |      |                       |                                                                                                                                                                                                           |
| BlueTooth                                                                     |          |      |                       |                                                                                                                                                                                                           |
| CMOS Camera                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Card Reader                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Smart Card Reader                                                             |          |      |                       |                                                                                                                                                                                                           |
| Finger Print                                                                  |          |      |                       |                                                                                                                                                                                                           |
| LTE                                                                           |          |      |                       |                                                                                                                                                                                                           |
| **Secure Boot**                                                               |          |      |                       |                                                                                                                                                                                                           |
| Secure Boot Control                                                           |          |      |                       |                                                                                                                                                                                                           |
| **PCI Subsystem Settings**                                                    |          |      |                       |                                                                                                                                                                                                           |
| Above 4G Decoding                                                             |          |      |                       |                                                                                                                                                                                                           |
| Hot-Plug Support                                                              |          |      |                       |                                                                                                                                                                                                           |
| **Trusted Computing**                                                         |          |      |                       |                                                                                                                                                                                                           |
| Security Device Support                                                       |          |      |                       |                                                                                                                                                                                                           |
| **SMART Settings**                                                            |          |      |                       |                                                                                                                                                                                                           |
| SMART Self Test                                                               |          |      |                       |                                                                                                                                                                                                           |
| **Memory Configuration**                                                      |          |      |                       |                                                                                                                                                                                                           |
| MRC ULT Safe Config                                                           |          |      |                       |                                                                                                                                                                                                           |
| Maximum Memory Frequency                                                      |          |      |                       | 最大内存工作频率                                                                                                                                                                                                  |
| HOB Buffer Size                                                               |          |      |                       |                                                                                                                                                                                                           |
| ECC Support                                                                   |          |      |                       |                                                                                                                                                                                                           |
| Max TOLUD                                                                     |          |      |                       |                                                                                                                                                                                                           |
| SA GV                                                                         |          |      |                       |                                                                                                                                                                                                           |
| SA GV Low Freq                                                                |          |      |                       |                                                                                                                                                                                                           |
| Retrain on Fast Fail                                                          |          |      |                       |                                                                                                                                                                                                           |
| Command Tristate                                                              |          |      |                       |                                                                                                                                                                                                           |
| Enable RH Prevention                                                          |          |      |                       |                                                                                                                                                                                                           |
| Row Hammer Solution                                                           |          |      |                       |                                                                                                                                                                                                           |
| RH Activation Probability                                                     |          |      |                       |                                                                                                                                                                                                           |
| Exit On Failure (MRC)                                                         |          |      |                       | 退出时显示失败信息                                                                                                                                                                                                 |
| MC Lock                                                                       |          |      |                       |                                                                                                                                                                                                           |
| Probeless Trace                                                               |          |      |                       |                                                                                                                                                                                                           |
| GDXC IOT size                                                                 |          |      |                       |                                                                                                                                                                                                           |
| GDXC MOT size                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Memory Trace                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Enable/Disable IED (Intel Enhanced Debug)                                     |          |      |                       |                                                                                                                                                                                                           |
| Ch Hash Support                                                               |          |      |                       |                                                                                                                                                                                                           |
| Ch Hash Mask                                                                  |          |      |                       |                                                                                                                                                                                                           |
| Ch Hash Interleaved Bit                                                       |          |      |                       |                                                                                                                                                                                                           |
| VC1 Read Metering                                                             |          |      |                       |                                                                                                                                                                                                           |
| VC1 RdMeter Time Window                                                       |          |      |                       |                                                                                                                                                                                                           |
| VC1 RdMeter Threshold                                                         |          |      |                       |                                                                                                                                                                                                           |
| Strong Weak Leaker                                                            |          |      |                       |                                                                                                                                                                                                           |
| Memory Scrambler                                                              |          |      |                       |                                                                                                                                                                                                           |
| Force ColdReset                                                               |          |      |                       |                                                                                                                                                                                                           |
| Channel A DIMM Control                                                        |          |      |                       |                                                                                                                                                                                                           |
| Channel B DIMM Control                                                        |          |      |                       |                                                                                                                                                                                                           |
| Force Single Rank                                                             |          |      |                       |                                                                                                                                                                                                           |
| Memory Remap                                                                  | 0x8A7    | 0x1  | Enabled               | 支持64位系统重新指派内存地址                                                                                                                                                                                           |
| Time Measure                                                                  |          |      |                       |                                                                                                                                                                                                           |
| DLL Weak Lock Support                                                         |          |      |                       |                                                                                                                                                                                                           |
| Pwr Down Idle Timer                                                           |          |      |                       |                                                                                                                                                                                                           |
| Mrc Fast Boot                                                                 |          |      |                       |                                                                                                                                                                                                           |
| Lpddr Mem WL Set                                                              |          |      |                       |                                                                                                                                                                                                           |
| EV Loader                                                                     |          |      |                       |                                                                                                                                                                                                           |
| EV Loader Delay                                                               |          |      |                       |                                                                                                                                                                                                           |
| **Memory Training Algorithms**                                                |          |      |                       |                                                                                                                                                                                                           |
| Early Command Training                                                        |          |      |                       |                                                                                                                                                                                                           |
| SenseAmp Offset Training                                                      |          |      |                       |                                                                                                                                                                                                           |
| Early ReadMPR Timing Centering 2D                                             |          |      |                       |                                                                                                                                                                                                           |
| Read MPR Training                                                             |          |      |                       |                                                                                                                                                                                                           |
| Receive Enable Training                                                       |          |      |                       |                                                                                                                                                                                                           |
| Jedec Write Leveling                                                          |          |      |                       |                                                                                                                                                                                                           |
| Early Write Time Centering 2D                                                 |          |      |                       |                                                                                                                                                                                                           |
| Early Write Drive Strength/Equalization                                       |          |      |                       |                                                                                                                                                                                                           |
| Early Read Time Centering 2D                                                  |          |      |                       |                                                                                                                                                                                                           |
| Write Timing Centering 1D                                                     |          |      |                       |                                                                                                                                                                                                           |
| Write Voltage Centering 1D                                                    |          |      |                       |                                                                                                                                                                                                           |
| Read Timing Centering 1D                                                      |          |      |                       |                                                                                                                                                                                                           |
| Dimm ODT Training*                                                            |          |      |                       |                                                                                                                                                                                                           |
| Max RTT_WR                                                                    |          |      |                       |                                                                                                                                                                                                           |
| DIMM RON Training*                                                            |          |      |                       |                                                                                                                                                                                                           |
| Write Drive Strength/Equalization 2D*                                         |          |      |                       |                                                                                                                                                                                                           |
| Write Slew Rate Training*                                                     |          |      |                       |                                                                                                                                                                                                           |
| Read ODT Training*                                                            |          |      |                       |                                                                                                                                                                                                           |
| Read Equalization Training*                                                   |          |      |                       |                                                                                                                                                                                                           |
| Read Amplifier Training*                                                      |          |      |                       |                                                                                                                                                                                                           |
| Write Timing Centering 2D                                                     |          |      |                       |                                                                                                                                                                                                           |
| Read Timing Centering 2D                                                      |          |      |                       |                                                                                                                                                                                                           |
| Command Voltage Centering                                                     |          |      |                       |                                                                                                                                                                                                           |
| Write Voltage Centering 2D                                                    |          |      |                       |                                                                                                                                                                                                           |
| Read Voltage Centering 2D                                                     |          |      |                       |                                                                                                                                                                                                           |
| Late Command Training                                                         |          |      |                       |                                                                                                                                                                                                           |
| Round Trip Latency                                                            |          |      |                       |                                                                                                                                                                                                           |
| Turn Around Timing Training                                                   |          |      |                       |                                                                                                                                                                                                           |
| Rank Margin Tool                                                              |          |      |                       |                                                                                                                                                                                                           |
| Memory Test                                                                   |          |      |                       |                                                                                                                                                                                                           |
| DIMM SPD Alias Test                                                           |          |      |                       |                                                                                                                                                                                                           |
| Receive Enable Centering 1D                                                   |          |      |                       |                                                                                                                                                                                                           |
| Retrain Margin Check                                                          |          |      |                       |                                                                                                                                                                                                           |
| Write Drive Strength Up/Dn independently                                      |          |      |                       |                                                                                                                                                                                                           |
| CMD Slew Rate Training                                                        |          |      |                       |                                                                                                                                                                                                           |
| CMD Drive Strength / Tx Equalization                                          |          |      |                       |                                                                                                                                                                                                           |
| CMD Normalization                                                             |          |      |                       |                                                                                                                                                                                                           |
| **Memory Thermal Configuration**                                              |          |      |                       |                                                                                                                                                                                                           |
| Memory Thermal Management                                                     |          |      |                       |                                                                                                                                                                                                           |
| PECI Injected Temperature                                                     |          |      |                       |                                                                                                                                                                                                           |
| EXTTS# via TS-on-Board                                                        |          |      |                       |                                                                                                                                                                                                           |
| EXTTS# via TS-on-DIMM                                                         |          |      |                       |                                                                                                                                                                                                           |
| Virtual Temperature Sensor (VTS)                                              |          |      |                       |                                                                                                                                                                                                           |
| **Memory Power and Thermal Throttling**                                       |          |      |                       |                                                                                                                                                                                                           |
| DDR PowerDown and idle counter                                                |          |      |                       |                                                                                                                                                                                                           |
| For LPDDR Only: DDR PowerDown and idle counter                                |          |      |                       |                                                                                                                                                                                                           |
| REFRESH_2X_MODE                                                               |          |      |                       |                                                                                                                                                                                                           |
| LPDDR Thermal Sensor                                                          |          |      |                       |                                                                                                                                                                                                           |
| SelfRefresh Enable                                                            |          |      |                       |                                                                                                                                                                                                           |
| SelfRefresh IdleTimer                                                         |          |      |                       |                                                                                                                                                                                                           |
| Throttler CKEMin Defeature                                                    |          |      |                       |                                                                                                                                                                                                           |
| Throttler CKEMin Timer                                                        |          |      |                       |                                                                                                                                                                                                           |
| For LPDDR Only: Throttler CKEMin Defeature                                    |          |      |                       |                                                                                                                                                                                                           |
| For LPDDR Only: Throttler CKEMin Timer                                        |          |      |                       |                                                                                                                                                                                                           |
| **Dram Power Meter**                                                          |          |      |                       |                                                                                                                                                                                                           |
| Use user provided power weights, scale factor, and channel power floor values |          |      |                       |                                                                                                                                                                                                           |
| Energy Scale Factor                                                           |          |      |                       |                                                                                                                                                                                                           |
| Idle Energy Ch0/1Dimm0/1                                                      |          |      |                       |                                                                                                                                                                                                           |
| PowerDown Energy Ch0/1Dimm0/1                                                 |          |      |                       |                                                                                                                                                                                                           |
| Activate Energy Ch0/1Dimm0/1                                                  |          |      |                       |                                                                                                                                                                                                           |
| Read Energy Ch0/1Dimm0/1                                                      |          |      |                       |                                                                                                                                                                                                           |
| Write Energy Ch/10Dimm0/1                                                     |          |      |                       |                                                                                                                                                                                                           |
| **Memory Thermal Reporting**                                                  |          |      |                       |                                                                                                                                                                                                           |
| Lock Thermal Management Registers                                             |          |      |                       |                                                                                                                                                                                                           |
| Extern Therm Status                                                           |          |      |                       |                                                                                                                                                                                                           |
| Closed Loop Therm Manage                                                      |          |      |                       |                                                                                                                                                                                                           |
| Open Loop Therm Manage                                                        |          |      |                       |                                                                                                                                                                                                           |
| Warm Threshold Ch0 Dimm0                                                      |          |      |                       |                                                                                                                                                                                                           |
| Warm Threshold Ch0 Dimm1                                                      |          |      |                       |                                                                                                                                                                                                           |
| Hot Threshold Ch0 Dimm0                                                       |          |      |                       |                                                                                                                                                                                                           |
| Hot Threshold Ch0 Dimm1                                                       |          |      |                       |                                                                                                                                                                                                           |
| Warm Threshold Ch1 Dimm0                                                      |          |      |                       |                                                                                                                                                                                                           |
| Warm Threshold Ch1 Dimm1                                                      |          |      |                       |                                                                                                                                                                                                           |
| Hot Threshold Ch1 Dimm0                                                       |          |      |                       |                                                                                                                                                                                                           |
| Hot Threshold Ch1 Dimm1                                                       |          |      |                       |                                                                                                                                                                                                           |
| Warm Budget Ch0 Dimm0                                                         |          |      |                       |                                                                                                                                                                                                           |
| Warm Budget Ch0 Dimm1                                                         |          |      |                       |                                                                                                                                                                                                           |
| Hot Budget Ch0 Dimm0                                                          |          |      |                       |                                                                                                                                                                                                           |
| Hot Budget Ch0 Dimm1                                                          |          |      |                       |                                                                                                                                                                                                           |
| Warm Budget Ch1 Dimm0                                                         |          |      |                       |                                                                                                                                                                                                           |
| Warm Budget Ch1 Dimm1                                                         |          |      |                       |                                                                                                                                                                                                           |
| Hot Budget Ch1 Dimm0                                                          |          |      |                       |                                                                                                                                                                                                           |
| Hot Budget Ch1 Dimm1                                                          |          |      |                       |                                                                                                                                                                                                           |
| **Memory RAPL**                                                               |          |      |                       |                                                                                                                                                                                                           |
| Rapl Power Floor Ch0                                                          |          |      |                       |                                                                                                                                                                                                           |
| Rapl Power Floor Ch1                                                          |          |      |                       |                                                                                                                                                                                                           |
| RAPL PL Lock                                                                  |          |      |                       |                                                                                                                                                                                                           |
| RAPL PL 1 enable                                                              |          |      |                       |                                                                                                                                                                                                           |
| RAPL PL 1 Power                                                               |          |      |                       |                                                                                                                                                                                                           |
| RAPL PL 1 WindowX                                                             |          |      |                       |                                                                                                                                                                                                           |
| RAPL PL 1 WindowY                                                             |          |      |                       |                                                                                                                                                                                                           |
| RAPL PL 2 enable                                                              |          |      |                       |                                                                                                                                                                                                           |
| RAPL PL 2 Power                                                               |          |      |                       |                                                                                                                                                                                                           |
| RAPL PL 2 WindowX                                                             |          |      |                       |                                                                                                                                                                                                           |
| RAPL PL 2 WindowY                                                             |          |      |                       |                                                                                                                                                                                                           |
|                                                                               |          |      |                       |                                                                                                                                                                                                           |
| CPU Fan Speed                                                                 | 0x2C4    | 0x64 | Maximum               | 风扇转速                                                                                                                                                                                                      |
| FAN1 Device                                                                   | 0x2EA    | 0x1  | Enabled               |                                                                                                                                                                                                           |
| FAN2 Device                                                                   | 0x2EB    | 0x1  | Enabled               |                                                                                                                                                                                                           |
| Generic Device 4                                                              | 0x371    | 0x1  | Enabled               | 开启第四个核心                                                                                                                                                                                                   |

# 相关下载

## XTU

```
https://downloadcenter.intel.com/zh-cn/download/24075/Intel-Extreme-Tuning-Utility-Intel-XTU-
```

## ThrottleStop

```
https://www.techpowerup.com/download/techpowerup-throttlestop/
```

## AppleIntelInfo.kext

```
https://github.com/Piker-Alpha/AppleIntelInfo
```

## Phoenix Flasher (DOS based, phlash16.exe)

```
https://www.wimsbios.com/phoenixflasher.jsp
```

## BIOS工具包

```
https://www.bios-mods.com/downloads/
```

# 参考教程

## TDP and turbo parameter modification with MSR on non-overclockable CPU

```
https://gist.github.com/Mnkai/5a8edd34bd949199224b33bd90b8c3d4
```

## BIOS用户指南

```
http://www.h3c.com/cn/d_201801/1055806_30005_0.htm#_Toc502671328
```

## Unlock Hidden menu in Phoenix BIOS Setup Menu Tutorial

```
http://forum.notebookreview.com/attachments/unlock-hidden-menu-in-phoenix-bios-setup-menu-tutorial-zip.62866/
```

## How to Enable Intel VT and AHCI on a Napa/Santa Rosa platform Phoenix BIOS Vaio laptop

```
http://forum.notebookreview.com/sony/189228-how-enable-intel-vt-ahci-napa-santa- rosa-platform-phoenix-bios-vaio-laptop.html#post2678924
```

## Acer Laptop with Phoenix BIOS: Enable Virtualization (Test Machine: Acer Aspire 9420)

```
http://forum.notebookreview.com/acer/465936-acer-laptop-phoenix-bios-enable- virtualization-test-machine-acer-aspire-9420-a.html#post5991508
```

## LongSoft/UEFITool

```
https://github.com/LongSoft/UEFITool
```
