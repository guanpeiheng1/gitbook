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

# 资料

## 论坛

```
http://www.smxdiy.com/
https://www.bios-mods.com/
```

## 其它

```
https://www.cnblogs.com/simadi/p/4357057.html
https://www.win-raid.com/t154f16-Tool-Guide-News-quot-UEFI-BIOS-Updater-quot-UBU.html
```

# 常用工具

## BIOS Backup TooKit

BIOS备份。

```
https://www.jb51.net/softs/193820.html
```

## DMIScope

查看BIOS信息。

```
http://www.xz7.com/downinfo/495965.html
```

## RW

查看BIOS信息。

```
http://www.uzzf.com/soft/86050.html
```

## BIOS工具包

```
https://www.bios-mods.com/downloads/
```

## Core Temp

查看CPU各核温度。

```
https://www.alcpu.com/CoreTemp/
```

## CPU-Z

查看系统硬件信息。

## GPU-Z

查看显卡信息。

## BurnInTest

电脑稳定性测试工具。

## Kombustor

显卡测试拷机工具。

## Furmark

测试显卡性能。

## Thaiphoon

读取内存参数。

```
http://www.softnology.biz/files.html
```

打开软件并点击Read获取SPD信息，选择内存后点击Report，然后直接页面拉到最下面，点击Show delays in nanoseconds。点击Export，选择Complete HTML Report，即可生成包含内存信息的配置文件。

# BIOS降级

## 华硕

华硕主板在Windows下可用WinFlash进行BIOS刷写操作，但只能升级而不能降级。WinFlash下载链接如下。

```
https://www.pcsoft.com.cn/soft/166235.html
```

为绕过限制，可新建WinFlash的快捷方式，在快捷方式下右键点击属性，在目标一栏的最后加上参数`/nodate`，再双击打开快捷方式即可。注意，此法只能降级官方BIOS而不能刷修改版BIOS。

# UEFI BIOS

以本机华硕笔记本为例，类型为AMI BIOS。

## 制作EFI Shell与grub引导盘

准备一个U盘，格式化为FAT32格式。打开以下链接，将整个项目Download下来后解压，将所有内容复制到U盘内。

```
https://github.com/holoto/efi_shell_flash_bios
```

打开以下链接并下载modGRUBShell.efi，改名为bootx64.efi，并复制到EFI/BOOT/grub efi shell for setup_var替换。

```
https://github.com/datasone/grub-mod-setup_var

# 或以下链接
http://brains.by/posts/bootx64.7z
```

打开EFI/BOOT，把`efi shell for flash bios`目录里的文件复制到此目录即可启动EFI Shell，把 `grub efi shell for setup_var`目录里的文件复制到此目录即可启动grub。

重启电脑，在BIOS里选择U盘启动即可。

## 修改隐藏设置

需要先从官网获得BIOS固件。

### 提取条目

下载UEFITool，链接如下。

```
https://github.com/LongSoft/UEFITool
```

用UEFITool打开原BIOS，通过搜索功能搜索BIOS Lock或DVMT，注意搜索框需切换到`Text`选项卡。在搜索结果所指向的PE32模块点击右键（一般并非整个GUID），选择Extract body，保存为bin文件，完成提取。

下载IFR Extractor，链接如下。

```
https://www.appgao.com/files/288.html
https://github.com/LongSoft/Universal-IFR-Extractor
```

用IFR Extractor打开所提取的文件，保存为txt。打开txt后可以看到BIOS所有可调整的条目及选项，现以BIOS Lock条目说明如下。

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

注意如果是如下的`Numeric`形式，则表明此条目为填写数字的内容，该地址的值在Min和Max之间即可。

```
Numeric: Wake up hour, VarStoreInfo (VarOffset/VarName): 0xF, VarStore: 0x1, QuestionId: 0x30, Size: 1, Min: 0x0, Max 0x17, Step: 0x1 {07 91 DC 00 DD 00 30 00 01 00 0F 00 10 10 00 17 01}
                 Default: DefaultId: 0x0, Value (8 bit): 0x0 {5B 06 00 00 00 00}
             End {29 02}
```

对于该txt的内容，Suppress If语句用于禁用子菜单，True和False分别被编码为46 02和47 02。

### 修改条目

从U盘进入grub，输入以下命令以修改。下面以BIOS Lock为例。

```
setup_var_3 0x8A6 // 查找BIOS Lock地址
setup_var_3 0x8A6 0x0 // 更改BIOS Lock地址值为0x0[Disabled]
```

### 常见问题

#### 无法修改条目

```
https://habr.com/en/post/190354/
```

## 修改启动固件

### 修改文件

#### 通过AFUWIN

下载AFUWIN，链接如下。打开AFUWIN，点击Save提取BIOS。

```
http://www.winwin7.com/soft/xtgj-8140.html
```

下载AMIBCP，链接如下。

```
https://www.onlinedown.net/soft/1114140.htm
```

用AMIBCP打开所提取的rom文件，左侧目录栏切换到Setup。在Setup内部为BIOS的具体条目，将需要显示的条目由Default改为Supervisor或USER，修改完成后保存为新的rom文件。对于本机而言，有两个Setup菜单，其中第二个才是华硕的UEFI BIOS菜单，且需要修改为Supervisor才能使显示生效。

#### 通过UEFITool

用UEFITool打开从官网下载的BIOS固件，搜索Setup，找到固件的Setup DXE。在其上点击右键，选择`Extract body...`并保存为setup.bin。

以二进制文件方式打开setup.bin，搜索`01 01 00 01 01 01`。若存在，则证明有菜单被隐藏，将00改为01即可。

找到需要修改的条目，True为46 02，False为47 02，修改为对应选项。完成后保存，回到UEFITool，右键选择`Replace body...`，选择刚才修改好的文件，另存为新的BIOS固件即可。

### 刷入文件

将修改好的BIOS修改为bios.rom并放入U盘根目录。从U盘进入EFI Shell，输入以下命令以备份并刷写BIOS。

```
map // 列出所有硬盘设备
fs0: // 选择U盘（根据上面的设备编号输入）
ls // 列出U盘内容
Fpt.efi -d backup.rom -bios // 备份BIOS到backup.rom
Fpt.efi -f bios.rom -bios // 刷写bios.rom
```

若提示Error 368错误，则需先按照上面grub修改BIOS设置的流程，将BIOS Lock设为Disabled。注意，本机刷入修改版的BIOS后并无作用。

## 手动删减引导条目

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

# Phoenix BIOS

## 显示隐藏菜单

### 检查长度

下载Phoenix BIOS Editor以及要修改的原版Phoenix BIOS文件。Phoenix BIOS Editor下载链接如下。

```
https://dl.pconline.com.cn/download/821568-1.html
```

用Phoenix BIOS Editor打开BIOS文件，若提示Invaild rom length，则用十六进制编辑器打开BIOS文件，把00100010h至结尾的内容清除后保存即可，这一部分是BIOS文件的附加信息。

### 提取模块

打开BIOS文件后进入C:\Program Files (x86)\Phoenix Technologies Ltd\BIOS Editor\TEMP目录，复制OLD1.RLS、STRINGS0.ROM（或STRINGS00.ROM）、TEMPLAT0.ROM（或TEMPLAT00.ROM）到任意位置。

用十六进制编辑器打开以上复制出来的文件，复制OLD1.RLS的头两个字节（如D6 70）并在STRING0.ROM中查找这两个HEX值，取搜索结果较前的一个。在STRING0.ROM中，这两个字节之前（不含）的被称为header，长度应该为0x1C。删掉header并保存为STRING.ROM。

### 查找菜单位置

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

将81 03按小端规则处理后可知Token ID为0381，是BIOS中开关此功能的寄存器，可用于在不刷入修改版BIOS的情况下直接修改条目的值。

包含menu header后字符串的地址为0x000012D5，至此已找到所需菜单的位置，记录此地址。

### 放置菜单

在Phoenix BIOS Editor的Setup Table窗口中点击Emulate，显示模拟BIOS。有些菜单尽管在模拟器中可见，刷入后却不可见，对于这些菜单，最好的方法是将其移至其它有空位的标签页中。

记录原标签页的排序，经查看后可知在Main标签页中有空位，则在STRINGS.ROM中搜索Main，注意需要全匹配。记录字符串的地址，忽略高四位，将地址按小端规则重新排列，并作为HEX值在STRINGS.ROM中搜索，取结果的地址值。

忽略高四位，将地址按小端规则重新排列，并作为HEX值在TEMPLAT0.ROM中搜索，摘录它们的前两个字节，即为menu header，理论上种类为10（Generic Text）。包含menu header后取字符串地址，这里为0x00000AA1。


忽略高四位，将地址按小端规则重新排列，并作为HEX值在TEMPLAT0.ROM中搜索，此处为A1 0A。查看前面所记录的标签页的顺序，此处为Main-Advanced-Security-Boot-Exit，Main为第一个标签页，其HEX值为A1 0A，故Advanced及以后的标签页的HEX值为从Main开始往后取。每个标签页占两个字节，且每个标签页之间相距两个字节。取Main的前两个字节、Main本身以及Main后的20个字节，如下所示。

| 地址       | HEX值                                           |
| ---------- | ----------------------------------------------- |
| 0x0000015A | 00 00 A1 0A 74 01 A5 09 B8 01 D3 0A 04 02 3F 0B |
| 0x0000016A | 5E 04 17 0A 6E 02 00 00                         |

Main的HEX值为A1 0A，其余分别为Advanced（A5 09），Security（D3 0A），Boot（3F 0B），Exit（17 0A）。每个菜单HEX值后的两个字节表示这一菜单对应的地址，对于Main而言为74 01。跳到地址0x00000174，此处即为Main选项卡中包含的所有菜单。

留意到Advanced选项卡的地址为0x000001B8，则Main选项卡的菜单不能超出该位置。在选项卡内部，每条菜单用两个字节表示，这两个字节为该菜单的地址，每条菜单之间用00 00分隔。如果两条菜单之间用00 00 00 00分隔（不小于四组00），则00 00 00 00后的菜单将不可见。因此，直接向00 00 00 00处填充菜单即可解锁新菜单。注意，每组选项卡之间有00 00 00 00作为分隔。

为验证所找到的位置是否正确，可以看到该选项卡的首两个字节为E9 0A，对应的菜单应该是System Time。跳到地址0x00000AE9，可以看到首两个字节为21 0A，为菜单的menu header，其中21表示时间，故所找到的位置是正确的。

在标签页内寻找可以插入的位置，一般为00 00 00 00，替换前面的00 00即可。前面需要解锁的菜单地址为0x000012D5，忽略高四位，将地址按小端规则重新排列并填入，变为D5 12 00 00即可。注意，修改时不能改变文件大小，即只能替换而不能删除文件中的任何一个字节。如果没有空位，可以替换部分平时少用的菜单。

### 打包固件

将需要解锁的菜单都写入后，保存TEMPLAT0.ROM文件，并复制到C:\Program Files (x86)\Phoenix Technologies Ltd\BIOS Editor\TEMP覆盖。回到Phoenix BIOS Editor，由于没有在其界面中操作，此时并不能保存文件。故在Setup Table中点击Main选项卡，任意双击一个`*`号，此时已经可以保存，点击保存即可。

注意，每个BIOS只能在原版的基础上修改一次，在已经修改过的BIOS文件中不能二次修改，否则Phoenix BIOS Editor会报错。

## 刷入修改固件

注意，理论上Phoenix BIOS的后缀名可以改为任意名称，因此可以直接把BIOS.rom改为BIOS.wph。所需软件的下载链接如下。

```
https://www.wimsbios.com/phoenixflasher.jsp
```

### 通过Phoenix Secure WinFlash

直接按照软件操作即可。

### 通过phlash16

phlash16为DOS下的Phoenix BIOS刷写工具，进入DOS环境后通过以下命令刷写即可，其中bios.rom为要刷写的BIOS。由于E2ROM寄存器才可被刷写，因此提示Invaild flashint.bin说明该BIOS无法被刷入。关于phlash16的具体参数，可通过输入`phlash16 /?`查询。

```
phlash16 bios.rom /x /s /c /mfg /mode=3
```

## 修改隐藏条目

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

# CPU

## 查看被限制性能的处理器

在Windows下打开事件查看器，选择Windows日志-系统，然后点击右侧栏的`筛选当前日志`，类型选择Kernel-Processor-Power即可。

## 超频

对于CPU，BCLK乘以倍增系数即为内核频率。BCLK即基本时钟速度，一般为100MHz。倍增系数即倍频，随CPU变化。

如果CPU未锁定倍频，可直接超频。若CPU锁定了倍频，可通过提高主板FSB速度，或屏蔽CPU部分针脚实现超频。

对于Intel，名称末尾带K或X的CPU表明该CPU未锁频。超频前建议将CPU各核设为相等的频率。

### 通过BIOS

一般提高CPU Core Ratio即可。

### 通过软件

#### XTU

下载链接如下。

```
https://downloadcenter.intel.com/zh-cn/download/24075/Intel-Extreme-Tuning-Utility-Intel-XTU-
```

安装完成后打开，点击Basic Tuning，向右调整两个滑块即可，注意缓存倍频如果低于内核倍频，CPU 性能会降低。建议每次上调一倍，以防止电脑死机。完成设定后用Stress Test进行压力测试，查看CPU能否稳定运行。

若需要更精细的调整，可点击左边Advanced Tuning，会出现提示警告。点击`I agree,don't show again`即可打开高级选项。

一般调节Turbo Boost Short Power Max、Turbo Boost Power Max、Turbo Boost Power Time Window，控制温度使用Core Voltage Offset。

|                   项目                  |        中文名        |                                                                        注释                                                                        |                                                                                                                                                                                    调整方法                                                                                                                                                                                    |
|-----------------------------------------|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Core**                                |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Refrence Clock                          | 外频                 |                                                                                                                                                    | 保持Default                                                                                                                                                                                                                                                                                                                                                                    |
| Max Non Turbo Boost Ratio               | 默频                 | 最大非睿频下倍频                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                |
| Core Voltage Mode                       | CPU电压模式          | 分为 Adaptive（自适应）和 Static（静态）                                                                                                           | 保持默认的Adaptive                                                                                                                                                                                                                                                                                                                                                             |
| Core Voltage                            | CPU电压              | 设置CPU的绝对电压                                                                                                                                  | 保持Default                                                                                                                                                                                                                                                                                                                                                                    |
| Core Voltage Offset                     | CPU偏移电压          | 相对于绝对电压偏移（增大或减小）的电压                                                                                                             | 降压的Offset是负值。建议以0.01V（10mV）为单位降压，先调整Offset至-0.01V，然后转至Stress Test，勾选CPU Stress Test并测试10分钟，若无异常继续降0.01V。在降压0.05V之后以0.005V（5mV）为单位进行降压，并且每次降压之后应该至少有20分钟的P95或者FPU的测试并且保证显卡有负载（比如GPUZ的Render Test或者甜甜圈）。若意外关机，则把最后一次降压幅度减小0.01V，并测试两小时，无异常即可 |
| Enhanced Intel SpeedStep Technology     | 电源管理相关         | 控制CPU电压和频率关系，使得不同场景下CPU可以在高性能和电池优化模式中来回切换                                                                       |                                                                                                                                                                                                                                                                                                                                                                                |
| Intel Turbo Boost Technology            | 睿频加速技术         |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Core IccMax                   | 核心最大电流         | 调节核心电流                                                                                                                                       | 保持Default                                                                                                                                                                                                                                                                                                                                                                    |
| AVX Ratio Offset                        | AVX倍频补偿          | 控制CPU跑AVX指令集（比如浮点运算）的最大频率。比如设置x3的AVX频率补偿，倍频45x外频100MHz，那么在浮点运算的时候，CPU的主频只会到（45-3）*100=4.2GHz | 保持Default                                                                                                                                                                                                                                                                                                                                                                    |
| Turbo Boost Short Power Max Enable      | 短时间超功耗睿频模式 | 可以让CPU短时间睿频到一个超过标定频率很多的一个频率，但代价是温度高，而且CPU可能承受不住太长时间                                                   |                                                                                                                                                                                                                                                                                                                                                                                |
| Turbo Boost Short Power Max             | 短时间最大睿频功耗   | PL2，决定瞬间性能（在瞬间负载下这个数值会限制CPU的功耗）                                                                                           | 拉至最大                                                                                                                                                                                                                                                                                                                                                                       |
| Turbo Boost Power Max                   | 长时间睿频最大功耗   | PL1，决定稳定性能（PL1数值上肯定不会超过PL2）                                                                                                      | 拉至最大                                                                                                                                                                                                                                                                                                                                                                       |
| Turbo Boost Power Time Window           | 睿频加速时间         | PL2时间                                                                                                                                            | 拉至最大                                                                                                                                                                                                                                                                                                                                                                       |
| Turbo Boost Short Power Time Windows    | 睿频最大功耗持续时间 | PL1时间（在这个此时间内，CPU功耗可以突破PL1但不高于PL2）                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                |
| Multipliers                             | 多核倍频             | 超频专用                                                                                                                                           | 拉至最大                                                                                                                                                                                                                                                                                                                                                                       |
| **Cache**                               |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Cache Voltage Mode                      | 缓存电压模式         |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Cache Voltage                           | 缓存绝对电压         |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Cache Voltage Offset                    | 缓存偏移电压         |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Cacha IccMac                            |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| **Graphics**                            |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Precessor Graphics Ratio Limit          |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics Valtage Mode         |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics Valtage              |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Gtaphics Valtage Offset       |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics IccMax               |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics Media Voltage        |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics Media Voltage Offset |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics Unslice Iccmax       |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| **Other**                               |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| System Agent IccMax                     |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |

XTU可用命令行进行控制。打开记事本，复制以下代码并保存，后缀名改为ps1，即可通过PowerShell运行。各种参数ID可以打开C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XtuCLI.exe查看。

```
$status = Get-Service -Name "XTU3SERVICE" | Select-Object "status" | Format-Wide
if ($status -ne "Running") {start-service -name "XTU3SERVICE"}
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 48 -v 45.00 # TDP
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 47 -v 60.00 # 短时间功耗墙
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 66 -v 28.00 # 睿频时间
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 34 -v -125 # 所降电压
sleep 4
stop-process -id $PID -force
```

#### Intel Performance Maximizer

官方自动超频软件，适用于i5、i7、i9、X系列的部分处理器。下载后运行即可。

```
https://downloadcenter.intel.com/zh-cn/product/188727/Intel-Performance-Maximizer
```

#### Throttlestop

下载链接如下。XP下可运行的版本为8.40。

```
# 最新版本
https://www.techpowerup.com/download/techpowerup-throttlestop/

# 8.40版本
http://xiazai.zol.com.cn/detail/44/439686.shtml
```

打开软件后勾选Set Multiplier和Speed Shift EPP，取消勾选BD PROCHOT，将Set Multiplier后面的数字拉到最大。点击Limit Reasons可查看功耗被限制的原因。

点击BCLK以设置PL1/PL2，可先尝试200。点击FIVR可设置电压，首先选择CPU Core/Cache，然后勾选Adjustable Voltage，再将下方的Offset Voltage拉到最大。

其余选项解释如下。

|           项目           |    中文名     |                                                                          注释                                                                          | 调整方法 |
|--------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Clock/Chipset Modulation | 时钟/芯片调整 | 大多数新芯片不开启，开启后ThrottleStop中的功能有的不起作用                                                                                             | 关闭     |
| Set Multipliet           | 乘数调整      |                                                                                                                                                        | 拉到最大 |
| Speed Shift-EPP          | 转换速度      | 如果勾选需要到TPL选项里面开启，开启了后根据CPU负载率来快速的降低和提升频率，数字可填，0是保持较大的性能上下浮动，可以向下浮动很低的频率，一般用于老CPU |          |
| Power Saver              | 节电调整      | 让机器用总线速率的一半运行，使相对老的CPU实现降低频率来降低功耗                                                                                        |          |
| Disable Turbo            | 关闭睿频      |                                                                                                                                                        |          |
| BD PROCHOT               | 温度墙        |                                                                                                                                                        |          |
| C1E                      |               | 空闲时随机上下数值频率                                                                                                                                 |          |

#### SetFSB

需要使用2.1.91.1版本，否则将提示无法使用。

使用前需知道CPU所用的时钟发生器的型号，可尝试搜索得到。若仍未知，则首先打开CPU-Z，记录总线速度和额定FSB，然后打开SetFSB，逐个时钟发生器进行尝试，点击Get FSB，查看下面的FSB值是否与CPU-Z中得到的一致，若一致则有可能为该时钟发生器。

确定时钟发生器的型号后，拖动滑块即可进行调整。本机可能的时钟发生器为ICS9LPR501HGLF和ICSLP505-2HGLF。

#### ClockGen

以管理员身份运行ClockGen，点击Options，确保已勾选`Apply current settings at startup`，保存。点击PLL Setup，勾选Ignore GSB/PCI，选择正确的PLL，可通过搜索或逐个尝试的方式，点击Read Clocks，然后点击PLL Control并查看数据是否正确。若正确，稍微向右拖动Selection的滑块并Apply即可。

#### AMD Ryzen Master

AMD锐龙处理器超频工具，下载链接如下。打开后调整速度和电压即可。

```
https://www.amd.com/zh-hans/technologies/ryzen-master
```

#### CPU-Tweaker

支持AMD Phenom DDR2&DDR3、INTEL Core i7 DDR3，不支持intel四核处理器。下载链接如下。

```
https://www.majorgeeks.com/files/details/cpu_tweaker.html
```

## 修改MSR寄存器

CPU的功率限制被写在MSR寄存器中。通过修改MSR寄存器的值，可以提升CPU的功率。

### 开启CFG

在BIOS中，若CFG Lock处于Enabled状态，将不能对MSR寄存器进行读写。故须先用grub将CFG Lock设为Disabled。

### 查看寄存器地址

#### 通过Hackintool

推荐使用该方法。

进入Mac并打开Hackintool，切换到工具选项卡。点击Intel图标，将输出复制到文本文档中，该输出即有CPU的相关信息，包括MSR寄存器的地址。

#### 通过AppleIntelInfo.kext

通过以下链接下载压缩包。

```
https://github.com/Piker-Alpha/AppleIntelInfo
```

在Mac下载压缩包并用Xcode编译，编译完成后切换到kext所在目录，输入以下命令以获取CPU信息。

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

### 查询寄存器含义

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
// 读取[A]地址的值
sudo rdmsr [A]

// 将[A]地址的值改为[B]
sudo wrmsr [A] [B]
```

### 修改寄存器内容

以下以修改Package power limits为例进行说明。

首先需要读取MSR_RAPL_POWER_UNIT寄存器的值，此寄存器存储功率/能量/时间单位。在本CPU中，该寄存器的地址为0x606，故输入以下命令。

```
sudo rdmsr 0x606 // 输出A0E03
```

从官方文档（第三章）可得到关于此寄存器的说明，原文如下。

```
MSR_RAPL_POWER_UNIT (Figure 14-35) provides the following information across all RAPL domains: 
Power Units (bits 3:0): Power related information (in Watts) is based on the multiplier, 1/ 2^PU; where PU is an unsigned integer represented by bits 3:0. Default value is 0011b, indicating power unit is in 1/8 Watts increment. 
Energy Status Units (bits 12:8): Energy related information (in Joules) is based on the multiplier, 1/2^ESU; where ESU is an unsigned integer represented by bits 12:8. Default value is 10000b, indicating energy status unit is in 15.3 micro-Joules increment. 
Time Units (bits 19:16): Time related information (in Seconds) is based on the multiplier, 1/ 2^TU; where TU is an unsigned integer represented by bits 19:16. Default value is 1010b, indicating time unit is in 976 micro- seconds increment. 
```

将A0E03转换为二进制为1010 0000 1110 0000 0011。bits A:B表示二进制的第B到A位（从低位开始数，个位是第0位），故按照说明可得下表。

| 原文                              | 位置    | 含义     | 内容（二进制）      | 内容含义                                                       | 说明                                                                        |
| --------------------------------- | ------- | -------  | ------------------- | -------------------------------------------------------------- |                                                                             |
| Power Units                       | 3:0     | 功率单位 | 0011                | 0.125W                                                         | 0011转换为十进制为3，故功率以$(\frac{1}{2})^3$=0.125W为单位                 |
| Energy Status Units               | 12:8    | 能量单位 | 01110               | 0.00006103515625J                                              | 01110转换为十进制为14，故能量以$(\frac{1}{2})^{14}$=0.00006103515625J为单位 |
| Time Units                        | 19:16   | 时间单位 | 1010                | 0.0009765625s                                                  | 01110转换为十进制为10，故时间以$(\frac{1}{2})^{10}$=0.0009765625s为单位     |

Package power limits位于MSR_PKG_POWER_LIMIT寄存器中，本CPU对应的地址为0x610。输入以下指令得到该地址的值，并对应进行修改。

```
sudo rdmsr 0x610 // 输出42816000DD8160（二进制为0000 0000 0100 0010 1000 0001 0110 0000 0000 0000 1101 1101 1000 0001 0110 0000）
```

|              原文              |  位置 | 内容（二进制）  | 更改为（二进制） |                                                         说明                                                         |
|--------------------------------|-------|-----------------|------------------|----------------------------------------------------------------------------------------------------------------------|
| Package Power Limit #1         | 14:0  | 000000101100000 | /                | 原值转换为十进制为352，因功率以0.125W为单位，故为352×0.125=44W                                                       |
| Enable Power Limit #1          | 15    |               1 | 0                | 0为关闭，1为开启                                                                                                     |
| Package Clamping Limitation #1 | 16    |               1 |                  |                                                                                                                      |
| Time Window for Power Limit #1 | 23:17 |         1101110 |                  | 时间限制值= $(2^Y*(1+Z/4)*TimeUnit)$，其中Y是寄存器的第21:17位，Z是寄存器的第23:22位，TimeUnit是前面所定义的时间单位 |
| Package Power Limit #2         | 46:32 | 000000101100000 |                  |                                                                                                                      |
| Enable Power Limit #2          | 47    |               1 | 0                | 0为关闭，1为开启                                                                                                     |
| Package Clamping Limitation #2 | 48    |               0 |                  |                                                                                                                      |
| Time Window for Power Limit #2 | 55:49 |         0100001 |                  | 时间限制值= $(2^Y*(1+Z/4)*TimeUnit)$，其中Y是寄存器的第21:17位，Z是寄存器的第23:22位，TimeUnit是前面所定义的时间单位 |
| Lock                           | 63    |               0 |                  | CFG锁，若为1则无法写入                                                                                               |

将更改后的内容从低位到高位组成一个64位的二进制数并转换为十六进制 ，然后用wrmsr写入即可。写入后用rdmsr读取，检验结果是否正确。

### 可修改的寄存器

#### MSR_PKG_POWER_LIMIT

参考值为42819800FC80C8h。

|             原文            |  位置 | 参考值（二进制） | 说明 |
|-----------------------------|-------|------------------|------|
| Pkg power limit             | 14:0  |         11001000 |      |
| Pkg power enabled           | 15:15 |                1 |      |
| Pkg clamping limit          | 16:16 |                0 |      |
| Pkg power limit time window | 23:17 |          1111110 |      |
| Pkg power limit 2           | 46:32 |        110011000 |      |
| Pkg power 2 enabled         | 47:47 |                1 |      |
| Pkg clamping limit 2        | 48:48 |                0 |      |
| Pkg power limit time window | 55:49 |          0100001 |      |
| MSR lock                    | 63:63 |                0 |      |

#### MSR_PLATFORM_INFO

|                原文                | 位置 | 参考值（二进制） |                  说明                  |
|------------------------------------|------|------------------|----------------------------------------|
| Maximum non-turbo                  | 15:8 |                  |                                        |
| Programmable ratio limit for turbo | 28   |                  | 为1时可修改MSR_TURBO_RATIO_LIMIT的内容 |
| Programmable TDP limit for turbo   | 29   |                  |                                        |
| Programmable TJ offset             | 30   |                  |                                        |

#### MSR_TURBO_RATIO_LIMIT

参考值为28282828h。

|   原文   |  位置 | 参考值（二进制） | 说明 |
|----------|-------|------------------|------|
| Ratio 1C | 7:0   |         00101000 |      |
| Ratio 2C | 15:8  |         00101000 |      |
| Ratio 3C | 23:16 |         00101000 |      |
| Ratio 4C | 31:24 |         00101000 |      |

# GPU

## 超频

### ATI Tray Tools

ATI显卡超频工具，可用于取代AMD官方的Catalyst Control Center，下载链接如下。

```
https://www.onlinedown.net/soft/28765.htm
```

点击硬件-超频设置即可对显卡进行超频。也可开启Anti-Alisaing、Temporal AA、Adaptive anti-alisaing、Anisotropic Filtering，关闭Enable High Quality AF、Wait for Vertical Sync、Anisotropic Filter Optimization、Trilinear Filter Optimization，调高Texture Preference、MipMap Detail Level、Catalyst AI。

### MSI Afterburner

非MSI图形卡也可使用，下载链接如下。

```
https://www.msi.com/Landing/afterburner
```

打开后进入设置，点击常规，勾选解除电压调整控制和解锁电压监控控制。回到主界面后调整核心电压即可。

若希望解锁频率上限，可进入MSI Afterburner的安装目录，打开MSIAfterBurner.cfg，定位到[ATIADLHAL]一项，将相关内容修改为如下，保存后重启电脑即可。

```
UnofficialOverclockingEULA = I confirm that I am aware of unofficial overclocking limitations and fully understand that MSI will not provide me any support on it.
UnofficialOverclockingMode = 1
```

### EVGA Precision X 16

用于Nvidia显卡，下载链接如下。

```
https://www.evga.com/precisionx/
```

打开软件后将Power Target和GPU Temp Target拉满，点亮Overvoltage以设置电压。

### NVIDIA Profile Inspector

修改Nvidia游戏配置文件，下载链接如下。

```
https://github.com/Orbmu2k/nvidiaProfileInspector
```

### GMABooster

用于Intel显卡，下载链接如下。打开后选择高频率即可。

```
https://www.techspot.com/downloads/4828-gmabooster.html
```

### NVIDIA nTune

用于Nvidia显卡，下载链接如下。安装后找到GPU超频选项，根据实际情况调整即可。

```
https://www.nvidia.com/en-us/drivers/ntune-5052500/
```

### ASUS GPU Tweak II

用于华硕显卡，下载链接如下。打开后直接进行参数调整即可。

```
https://www.asus.com/supportonly/GPUTweak%20II/HelpDesk_Download/
```

### SAPPHIRE TriXX

下载链接如下。打开后点击TriXX BOOST并配置即可。

```
https://www.sapphiretech.com/zh-cn/software
```

# 内存

## 超频

### 通过BIOS

若主板支持XMP，在BIOS中打开即可。选项名称一般为Ai Overclock Tuner。

也可手动调整配置文件。在BIOS中找到可以调整内存配置文件的部分，选择频率、时序和内存电压的各种组合。需要注意的是，要稳定在更高的频率，需要增大（放松）时序，也可能需要增加电压；要在当前频率稳定的情况下提升性能，应减小（缩紧）时序。将1.5V视为电压最大值，并尽可能将其降低。

如果系统在应用新设置后无法启动，可尝试下调频率、更改时序或稍微增大内存电压和IMC电压以允许更高的频率。

### 通过软件

#### MemSet

可用于调节内存参数，下载链接如下。

```
http://www.xz7.com/downinfo/39311.html
```

在打开软件时需先手动安装Drivers文件夹中的Setup.inf。选项含义如下。

|             项目             |      中文名      |                            注释                            | 调整方法 |
|------------------------------|------------------|------------------------------------------------------------|----------|
| Clock/Chipset Modulation     | CAS#延迟         | 大多数新芯片不开启，开启后ThrottleStop中的功能有的不起作用 | 调至最低 |
| Precharge Delay              | 预充电延迟       |                                                            | 5-11之间 |
| RAS# Precharge               | tRP行预充电时间  | 越低表示在不同行切换的速度越快                             |          |
| Refresh Mode Select          | 内存更新模式选择 |                                                            |          |
| Refresh Cycle Time           | 更新周期         |                                                            |          |
| Performance Mode             | 增强模式         |                                                            |          |
| Write Recovery Time          | 写到读的延迟     |                                                            |          |
| Read-Write Turnaround clocks | 读到写的转变时钟 |                                                            |          |

#### DRAM Calculator for Ryzen

用于AMD Ryzen Zen架构的第一代和第二代处理器，下载链接如下。

```
https://www.techpowerup.com/download/ryzen-dram-calculator/
```

首先打开Thaiphoon，并提取内存信息文件。然后打开DRAM Calculator for Ryzen，左上角选择相关参数，解释如下。

|       参数      |                                         说明                                        |
|-----------------|-------------------------------------------------------------------------------------|
| Processor       | 处理器类型                                                                          |
| Memory Type     | 可在Thaiphoon的part number后几位字母看到                                            |
| Profile version | 建议选择Manual，不支持XMP的普通内存条选择V1（较好体质的颗粒）或V2（较差体质的颗粒） |
| Memory bank     | 内存单面/双面                                                                       |
| Frequency       | 要超到的频率                                                                        |
| DIMM Modules    | 内存根数                                                                            |
| Motherboard     | 芯片组类型                                                                          |

点击Import XMP，导入配置文件，点击Calculate SAFE或Calculate FAST生成超频所需要的全部时序参数，截图保存进BIOS照抄即可。一般首选SAFE档成功率较高，FAST次之。

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

## 本机CPU信息

由Hackintool输出。

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

## 本机UEFI BIOS推荐设置

Override指手动设置。

|                                      选项                                     | 本机BIOS地址 | 设定值 |          设置         |                                                                                                                                                             说明                                                                                                                                                            |
|-------------------------------------------------------------------------------|--------------|--------|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Intel RC ACPI Settings**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| PTID Support                                                                  |              |        |                       | PTID是电源/监视芯片，本选项允许启用/禁用电源和温度仪器详细信息，设置为禁用则会从BIOS删除SSDt表之一的PtidDevc                                                                                                                                                                                                                |
| PECI Access Method                                                            |              |        | ACPI                  | PECI（Platform Environment Control Interface）为用于监测CPU及芯片组温度的一线总线                                                                                                                                                                                                                                           |
| Native PCIE Enable                                                            |              |        |                       | 开启时操作系统可控制热插拔、SHPC本地热插拔控制、电源管理事件、PCIe高级错误报告控制、PCIe能力结构控制、时延容忍报告控制，禁用时由BIOS控制                                                                                                                                                                                    |
| Native ASPM                                                                   |              |        |                       | 启用时由操作系统控制设备的ASPM（Active State Power Management，主动性电源管理）支持，禁用时由BIOS控制                                                                                                                                                                                                                       |
| Wake system from S5                                                           |              |        |                       | 从S5事件中唤醒系统                                                                                                                                                                                                                                                                                                          |
| ACPI Debug                                                                    |              |        |                       | 调试ACPI                                                                                                                                                                                                                                                                                                                    |
| Low Power S0 Idle Capability                                                  |              |        |                       | 在S0低功耗模式（现代待机模式）下，系统将保持部分运行，在网络可用且实时动作需要完成（如系统维护）时唤醒                                                                                                                                                                                                                      |
| 10sec Power Button OVR                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| EC Notification                                                               |              |        |                       | EC通知                                                                                                                                                                                                                                                                                                                      |
| EC CS Debug Light                                                             |              |        |                       | Caps Lock指示灯                                                                                                                                                                                                                                                                                                             |
| EC Low Power Mode                                                             |              |        |                       | EC低电量模式                                                                                                                                                                                                                                                                                                                |
| EC Debug LED                                                                  |              |        |                       | EC调试LED灯                                                                                                                                                                                                                                                                                                                 |
| EC Base CS Power Policy                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Sensor Standby                                                                |              |        |                       | 传感器待机                                                                                                                                                                                                                                                                                                                  |
| CS PL1 Limit                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CS PL1 Value                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Lpit Recidency Counter                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Intel Ready Mode Technology                                                   |              |        |                       | Intel Ready Mode与传统睡眠模式相似，使电脑进入安静运行且低功耗的状态，与手机和平板电脑待机时使用的模式相近                                                                                                                                                                                                                  |
| HW Notification                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Intel RMT State                                                               |              |        |                       | Intel Ready Mode技术状态                                                                                                                                                                                                                                                                                                    |
| PCI Delay Optimization                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Type C Support                                                                |              |        |                       | Type-C支持                                                                                                                                                                                                                                                                                                                  |
| **PEP Constraints Configuration**                                             |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| PEP CPU/Graphics/UART                                                         |              |        |                       | 在PEP mitigation list中开启或关闭CPU/Graphics/UART选项                                                                                                                                                                                                                                                                      |
| PEP ISP                                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PEP SATA Controller                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PEP RAID VOL0                                                                 |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PEP SATA PORT0-5                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PEP SATA NVM1-3                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PEP I2C0-I2C5                                                                 |              |        |                       | 在PEP mitigation list中开启或关闭PEP I2C0-PEP I2C5选项                                                                                                                                                                                                                                                                      |
| PEP SPI                                                                       |              |        |                       | 在PEP mitigation list中开启或关闭PEP SPI选项                                                                                                                                                                                                                                                                                |
| PEP XHCI/Audio/EMMC/SDXC                                                      |              |        |                       | 在PEP mitigation list中开启或关闭XHCI/Audio/EMMC/SDXC选项                                                                                                                                                                                                                                                                   |
| PEP WIGIG F1                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **CPU Configuration**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| SW Guard Extensions (SGX)                                                     |              |        |                       | SGX即Software Guard Extensions，应用程序可以使用它来创建和访问私有内存区域，选择Enabled或Software Controlled时将启用SGX，其中选择Enabled时预留的私有内存大小由BIOS设置，选择Software Controlled时预留的私有内存大小由操作系统指定                                                                                           |
| Select Owner EPOCH input type                                                 |              |        |                       | 更改所创建的锁定内存区域的安全密钥的种子（Intel驱动程序通过该种子创建一个密钥来锁定私有内存区域）                                                                                                                                                                                                                           |
| Software Guard Extensions Epoch 0                                             |              |        |                       | 系统生成并显示的EPOCH值                                                                                                                                                                                                                                                                                                     |
| Software Guard Extensions Epoch 1                                             |              |        |                       | 系统生成并显示的EPOCH值                                                                                                                                                                                                                                                                                                     |
| PRMRR Size                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CPU Flex Ratio Override                                                       | 0x4EE        | 0x1    | Enabled               | CPU倍频手动设置                                                                                                                                                                                                                                                                                                             |
| CPU Flex Ratio Settings                                                       | 0x4EC        | 0x3F   | Maximum               | CPU倍频手动设置                                                                                                                                                                                                                                                                                                             |
| Hardware Prefetcher                                                           | 0x5BE        | 0x1    | Enabled               | 让CPU在L2 Cache进行预取反馈和数据                                                                                                                                                                                                                                                                                           |
| Adjacent Cache Line Prefetch                                                  | 0x5BF        | 0x1    | Enabled               | 让L2 Cache的中间缓存线运行相邻缓存线预取功能                                                                                                                                                                                                                                                                                |
| Intel (VMX) Virtualization Technology                                         | 0x5B8        | 0x1    | Enabled               | Intel硬件辅助虚拟化技术                                                                                                                                                                                                                                                                                                     |
| PECI                                                                          |              |        |                       | PECI（Platform Environment Control Interface）为用于监测CPU及芯片组温度的一线总线                                                                                                                                                                                                                                           |
| Active Processor Cores                                                        | 0x4F1        | 0x0    | All                   | 激活处理器的硬件核心数                                                                                                                                                                                                                                                                                                      |
| Execute Disable Bit                                                           |              |        |                       | 病毒防护技术 (Execute Disable Bit) 让处理器针对内存区域进行分类，以判断程序是否安全可执行                                                                                                                                                                                                                                   |
| Limit CPUID Maximum                                                           | 0x4EA        | 0x0    | Disabled              | 开启时cupid toreturn将返回最大值3                                                                                                                                                                                                                                                                                           |
| Hyper-Threading                                                               | 0x4F0        | 0x1    | Enabled               | 禁用时每个启用的核心只启用一个线程，在运行需要尽可能短的响应时间的应用程序时可设为Disabled                                                                                                                                                                                                                                  |
| BIST                                                                          |              |        |                       | 内建自检                                                                                                                                                                                                                                                                                                                    |
| JTAG C10 Power                                                                |              |        |                       | 启用/禁用C10及以上电源状态下的Power JTAG                                                                                                                                                                                                                                                                                    |
| AP threads Idle Manner                                                        |              |        |                       | 等待信号运行的AP线程空闲方式                                                                                                                                                                                                                                                                                                |
| AP threads Handoff Manner                                                     |              |        |                       | AP线程从POST结束切换到OS方式                                                                                                                                                                                                                                                                                                |
| AES                                                                           |              |        |                       | 启用/禁用AES (高级加密标准，Advance Encryption Standard)                                                                                                                                                                                                                                                                    |
| MachineCheck                                                                  |              |        |                       | 机器检查                                                                                                                                                                                                                                                                                                                    |
| MonitorMWait                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Intel Trusted Execution Technology                                            |              |        |                       | 启用英特尔可信执行技术（Trusted Execution Technology）提供的额外硬件功能                                                                                                                                                                                                                                                    |
| Alias Check Request DPR Memory Size (MB)                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Reset AUX Content                                                             |              |        |                       | 重置TPM辅助内容（在AUX内容被重置后，Txt可能不起作用）                                                                                                                                                                                                                                                                       |
| Flash Wear Out Protection                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Current Debug Interface Status                                                |              |        |                       | 当前调试界面状况                                                                                                                                                                                                                                                                                                            |
| Debug Interface                                                               |              |        |                       | 调试界面                                                                                                                                                                                                                                                                                                                    |
| Direct Connect Interface                                                      |              |        |                       | 直连界面                                                                                                                                                                                                                                                                                                                    |
| Debug Interface Lock                                                          |              |        |                       | 调试界面锁                                                                                                                                                                                                                                                                                                                  |
| Processor trace memory allocation                                             |              |        |                       | 禁用/选择处理器跟踪（内存区域大小为4KB-128MB）                                                                                                                                                                                                                                                                              |
| Processor trace                                                               |              |        |                       | Intel处理器跟踪，利用硬件来记录程序执行的轨迹                                                                                                                                                                                                                                                                               |
| SMM Processor Trace                                                           |              |        |                       | SMM处理器追踪                                                                                                                                                                                                                                                                                                               |
| FCLK Frequency for Early Power On                                             | 0x5F9        | 0x1    | 1Ghz                  | 减少PCIE的延迟，稍微提升PCIE设备性能                                                                                                                                                                                                                                                                                        |
| Voltage Optimization                                                          |              |        |                       | 电压优化                                                                                                                                                                                                                                                                                                                    |
| **CPU SMM Enhancement**                                                       |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| SMM MSR Save State Enable                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| SMM Code Access Check                                                         |              |        |                       | SMM代码权限检查                                                                                                                                                                                                                                                                                                             |
| SMM Use Delay Indication                                                      |              |        |                       | 启用/禁用SMI中用于MP同步的SMM_DELAYED MSR                                                                                                                                                                                                                                                                                   |
| SMM Use Block Indication                                                      |              |        |                       | 启用/禁用SMI中用于MP同步的SMM_BLOCKED MSR                                                                                                                                                                                                                                                                                   |
| SMM Use SMM en-US Indication                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **CPU - Power Management Control**                                            |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Boot performance mode                                                         | 0x4F7        | 0x2    | Turbo Performance     | BIOS在系统切换前设置的启动模式                                                                                                                                                                                                                                                                                              |
| Intel SpeedStep                                                               | 0x4F4        | 0x1    | Enabled               | 处理器性能状态（P状态）                                                                                                                                                                                                                                                                                                     |
| Race To Halt (RTH)                                                            |              |        |                       | 快速增加工作负荷以快速进入睡眠状态                                                                                                                                                                                                                                                                                          |
| Intel Speed Shift Technology                                                  | 0x4F6        | 0x1    | Enabled               | Intel CPU节电技术、状态调整，大大提高响应速度                                                                                                                                                                                                                                                                               |
| HDC Control                                                                   |              |        |                       | HDC配置                                                                                                                                                                                                                                                                                                                     |
| Turbo Mode                                                                    | 0x4FA        | 0x1    | Enabled               | 睿频模式（EMTTM必须启用）                                                                                                                                                                                                                                                                                                   |
| Platform PL1 Enable                                                           | 0x515        | 0x0    | Disabled              | Platform 1功耗限制                                                                                                                                                                                                                                                                                                          |
| Platform PL1 Power                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Power Limit 1 Time Window                                                     | 0x500        | 0x80   | Maximum               | 超频最长时间                                                                                                                                                                                                                                                                                                                |
| Platform PL2 Enable                                                           | 0x51B        | 0x0    | Disabled              | Platform 2功耗限制                                                                                                                                                                                                                                                                                                          |
| Platform PL2 Power                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Power Limit 4 Override                                                        | 0x50E        | 0x1    | Enabled               | 无视超频功率4限制                                                                                                                                                                                                                                                                                                           |
| Power Limit 4                                                                 |              |        | Maximum               | Platform 4功耗限制值                                                                                                                                                                                                                                                                                                        |
| Power Limit 4 Lock                                                            | 0x513        | 0x0    | Disabled              | Platform 4功耗限制锁                                                                                                                                                                                                                                                                                                        |
| C states                                                                      |              |        |                       | CPU核心睡眠深度（CPU全速运行时候会工作在C0，任务量较时会工作在C1，CPU核心休眠的时候会工作在C7，当CPU核心从睡眠状态唤醒需要比较高的延迟，通常比内存延迟高得多）                                                                                                                                                              |
| Enhanced C-states                                                             |              |        |                       | C1E状态，让处理器在闲置时降低电力消耗                                                                                                                                                                                                                                                                                       |
| C-State Auto Demotion                                                         |              |        | C1 and C3             | 一般来说，较深的C-states（如C6或C7）具有较长的延迟，并且具有较高的能量进入/退出成本。当更深的C-states的进入/退出频率很高时，产生的性能和能量损失就会变得非常严重。因此，不正确或低效率地使用更深层次的c状态会对电池空闲时间产生负面影响。为了增加驻留时间和提高电池在更深的C-states下的空闲时间，处理器支持C-states自动降级 |
| C-State Undemotion                                                            |              |        | C1 and C3             | C-State不降级                                                                                                                                                                                                                                                                                                               |
| Package C-State Demotion                                                      |              |        |                       | Package C-State降级，利用历史信息通过避免从深层Package C-State到浅层Package C-State的多次转换来节省电能                                                                                                                                                                                                                     |
| Package C-State Un-demotion                                                   |              |        |                       | 在确定Package C-state的层级前降级是不正确的，该选项使处理器能够切换回原来请求的深层Package C-State                                                                                                                                                                                                                          |
| CState Pre-Wake                                                               |              |        |                       | 使用core/package C-state Pre-Wake定时器启用或禁用core/package在深层C-state中的早期唤醒功能                                                                                                                                                                                                                                  |
| IO MWAIT Redirection                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Package C-State Limit                                                         |              |        |                       | 设置CPU Package的C-State支持，允许处理器的单个核心或整个处理器进入休眠状态                                                                                                                                                                                                                                                  |
| **View/Configure Turbo Options**                                              |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Energy Efficient P-state                                                      |              |        |                       | 节能P-state模式                                                                                                                                                                                                                                                                                                             |
| Package Power Limit MSR Lock                                                  | 0x514        | 0x0    | Disabled              | MSR锁                                                                                                                                                                                                                                                                                                                       |
| Power Limit 1 Override                                                        | 0x4FF        | 0x1    | Enabled               | 无视超频功率1限制                                                                                                                                                                                                                                                                                                           |
| Power Limit 1                                                                 |              |        |                       | 长时睿频功耗，CPU在长时间负载时，基本最后都会停留到这个功耗，基本为一台笔记本CPU实际上的TDP                                                                                                                                                                                                                                 |
| Power Limit 1 Time Window                                                     | 0x54A        | 0x80   | Maximum               | 超频最长时间                                                                                                                                                                                                                                                                                                                |
| Power Limit 2 Override                                                        | 0x501        | 0x1    | Enabled               | 无视超频功率2限制                                                                                                                                                                                                                                                                                                           |
| Power Limit 2                                                                 |              |        |                       | 短时睿频功耗，通常会高过PL1许多，是在短时间负载内CPU能够达到的最高功耗，基本为一台笔记本的CPU性能上限                                                                                                                                                                                                                       |
| 1-Core Ratio Limit Override                                                   | 0x5D1        | 0x53   | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                                                                                                                        |
| 2-Core Ratio Limit Override                                                   | 0x5D2        | 0x53   | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                                                                                                                        |
| 3-Core Ratio Limit Override                                                   | 0x5D3        | 0x53   | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                                                                                                                        |
| 4-Core Ratio Limit Override                                                   | 0x5D4        | 0x53   | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                                                                                                                        |
| 5-Core Ratio Limit Override                                                   | 0x6D3        | 0x53   | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                                                                                                                        |
| 6-Core Ratio Limit Override                                                   | 0x6D4        | 0x53   | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                                                                                                                        |
| 7-Core Ratio Limit Override                                                   | 0x6D5        | 0x53   | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                                                                                                                        |
| 8-Core Ratio Limit Override                                                   | 0x6D6        | 0x53   | Maximum               | 单核工作时超频至最高                                                                                                                                                                                                                                                                                                        |
| Energy Efficient Turbo                                                        |              |        |                       | 节能睿频模式，将适时降低涡轮频率，以提高能源效率                                                                                                                                                                                                                                                                            |
| **CPU VR Settings**                                                           |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| PSYS Slope                                                                    |              |        |                       | PSYS Slope基本为IMON slope，以1/100的增量定义，并使用BIOS VR mailbox 0x9指令。有效范围为0-200，输入125表示斜率为1.25，输入0表示自动                                                                                                                                                                                         |
| PSYS Offset                                                                   |              |        |                       | PSYS偏移量以1/4的增量定义，并使用BIOS VR mailbox 0x9指令。有效范围为0-255，输入100表示偏移量为25                                                                                                                                                                                                                            |
| PSYS PMax Power                                                               |              |        |                       | 该值以1/8瓦特为单位定义，并使用BIOS VR mailbox 0xB指令。有效范围为0-8192，输入1000表示PMax值为125瓦，输入0表示自动                                                                                                                                                                                                          |
| VR Mailbox Command options                                                    |              |        |                       | VR mailbox发送的指令，1表示MPS VR cmd，2表示PS4 Exit VR cmd，4表示MPS VR Decay cmd。输入要使用的命令的和，如5表示1+4                                                                                                                                                                                                        |
| **Acoustic Noise Settings**                                                   |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Acoustic Noise Mitigation                                                     |              |        |                       | 当CPU处于更深的C-States时，开启该选项可以帮助减轻某些SKU上的噪音                                                                                                                                                                                                                                                            |
| Disable Fast PKG C State Ramp for IA/GT/SA Domain                             |              |        |                       | 选择False以在更深层次的C-States中启用快速Ramp                                                                                                                                                                                                                                                                               |
| Slow Slew Rate for IA/GT/SA Domain                                            |              |        |                       | 设置VR IA慢转换速率的深层C-States Ramp时间，用于帮助减少声噪声。慢转换速率等于快转换速率除以数字2/4/8/16                                                                                                                                                                                                                    |
| **Core/IA VR Settings**                                                       |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| VR Config Enable                                                              |              |        | Enabled               | /                                                                                                                                                                                                                                                                                                                           |
| AC Loadline                                                                   |              |        |                       | AC负载线，定义为1/100 mOhms，使用BIOS VR mailbox 0x2指令。有效范围为0-6249(0-62.49mOhms)，输入1255表示12.55mOhm，输入0表示自动                                                                                                                                                                                              |
| DC Loadline                                                                   |              |        |                       | DC负载线，定义为1/100 mOhms，使用BIOS VR mailbox 0x2指令。有效范围为0-6249（0-62.49mOhms），输入1255表示12.55mOhm，输入0表示自动                                                                                                                                                                                            |
| PS Current Threshold1                                                         |              |        | 120                   | PS当前阈值1以1/4A增量定义，使用BIOS VR mailbox 0x3指令。有效范围为0-512（0-128A），输入400表示10A，输入0表示自动。默认为80（20A）                                                                                                                                                                                           |
| PS Current Threshold2                                                         |              |        | 40                    | PS当前阈值1以1/4A增量定义，使用BIOS VR mailbox 0x3指令。有效范围为0-512（0-128A），输入400表示10A，输入0表示自动。默认为20（5A）                                                                                                                                                                                            |
| PS Current Threshold3                                                         |              |        | 0                     | PS当前阈值1以1/4A增量定义，使用BIOS VR mailbox 0x3指令。有效范围为0-512（0-128A），输入400表示10A，输入0表示自动。默认为4（1A）                                                                                                                                                                                             |
| PS3 Enable                                                                    |              |        |                       | 使用BIOS VR mailbox 0x3指令                                                                                                                                                                                                                                                                                                 |
| PS4 Enable                                                                    |              |        |                       | 使用BIOS VR mailbox 0x3指令                                                                                                                                                                                                                                                                                                 |
| IMON Slope                                                                    |              |        |                       | IMON（Load Current Monitor）Slope以1/100增量定义，使用BIOS VR mailbox 0x4指令，CPU真实功率乘以该倍率后报给EC控制。有效范围为0-200，输入125表示倍率为1.25，输入0表示自动。如输入80，则20W限制的CPU在上报时仅为16W，EC会继续控制该CPU达到25W，但认为该CPU当前功率仅为20W（25*0.8=20）                                         |
| IMON Offset                                                                   |              |        |                       | IMON偏移量以1/1000的增量定义，使用BIOS VR mailbox 0x4指令。有效范围为0-63999，输入25348表示偏移量为25.348。调整会影响电池模式消耗                                                                                                                                                                                           |
| IMON Prefix                                                                   |              |        |                       | 设置IMON偏移值为正数或负数                                                                                                                                                                                                                                                                                                  |
| VR Current Limit                                                              |              |        | 400                   | 设置电压调节器的电流限制（允许的最大瞬时电流），以1/4A为单位递增，使用BIOS VR mailbox 0x6指令。输入400表示100A，输入0表示自动                                                                                                                                                                                               |
| VR Voltage Limit                                                              |              |        | 1100                  | 设置电压调节器的电压限制，使用BIOS VR mailbox 0x6指令。输入1250表示1.25V，输入0表示自动                                                                                                                                                                                                                                     |
| TDC Enable                                                                    |              |        |                       | TDC（Thermal Design Current）                                                                                                                                                                                                                                                                                               |
| TDC Current Limit                                                             |              |        |                       | TDC电流限制以1/8A增量定义，使用BIOS VR mailbox 0x1A指令。有效范围是0-32767，输入1000表示125A，输入0表示0A                                                                                                                                                                                                                   |
| TDC Time Window                                                               |              |        |                       | TDC时间窗口以毫秒为单位定义。选项有1ms/2ms/3ms/4ms/5ms/6ms/7ms/8ms/10ms（9ms在MSR定义中没有有效的编码）                                                                                                                                                                                                                     |
| TDC Lock                                                                      |              |        |                       | TDC锁                                                                                                                                                                                                                                                                                                                       |
| **Power Limit 3 Settings**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Power Limit 3 Override                                                        | 0x506        | 0x1    | Enabled               | 无视超频功率3限制                                                                                                                                                                                                                                                                                                           |
| Power Limit 3                                                                 |              |        |                       | 超过这个阈值，PL3快速功率限制算法将试图通过被动限制频率来限制高于PL3的峰值的占空比                                                                                                                                                                                                                                          |
| Power Limit 3 Time Window                                                     | 0x50B        | 0x40   | Maximum               | 超频最长时间                                                                                                                                                                                                                                                                                                                |
| Power Limit 3 Duty Cycle                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Power Limit 3 Lock                                                            | 0x50D        | 0x0    | Disabled              | Platform 3功耗限制锁                                                                                                                                                                                                                                                                                                        |
| **Config TDP Configurations**                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Configurable TDP Boot Mode                                                    | 0x528        | 0x2    | Up                    | TDP驱动模式                                                                                                                                                                                                                                                                                                                 |
| Configurable TDP Lock                                                         | 0x529        | 0x0    |                       | TDP锁                                                                                                                                                                                                                                                                                                                       |
| CTDP BIOS control                                                             |              |        |                       | 通过运行时ACPI BIOS方法开启CTDP控制                                                                                                                                                                                                                                                                                         |
| **Custom Settings Down**                                                      |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Power Limit 1                                                                 |              |        |                       | 设置Package Power Limit 1，以毫瓦为单位。当超过这个限制时，CPU比率会在一段时间后降低。设置较低的阈值可以节约功耗和保护CPU，设置较高的阈值可以提高性能。设为0时由BIOS设置，默认设置取决于CPU                                                                                                                                 |
| Power Limit 2                                                                 |              |        |                       | 设置Package Power Limit 2，以毫瓦为单位。当超过这个限制时，CPU比率会在一段时间后降低。设置较低的阈值可以节约功耗和保护CPU，设置较高的阈值可以提高性能。设为0时由BIOS设置，默认设置取决于CPU                                                                                                                                 |
| Power Limit 1 Time Window                                                     |              |        |                       | TDP值被维护的时间窗口的长度，有效范围为0-128，默认值为8                                                                                                                                                                                                                                                                     |
| ConfigTDP Turbo Activation Ratio                                              |              |        |                       | Turbo激活比的自定义值。默认值是0，表示不使用自定义值                                                                                                                                                                                                                                                                        |
| **View/Configure CPU Lock Options**                                           |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| CFG Lock                                                                      |              |        |                       | 关闭或开启MSR 0xe2寄存器，CPU的P-State/C-State电源管理是放在MSR寄存器里的（用黑苹果时候须解锁）                                                                                                                                                                                                                             |
| Overclocking Lock                                                             | 0x5D6        | 0x0    | Disabled              | 超频锁                                                                                                                                                                                                                                                                                                                      |
| **GT - Power Management Control**                                             |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| RC6(Render Standby)                                                           | 0x740        | 0x1    | Enabled               | 集成显卡待机                                                                                                                                                                                                                                                                                                                |
| Maximum GT frequency                                                          | 0x752        | 0xFF   | Default Max Frequency | 显卡最大频率                                                                                                                                                                                                                                                                                                                |
| **PCH-FW Configuration**                                                      |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| ME State                                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Manageability Features State                                                  |              |        |                       | 启用/禁用Intel Active Management Technology BIOS扩展，注意iAMT H/W总是启用，这个选项只控制BIOS扩展的执行。如果启用，则需要在SPI中添加额外的固件                                                                                                                                                                             |
| AMT BIOS Features                                                             | 0x6F1        | 0x1    | Enabled               | /                                                                                                                                                                                                                                                                                                                           |
| ME Unconfig on RTC Clear                                                      |              |        |                       | 禁用时，ME将不会在RT重置时取消配置                                                                                                                                                                                                                                                                                          |
| NFC Feature State                                                             |              |        | Disabled              | 指定一个NFC模块，或None表示禁用                                                                                                                                                                                                                                                                                             |
| Comms Hub Support                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| JHI Support                                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Core Bios Done Message                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **PTT Configuration**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| TPM Device Selection                                                          |              |        |                       | TPM设备选择                                                                                                                                                                                                                                                                                                                 |
| PTP aware OS                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| TPM 1.2 Deactivate                                                            |              |        |                       | TPM（Trusted Platform Module）1.2撤销                                                                                                                                                                                                                                                                                       |
| **Firmware Update Configuration**                                             |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Me FW Image Re-Flash                                                          |              |        |                       | BIOS升降级功能                                                                                                                                                                                                                                                                                                              |
| **ME Debug Configuration**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| HECI Timeouts                                                                 |              |        |                       | HECI收/发超时时间                                                                                                                                                                                                                                                                                                           |
| Force ME DID Init Status                                                      |              |        |                       | 强制DID初始化状态值                                                                                                                                                                                                                                                                                                         |
| CPU Replaced Polling Disable                                                  |              |        |                       | 禁用CPU更换轮询循环                                                                                                                                                                                                                                                                                                         |
| ME DID Message                                                                |              |        |                       | 启用/禁用ME DID消息                                                                                                                                                                                                                                                                                                         |
| HECI Retry Disable                                                            |              |        |                       | 禁用所有HECI API的重试机制                                                                                                                                                                                                                                                                                                  |
| HECI Message check Disable                                                    |              |        |                       | 在发送消息时，禁止对BIOS引导路径进行消息检查                                                                                                                                                                                                                                                                                |
| MBP HOB Skip                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| KT Device                                                                     |              |        |                       | KT设备                                                                                                                                                                                                                                                                                                                      |
| IDER Device                                                                   |              |        |                       | IDER设备                                                                                                                                                                                                                                                                                                                    |
| End Of Post Message                                                           |              |        |                       | 启用/禁用发送给ME的结束信息                                                                                                                                                                                                                                                                                                 |
| D0I3 Setting for HECI Disable                                                 |              |        |                       | 禁止为所有HECI设备设置DOI3位                                                                                                                                                                                                                                                                                                |
| **AMT Configuration**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| ASF support                                                                   |              |        |                       | 启用/禁用警报规格格式ASF（Alert Specification Format）                                                                                                                                                                                                                                                                      |
| USB Provisioning of AMT                                                       |              |        |                       | 开启/关闭USB发放功能                                                                                                                                                                                                                                                                                                        |
| **CIRA Configuration**                                                        |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| CIRA Timeout                                                                  |              |        |                       | 为要建立的CIRA连接设置超时选项。可用范围为1-254秒，输入000表示将60秒作为缺省的超时值，输入255表示不限制等待建立连接的时间                                                                                                                                                                                                   |
| **ASF Configuration**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| PET Progress                                                                  |              |        |                       | 启用/禁用PET进程                                                                                                                                                                                                                                                                                                            |
| WatchDog                                                                      |              |        | Enabled               | 看门狗计时器                                                                                                                                                                                                                                                                                                                |
| OS Timer                                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| BIOS Timer                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Secure Erase Configuration**                                                |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Secure Erase mode                                                             |              |        |                       | 安全擦除模式                                                                                                                                                                                                                                                                                                                |
| Force Secure Erase                                                            |              |        |                       | 强制安全擦除                                                                                                                                                                                                                                                                                                                |
| **MEBx Resolution Settings**                                                  |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Non-UI Mode Resolution                                                        |              |        | Auto                  | MEBx对MEBx用户界面外的消息使用的文本模式分辨率                                                                                                                                                                                                                                                                              |
| UI Mode Resolution                                                            |              |        | Auto                  | MEBx使用的文本模式分辨率                                                                                                                                                                                                                                                                                                    |
| Graphics Mode Resolution                                                      |              |        | Auto                  | MEBx使用的图形模式分辨率                                                                                                                                                                                                                                                                                                    |
| **Cpu Thermal Configuration**                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| DTS SMM                                                                       |              |        |                       | 开启CPU数字热传感器功能（DTS，Digital Thermal Sensor）                                                                                                                                                                                                                                                                      |
| Tcc Offset Clamp Enable                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Tcc Offset Lock Enable                                                        |              |        |                       | 开启温度墙调整锁                                                                                                                                                                                                                                                                                                            |
| Bi-directional PROCHOT#                                                       | 0x561        | 0x0    | Disabled              | PROCHOT#用于温度高时强制降频，开启该选项将使外部代理可驱动PROCHOT#以控制处理器                                                                                                                                                                                                                                              |
| Disable PROCHOT# Output                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Disable VR Thermal Alert                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PROCHOT Response                                                              |              |        | Disabled              | /                                                                                                                                                                                                                                                                                                                           |
| PROCHOT Lock                                                                  |              |        |                       | 温度墙锁                                                                                                                                                                                                                                                                                                                    |
| PECI Reset                                                                    |              |        |                       | PECI重置                                                                                                                                                                                                                                                                                                                    |
| PECI C10 Reset                                                                |              |        |                       | PECI C10重置                                                                                                                                                                                                                                                                                                                |
| **Platform Thermal Configuration**                                            |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Automatic Thermal Reporting                                                   |              |        |                       | 根据BWG热管理设置报告中推荐的值自动配置`_CRT`、`_PSV`、`_AC0`                                                                                                                                                                                                                                                               |
| Critical Trip Point                                                           | 0x2B2        | 0x7F   | 127 C                 | 系统关闭的温度                                                                                                                                                                                                                                                                                                              |
| Active Trip Point 0                                                           |              |        |                       | 操作系统在到达Active Trip Point 0的温度后，将把处理器风扇转速设置为Active Trip Point 0 Fan Speed。取值范围15/23/31/39/47/55/63/71/79/87/95/100/111/127C，默认值为71C                                                                                                                                                        |
| Active Trip Point 0 Fan Speed                                                 |              |        |                       | 操作系统在到达Active Trip Point 0的温度后，将把处理器风扇转速设置为Active Trip Point 0 Fan Speed。取值为0-100，0为风扇关闭，100为最大转速                                                                                                                                                                                   |
| Active Trip Point 1                                                           |              |        |                       | 操作系统在到达Active Trip Point 1的温度后，将把处理器风扇转速设置为Active Trip Point 1 Fan Speed。取值范围15/23/31/39/47/63/71/79/87/95/100/103/111/119(POR)/127C，默认值为55C                                                                                                                                              |
| Active Trip Point 1 Fan Speed                                                 |              |        |                       | 操作系统在到达Active Trip Point 1的温度后，将把处理器风扇转速设置为Active Trip Point 1 Fan Speed。取值为0-100，0为风扇关闭，100为最大转速，默认值为75。该值必须小于Active Trip Point 0 Fan Speed                                                                                                                            |
| Passive Trip Point                                                            | 0x2B1        | 0x77   | 119 C (POR)           | CPU被动降温点                                                                                                                                                                                                                                                                                                               |
| Passive TC1 Value                                                             |              |        |                       | 设置ACPI被动冷却公式的TC1值，取值范围为1-16                                                                                                                                                                                                                                                                                 |
| Passive TC2 Value                                                             |              |        |                       | 设置ACPI被动冷却公式的TC2值，取值范围为1-16                                                                                                                                                                                                                                                                                 |
| Passive TSP Value                                                             |              |        |                       | ACPI被动冷却公式的TSP值，表示在启用被动冷却时操作系统读取温度的频率，单位为秒                                                                                                                                                                                                                                               |
| Active Trip Points                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Passive Trip Points                                                           | 0x2B7        | 0x1    | Enabled               | 打开Passive Trip                                                                                                                                                                                                                                                                                                            |
| Critical Trip Points                                                          | 0x2B8        | 0x1    | Enabled               | 设置操作系统关闭的温度阈值。`_HOT`对象声明OSPM可以选择将系统转换到S4休眠状态的临界温度（如果支持），`_CRT`对象声明OSPM必须执行紧急关机的临界温度                                                                                                                                                                            |
| PCH Thermal Device                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PCH Temp Read                                                                 |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CPU Energy Read                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CPU Temp Read                                                                 |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Alert Enable Lock                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **DPTF Configuration**                                                        |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| DPTF                                                                          | 0x2C5        | 0x0    | Disabled              | 动态平台和热框架DPTF（Dynamic Platform and Thermal Framework）会在高温下启动，也会被显卡状态、供电模块温度等触发                                                                                                                                                                                                            |
| DPTF Configuration                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Processor Thermal Device                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Active Thermal Trip Point                                                     |              |        |                       | 控制ACPI Active Thermal Trip Point的温度。取值范围为0-127，默认值为90，0表示禁用                                                                                                                                                                                                                                            |
| Passive Thermal Trip Point                                                    |              |        |                       | 控制ACPI Passive Thermal Trip Point的温度。取值范围为0-127，默认值为100，0表示禁用                                                                                                                                                                                                                                          |
| Critical Thermal Trip Point                                                   |              |        |                       | 控制ACPI Critical Thermal Trip Point的温度。取值范围为0-127，默认值为105，0表示禁用                                                                                                                                                                                                                                         |
| S3/CS Thermal Trip Point                                                      |              |        |                       | 控制ACPI S3/CS Thermal Trip Point的温度。取值范围为0-127，默认值为110，0表示禁用                                                                                                                                                                                                                                            |
| Hot Thermal Trip Point                                                        |              |        |                       | 控制ACPI Hot Thermal Trip Point的温度。取值范围为0-127，默认值为105，0表示禁用                                                                                                                                                                                                                                              |
| Thermal Sampling Period                                                       |              |        |                       | 轮询间隔，以1/10秒为单位。取值范围为0-100，默认值为0，表示让驱动程序使用中断。采样周期粒度为0.1秒，若采样周期是30秒，那么_TSP需要为300秒                                                                                                                                                                                    |
| PPCC Step Size                                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Minimum Power Limit 0-2                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CLPO Enable                                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CLPO Start PState                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CLPO Step Size                                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CLPO Power Control                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CLPO Performance Control                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| ConfigTDP                                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| LPM                                                                           |              |        |                       | 支持LPM（link power management）以获得更好的省电效果                                                                                                                                                                                                                                                                        |
| **Policy Configuration**                                                      |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Active Policy                                                                 |              |        |                       | 启用/禁用激活策略算法，该算法通过散热而非限制功率来冷却设备                                                                                                                                                                                                                                                                 |
| Passive Policy                                                                |              |        |                       | 设置被动策略，该策略负责限制功率和组件性能                                                                                                                                                                                                                                                                                  |
| TRT Revision                                                                  |              |        |                       | 设置热关系表(TRT)，通知操作系统每个设备对每个热区的相对热贡献                                                                                                                                                                                                                                                               |
| Critical Policy                                                               |              |        |                       | 启用/禁用关键策略。在平台达到临界温度的情况下，临界策略负责关闭系统                                                                                                                                                                                                                                                         |
| Cooling Mode Policy                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Platform Noise Mitigation                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Power Boss                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Hardware Duty Cycling                                                         |              |        |                       | HDC（Hardware Duty cycle）让CPU自动进入空闲状态                                                                                                                                                                                                                                                                             |
| Adaptive Performance                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Virtual Sensor                                                                |              |        |                       | 虚拟传感器                                                                                                                                                                                                                                                                                                                  |
| PID Policy                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Design Variable 0-5                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PPCC Object                                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PDRT Object                                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| ARTG Object                                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PMAX Object                                                                   |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| _TMP 1-8 Object                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Platform Settings**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Firmware Configuration                                                        |              |        |                       | 固件配置                                                                                                                                                                                                                                                                                                                    |
| PS2 Keyboard and Mouse                                                        |              |        |                       | PS2键盘和鼠标                                                                                                                                                                                                                                                                                                               |
| Pmic Vcc IO Level                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Pmic Vddq Level                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| HEBC value                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| SLP_S0# Voltage Margining                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Audio Connector                                                               |              |        |                       | 音频连接器                                                                                                                                                                                                                                                                                                                  |
| Enable EInk DFU device                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Power Sharing Manager                                                         |              |        |                       | 电源共享管理器                                                                                                                                                                                                                                                                                                              |
| Domain Type SPLC 1                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Default Power Limit 1 SPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Default Time Window 1 SPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Domain Type SPLC 2                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Default Power Limit 2 SPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Default Time Window 2 SPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Domain Type DPLC 1                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Domain Preference DPLC 1                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Power Limit Index 1 DPLC                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Default Power Limit 1 DPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Default Time Window 1 DPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Minimum Power Limit 1 DPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Maximum Power Limit 1 DPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Maximum Time Window 1 DPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Domain Type DPLC 2                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Domain Preference DPLC 2                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Power Limit Index 2 DPLC                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Default Power Limit 2 DPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Default Time Window 2 DPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Minimum Power Limit 2 DPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Maximum Power Limit 2 DPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Default Time Window 2 SPLC                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Wireless device                                                               |              |        |                       | 无线设备                                                                                                                                                                                                                                                                                                                    |
| Domain Type SPLC 1-3                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Default Power Limit                                                           |              |        |                       | 默认功率限制                                                                                                                                                                                                                                                                                                                |
| Default Time Window                                                           |              |        |                       | 默认时间窗口大小                                                                                                                                                                                                                                                                                                            |
| TRxDelay_A                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| TRxCableLength_A                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| TRxDelay_B                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| TRxCableLength_B                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Domain Type                                                                   |              |        |                       | 域名类型                                                                                                                                                                                                                                                                                                                    |
| Country Identifier                                                            |              |        |                       | 国家代号                                                                                                                                                                                                                                                                                                                    |
| **Realsense 3D Camera Settings**                                              |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| RTD3 Camera Support                                                           |              |        |                       | RTD3相机支持                                                                                                                                                                                                                                                                                                                |
| Select Camera                                                                 |              |        |                       | 选择相机                                                                                                                                                                                                                                                                                                                    |
| Delay needed for Ivcam power on                                               |              |        |                       | Ivcam相机打开时需要延迟                                                                                                                                                                                                                                                                                                     |
| Delay needed for Ivcam power off                                              |              |        |                       | Ivcam相机关闭时需要延迟                                                                                                                                                                                                                                                                                                     |
| Rotation                                                                      |              |        |                       | 旋转                                                                                                                                                                                                                                                                                                                        |
| DFU support                                                                   |              |        |                       | DFU支持                                                                                                                                                                                                                                                                                                                     |
| Wake support                                                                  |              |        |                       | 唤醒支持                                                                                                                                                                                                                                                                                                                    |
| Delay needed for DS camera power on                                           |              |        |                       | DS相机打开时需要延迟                                                                                                                                                                                                                                                                                                        |
| Delay needed for DS camera power off                                          |              |        |                       | DS相机打开时需要延迟                                                                                                                                                                                                                                                                                                        |
| Rotation                                                                      |              |        |                       | 旋转                                                                                                                                                                                                                                                                                                                        |
| **OverClocking Performance Menu**                                             |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| OverClocking Feature                                                          | 0x696        | 0x1    | Enabled               | 超频特性                                                                                                                                                                                                                                                                                                                    |
| WDT Enable                                                                    |              |        |                       | 看门狗定时器                                                                                                                                                                                                                                                                                                                |
| XTU Interface                                                                 |              |        |                       | XTU界面                                                                                                                                                                                                                                                                                                                     |
| BCLK Aware Adaptive Voltage                                                   |              |        |                       | 外频/电压比率调整，启用时CPU会计算外频与电压之间的比率，通常适用于外频超频而防止电压过高                                                                                                                                                                                                                                    |
| **Processor**                                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Core Max OC Ratio                                                             |              |        |                       | 内核最大OC倍频                                                                                                                                                                                                                                                                                                              |
| Core Voltage Mode                                                             |              |        |                       | CPU核心电压模式                                                                                                                                                                                                                                                                                                             |
| Core Voltage Override                                                         |              |        |                       | CPU核心电压固定值                                                                                                                                                                                                                                                                                                           |
| Core Extra Turbo Voltage                                                      |              |        |                       | CPU核心电压睿频固定值                                                                                                                                                                                                                                                                                                       |
| Core Voltage Offset                                                           |              |        |                       | 加压/减压                                                                                                                                                                                                                                                                                                                   |
| Offset Prefix                                                                 |              |        | -                     | 偏移量正负                                                                                                                                                                                                                                                                                                                  |
| Ring Down Bin                                                                 |              |        |                       | AUTO时会降低超频后满载时的缓存频率，超缓存时需要关闭                                                                                                                                                                                                                                                                        |
| Ring Max OC Ratio                                                             |              |        |                       | 设置CPU Ring的最大超频比率                                                                                                                                                                                                                                                                                                  |
| Ring Min OC Ratio                                                             |              |        |                       | 设置CPU Ring的最小超频比率                                                                                                                                                                                                                                                                                                  |
| AVX Ratio Offset                                                              | 0x6A1        | 0x3    | 3                     | AVX倍频补偿，可以大幅度提高超频稳定性，建议设置在3。设置在1可以让CPU在浮点运算的时候比整数低100Mhz，若CPU一般情况下跑在4.5Ghz，那么在跑AVX指令集时就会降低到4.4Ghz，如果设置为3就会降低300Mhz。适当降低AVX频率有助于超频成功率的提升                                                                                        |
| **GT**                                                                        |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| GT OverClocking Frequency                                                     |              |        |                       | 核显超频频率                                                                                                                                                                                                                                                                                                                |
| GT Voltage Mode                                                               |              |        |                       | 核显电压模式                                                                                                                                                                                                                                                                                                                |
| GT Voltage Override                                                           |              |        |                       | 核显电压固定值                                                                                                                                                                                                                                                                                                              |
| GT Extra Turbo Voltage                                                        |              |        |                       | 核显电压睿频固定值                                                                                                                                                                                                                                                                                                          |
| GT Voltage Offset                                                             |              |        |                       | 核显电压偏移值                                                                                                                                                                                                                                                                                                              |
| Offset Prefix                                                                 |              |        |                       | 偏移量正负                                                                                                                                                                                                                                                                                                                  |
| **Uncore**                                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Uncore Voltage Offset                                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Offset Prefix                                                                 |              |        |                       | 偏移量正负                                                                                                                                                                                                                                                                                                                  |
| **Memory Overclocking Menu**                                                  |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Realtime Memory Timing                                                        |              |        |                       | 在BIOS阶段之后调整内存计时                                                                                                                                                                                                                                                                                                  |
| Memory profile                                                                |              |        |                       | 内存配置文件                                                                                                                                                                                                                                                                                                                |
| Memory Reference Clock                                                        |              |        |                       | 内存参考源时钟，通常可以设置为Auto。内存频率是按照与CPU外频和Home Agent的比率来实现的，100Mhz可以实现DDR 3000、3100等频率，133Mhz可以实现DDR 3333等频率                                                                                                                                                                     |
| Memory Ratio                                                                  |              |        |                       | 内存频率                                                                                                                                                                                                                                                                                                                    |
| QCLK Odd Ratio                                                                |              |        |                       | 间隔频率                                                                                                                                                                                                                                                                                                                    |
| tCL                                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| tRCD/tRP                                                                      |              |        |                       | tRCD（RAS to CAS Delay）为内存行地址传输到列地址的延迟时间，tRP（RAS Precharge Time）为内存行地址选通脉冲预充电时间                                                                                                                                                                                                         |
| tRAS                                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| tCWL                                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| tFAW                                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| tREFI                                                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| tRFC                                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| tRRD                                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| tRTP                                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| tWR                                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| tWTR                                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| NMode                                                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Memory Voltage                                                                |              |        |                       | 内存电压                                                                                                                                                                                                                                                                                                                    |
| DllBwEn[0]-[3]                                                                |              |        |                       | 设置DllBwEn值                                                                                                                                                                                                                                                                                                               |
| **Voltage PLL Trim Controls**                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Core PLL Voltage Offset                                                       |              |        |                       | 设置CPU锁相环电压偏移值，用于在极端超频条件下增加核心频率的范围，以15mV为单位。取值范围为0-63，输入0以使用出厂默认值                                                                                                                                                                                                        |
| GT PLL Voltage Offset                                                         |              |        |                       | 核心显卡锁相环电压偏移量                                                                                                                                                                                                                                                                                                    |
| Ring PLL Voltage Offset                                                       |              |        |                       | 环形总线锁相环电压                                                                                                                                                                                                                                                                                                          |
| System Agent PLL Voltage Offset                                               |              |        |                       | Home Agent锁相环电压                                                                                                                                                                                                                                                                                                        |
| Memory Controller PLL Voltage Offset                                          |              |        |                       | 内存控制器锁相环电压                                                                                                                                                                                                                                                                                                        |
| **Intel ICC**                                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| ICC/OC Watchdog Timer                                                         |              |        |                       | ICC/OC监视时钟设置                                                                                                                                                                                                                                                                                                          |
| ICC Locks after EOP                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| ICC Profile                                                                   |              |        |                       | ICC色彩特性文件                                                                                                                                                                                                                                                                                                             |
| ICC PLL Shutdown                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **DMI CLOCK placeholder for clock 2/3 title**                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Clock Frequency                                                               |              |        |                       | 时钟频率                                                                                                                                                                                                                                                                                                                    |
| Bclk Change Type                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Spread %                                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Trusted Computing**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Security Device Support                                                       |              |        |                       | 安全保护设备支持                                                                                                                                                                                                                                                                                                            |
| SHA-1 PCR Bank                                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| SHA256 PCR Bank                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| SHA384 PCR Bank                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| SHA512 PCR Bank                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| SM3_256 PCR Bank                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| TPM State                                                                     |              |        |                       | TPM状态                                                                                                                                                                                                                                                                                                                     |
| Pending operation                                                             |              |        |                       | 计划安全设备的操作                                                                                                                                                                                                                                                                                                          |
| Platform Hierarchy                                                            |              |        |                       | 平台层次结构                                                                                                                                                                                                                                                                                                                |
| Storage Hierarchy                                                             |              |        |                       | 存储层次结构                                                                                                                                                                                                                                                                                                                |
| Endorsement Hierarchy                                                         |              |        |                       | 支持层级结构                                                                                                                                                                                                                                                                                                                |
| TPM2.0 UEFI Spec Version                                                      |              |        |                       | TCG2规范版本支持。TCG_1_2为Win8/Win10兼容模式，TCG_2为支持新的TCG2协议和Win10或更高版本的事件格式                                                                                                                                                                                                                           |
| Physical Presence Spec Version                                                |              |        |                       | 选择告诉操作系统支持的PPI规范版本（1.2或1.3）。部分HCK测试可能不支持1.3                                                                                                                                                                                                                                                     |
| TPM 20 InterfaceType                                                          |              |        |                       | TPM 20交互模式                                                                                                                                                                                                                                                                                                              |
| Device Select                                                                 |              |        |                       | 选择设备                                                                                                                                                                                                                                                                                                                    |
| **ACPI Settings**                                                             |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Enable ACPI Auto Configuration                                                |              |        |                       | 开启ACPI自动配置                                                                                                                                                                                                                                                                                                            |
| Enable Hibernation                                                            |              |        |                       | 开启休眠                                                                                                                                                                                                                                                                                                                    |
| ACPI Sleep State                                                              |              |        |                       | ACPI睡眠状态                                                                                                                                                                                                                                                                                                                |
| Lock Legacy Resources                                                         |              |        |                       | 锁定旧版资源                                                                                                                                                                                                                                                                                                                |
| S3 Video Repost                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Intel Bios Guard Support**                                                  |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Intel Bios Guard Support                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **AMI Graphic Output Protocol Policy**                                        |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Output Select                                                                 |              |        |                       | 选择输出                                                                                                                                                                                                                                                                                                                    |
| Brightness Setting                                                            |              |        |                       | 亮度调整                                                                                                                                                                                                                                                                                                                    |
| BIST Enable                                                                   |              |        |                       | 内建自检                                                                                                                                                                                                                                                                                                                    |
| **Network Stack Configuration**                                               |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Network Stack                                                                 |              |        |                       | UEFI网络堆栈功能                                                                                                                                                                                                                                                                                                            |
| Ipv4 PXE Support                                                              |              |        |                       | Ipv4 PXE启动项支持                                                                                                                                                                                                                                                                                                          |
| Ipv4 HTTP Support                                                             |              |        |                       | Ipv4 HTTP启动项支持                                                                                                                                                                                                                                                                                                         |
| Ipv6 PXE Support                                                              |              |        |                       | Ipv6 PXE启动项支持                                                                                                                                                                                                                                                                                                          |
| Ipv6 HTTP Support                                                             |              |        |                       | Ipv6 HTTP启动项支持                                                                                                                                                                                                                                                                                                         |
| IP6 Configuration Policy                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PXE boot wait time                                                            |              |        |                       | PXE启动的等待时间                                                                                                                                                                                                                                                                                                           |
| Media detect count                                                            |              |        |                       | 侦测媒体数量                                                                                                                                                                                                                                                                                                                |
| **CSM Configuration**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| CSM Support                                                                   |              |        |                       | 兼容性支持模块（CSM）是UEFI固件的一个组件，它通过模拟BIOS环境提供传统BIOS兼容性，允许传统操作系统和一些不支持UEFI的选项ROM继续使用                                                                                                                                                                                          |
| GateA20 Active                                                                |              |        |                       | 设置GateA20的动作模式                                                                                                                                                                                                                                                                                                       |
| Option ROM Messages                                                           |              |        |                       | [Force BIOS]选购设备固件程序会在开机显示，[Keep Current]开箱时只显示ASUS标志                                                                                                                                                                                                                                                |
| INT19 Trap Response                                                           |              |        |                       | 对INT19陷阱的反馈                                                                                                                                                                                                                                                                                                           |
| Boot option filter                                                            |              |        |                       | 选择Legacy/UEFI启动方式                                                                                                                                                                                                                                                                                                     |
| Network                                                                       |              |        |                       | 网络                                                                                                                                                                                                                                                                                                                        |
| Storage                                                                       |              |        |                       | 存储                                                                                                                                                                                                                                                                                                                        |
| Video                                                                         |              |        |                       | 视频                                                                                                                                                                                                                                                                                                                        |
| Other PCI devices                                                             |              |        |                       | 其余PCI设备                                                                                                                                                                                                                                                                                                                 |
| **SDIO Configuration**                                                        |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| SDIO Access Mode                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Switchable Graphics**                                                       |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| SG Mode Select                                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **USB Configuration**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| USB Support                                                                   |              |        |                       | USB支持                                                                                                                                                                                                                                                                                                                     |
| Legacy USB Support                                                            |              |        |                       | 传统USB支持                                                                                                                                                                                                                                                                                                                 |
| USB 2.0 Controller Mode                                                       |              |        |                       | USB 2.0控制器模式                                                                                                                                                                                                                                                                                                           |
| XHCI Legacy Support                                                           |              |        |                       | 传统XHCI支持                                                                                                                                                                                                                                                                                                                |
| XHCI Hand-off                                                                 |              |        |                       | 让操作系统接管XHCI控制器                                                                                                                                                                                                                                                                                                    |
| EHCI Hand-off                                                                 |              |        |                       | 让操作系统接管EHCI控制器                                                                                                                                                                                                                                                                                                    |
| USB Mass Storage Driver Support                                               |              |        |                       | 大容量USB驱动支持                                                                                                                                                                                                                                                                                                           |
| Port 60/64 Emulation                                                          |              |        |                       | 60/64端口模拟                                                                                                                                                                                                                                                                                                               |
| USB transfer time-out                                                         |              |        |                       | USB传输超时                                                                                                                                                                                                                                                                                                                 |
| Device reset time-out                                                         |              |        |                       | 设备重置超时                                                                                                                                                                                                                                                                                                                |
| Device power-up delay                                                         |              |        |                       | 设备开启延迟                                                                                                                                                                                                                                                                                                                |
| Device power-up delay in seconds                                              |              |        |                       | 设备开启延迟（以秒为单位）                                                                                                                                                                                                                                                                                                  |
| **Demo Board**                                                                |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| CRB Test                                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **System Agent (SA) Configuration**                                           |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Stop Grant Configuration                                                      |              |        |                       | 停止许可配置                                                                                                                                                                                                                                                                                                                |
| Number of Stop Grant Cycles                                                   |              |        |                       | 停止许可循环的次数                                                                                                                                                                                                                                                                                                          |
| VT-d                                                                          |              |        |                       | 开启或关闭CPU的I/O虚拟化功能                                                                                                                                                                                                                                                                                                |
| CHAP Device (B0:D7:F0)                                                        |              |        |                       | 启用/禁用SA CHAP设备                                                                                                                                                                                                                                                                                                        |
| Thermal Device (B0:D4:F0)                                                     |              |        |                       | 启用/禁用SA Thermal设备                                                                                                                                                                                                                                                                                                     |
| GMM Device (B0:D8:F0)                                                         |              |        |                       | 启用/禁用SA GMM设备                                                                                                                                                                                                                                                                                                         |
| CRID Support                                                                  |              |        |                       | 启用兼容的修订ID（Compatible Revision ID）                                                                                                                                                                                                                                                                                  |
| Above 4GB MMIO BIOS assignment                                                |              |        | Enabled               | 4G以上内存映射的IO BIOS分配                                                                                                                                                                                                                                                                                                 |
| X2APIC Opt Out                                                                |              |        |                       | 防止操作系统启用扩展xAPIC（x2APIC）模式                                                                                                                                                                                                                                                                                     |
| SKY CAM Device (B0:D5:F0)                                                     |              |        |                       | 启用/禁用SA SKY CAM设备                                                                                                                                                                                                                                                                                                     |
| eDRAM Mode                                                                    |              |        |                       | 嵌入式DRAM（Embedded DRAM）模式                                                                                                                                                                                                                                                                                             |
| **Graphics Configuration**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Graphics Turbo IMON Current                                                   |              |        |                       | 图形Turbo IMON当前值，取值范围为14-31                                                                                                                                                                                                                                                                                       |
| Skip Scaning of External Gfx Card                                             | 0x8AE        | 0x0    | Disabled              | 跳过外部显卡扫描                                                                                                                                                                                                                                                                                                            |
| Primary Display                                                               | 0x80D        | 0x4    | SG                    | 不同显示类型的优先级。Auto为自动选择显示类型，IGFX为从主板上的核心显卡输出端口输出视频，PEG为从CPU上的PCIe插槽输出视频，PCI为从南桥上的PCIe插槽输出视频，SG为自动切换（此模式两种显示都会保留）                                                                                                                             |
| Select PCIE Card                                                              | 0x80E        | 0x2    | Auto                  | 选择PCIE卡                                                                                                                                                                                                                                                                                                                  |
| SG Delay After Power Enable                                                   |              |        |                       | SG模式在开机后的时延（毫秒）                                                                                                                                                                                                                                                                                                |
| SG Delay After Hold Reset                                                     |              |        |                       | SG模式在重启后的时延（毫秒）                                                                                                                                                                                                                                                                                                |
| Internal Graphics                                                             | 0x813        | 0x2    | Auto                  | 设置内部集成显卡显示                                                                                                                                                                                                                                                                                                        |
| GTT Size                                                                      | 0x74A        | 0x3    |                       | IGD的Gtt内存大小                                                                                                                                                                                                                                                                                                            |
| Aperture Size                                                                 | 0x74B        | 0x1    | 256MB                 | 显示数据处理的临时缓冲区大小                                                                                                                                                                                                                                                                                                |
| DVMT Pre-Allocated                                                            | 0x7E8        | 0x2    | 64M                   | 动态共享显存预设值                                                                                                                                                                                                                                                                                                          |
| DVMT Total Gfx Mem                                                            | 0x7E9        | 0x2    | 256M                  | 设置动态共享显存分配最大值。取值范围为128M/256M/MAX，默认为256M。MAX值与驱动及操作系统有关，在windows操作系统下，为主板内存的一半。由于集显本身是没有内存的，所以需要共享主板的内存。当GPU不再需要占用这些内存资源时，会自动归还给系统内存，将其返还操作系统供其它应用程序或系统功能使用                                    |
| Intel Graphics Pei Display Peim                                               |              |        |                       | 早期PEI阶段显示控制开关                                                                                                                                                                                                                                                                                                     |
| Gfx Low Power Mode                                                            |              |        |                       | 仅适用于SFF                                                                                                                                                                                                                                                                                                                 |
| ALS Support                                                                   |              |        |                       | 仅适用于ACPI。Legacy为ALS通过IGD INT10功能支持，ACPI为ALS通过ACPI ALS驱动支持                                                                                                                                                                                                                                               |
| VDD Enable                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PM Support                                                                    |              |        |                       | 电源管理控制开关                                                                                                                                                                                                                                                                                                            |
| PAVP Enable                                                                   |              |        |                       | Intel智能音频技术                                                                                                                                                                                                                                                                                                           |
| Cdynmax Clamping Enable                                                       |              |        |                       | 激活Cdynmas Clamping                                                                                                                                                                                                                                                                                                        |
| Cd Clock Frequency                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| IUER Button Enable                                                            |              |        |                       | IUER按钮功能                                                                                                                                                                                                                                                                                                                |
| **LCD Control**                                                               |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Primary IGFX Boot Display                                                     |              |        |                       | 在POST期间激活的主视频设备                                                                                                                                                                                                                                                                                                  |
| Secondary IGFX Boot Display                                                   |              |        |                       | 在POST期间激活的副视频设备                                                                                                                                                                                                                                                                                                  |
| LCD Panel Type                                                                |              |        |                       | LCD面板类型                                                                                                                                                                                                                                                                                                                 |
| Panel Scaling                                                                 |              |        |                       | 面板等级                                                                                                                                                                                                                                                                                                                    |
| Backlight Control                                                             |              |        |                       | 背光控制                                                                                                                                                                                                                                                                                                                    |
| Active LFP                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Backlight Brightness                                                          |              |        |                       | 背光亮度                                                                                                                                                                                                                                                                                                                    |
| **Intel(R) Ultrabook Event Support**                                          |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| IUER Slate Enable                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Slate Mode boot value                                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Slate Mode on S3 and S4 resume                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| IUER Dock Enable                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Dock Mode boot value                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Dock Mode upon S3 and S4 resume                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **External Gfx Card Primary Display Configuration**                           |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Primary PEG                                                                   |              |        |                       | 主PEG                                                                                                                                                                                                                                                                                                                       |
| Primary PCIE                                                                  |              |        |                       | 主PCIE                                                                                                                                                                                                                                                                                                                      |
| **DMI/OPI Configuration**                                                     |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| DMI Max Link Speed                                                            |              |        |                       | 设置DMI速度                                                                                                                                                                                                                                                                                                                 |
| DMI Vc1 Control                                                               |              |        |                       | DMI 1虚拟通道控制                                                                                                                                                                                                                                                                                                           |
| DMI Vcm Control                                                               |              |        |                       | DMI虚拟通道M控制                                                                                                                                                                                                                                                                                                            |
| Program Static Phase1 Eq                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| DMI Link ASPM Control                                                         |              |        |                       | 设置DMI Link上北桥与南桥的ASPM（Active State Power Management）功能                                                                                                                                                                                                                                                         |
| DMI Extended Sync Control                                                     |              |        |                       | DMI扩展同步只能在调试或测试环境中使用，并且在启用L0s时可能导致锁定                                                                                                                                                                                                                                                          |
| DMI De-emphasis Control                                                       |              |        |                       | DMI减重控制                                                                                                                                                                                                                                                                                                                 |
| DMI IOT                                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Lane 0-3                                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Gen3 RxCTLE Control**                                                       |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Bundle0/1                                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **PEG 0:1:0**                                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| **PEG 0:1:1**                                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| **PEG 0:1:2**                                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Enable Root Port                                                              |              |        |                       | 开启根端口                                                                                                                                                                                                                                                                                                                  |
| Max Link Speed                                                                |              |        |                       | 最大链路速度                                                                                                                                                                                                                                                                                                                |
| Max Link Width                                                                |              |        |                       | 最大链路带宽                                                                                                                                                                                                                                                                                                                |
| Power Down Unused Lanes                                                       |              |        |                       | 关闭未使用的链路                                                                                                                                                                                                                                                                                                            |
| Gen3 Eq Phase 2                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Gen3 Eq Phase 3 Method                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| ASPM                                                                          |              |        |                       | ASPM（Active State Power Management）让PCIe SSD在某种情况下，能够从工作模式（D0 state）通过把自身PCIe链路切换到低功耗模式，并且通知对方也这样做，从而达到降低整条链路功耗的目的                                                                                                                                             |
| ASPM L0s                                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| De-emphasis Control                                                           |              |        |                       | 取消PEG的控制                                                                                                                                                                                                                                                                                                               |
| OBFF                                                                          |              |        |                       | OBFF（Optimized Buffer Flush/Fill mechanism in PC technology）启用时，主机软件能够将系统状态传输至具有主线总控能力的下游装置，通过它们优化传输排程，并使系统在节能模式下工作的时间更长                                                                                                                                      |
| LTR                                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PEG0 Slot Power Limit Value                                                   |              |        |                       | PEG0插槽电源限定值                                                                                                                                                                                                                                                                                                          |
| PEG0 Slot Power Limit Scale                                                   |              |        |                       | Slot Power Limit Value倍率                                                                                                                                                                                                                                                                                                  |
| PEG0 Physical Slot Number                                                     |              |        |                       | PEG0物理插槽编号                                                                                                                                                                                                                                                                                                            |
| PEG0 Hotplug                                                                  |              |        |                       | PEG0热插拔                                                                                                                                                                                                                                                                                                                  |
| **PEG Port Configuration**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| PEG0/PEG1/PEG2 Max Payload size                                               |              |        |                       | 设定PEG0/PEG1/PEG2最大覆载量                                                                                                                                                                                                                                                                                                |
| Program PCIe ASPM after OpROM                                                 |              |        |                       | 编程PCIe ASPM的时间                                                                                                                                                                                                                                                                                                         |
| Program Static Phase1 Eq                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Always Attempt SW EQ                                                          |              |        |                       | 总是尝试SW EQ                                                                                                                                                                                                                                                                                                               |
| Number of Presets to test                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Allow PERST# GPIO Usage                                                       |              |        |                       | 启用/禁用基于GPIO的重置                                                                                                                                                                                                                                                                                                     |
| SW EQ Enable VOC                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Jitter Dwell Time                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Jitter Error Target                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| VOC Dwell Time                                                                |              |        |                       | VOC存在时间                                                                                                                                                                                                                                                                                                                 |
| VOC Error Target                                                              |              |        |                       | VOC错误目标                                                                                                                                                                                                                                                                                                                 |
| Generate BDAT PEG Margin Data                                                 |              |        |                       | 生成BDAT PEG边缘数据                                                                                                                                                                                                                                                                                                        |
| PCIe Rx CEM Test Mode                                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PEG Lane number for Test                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Non-Protocol Awareness                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PCIe Spread Spectrum Clocking                                                 |              |        |                       | 启用PCIe时钟芯片扩频功能                                                                                                                                                                                                                                                                                                    |
| Gen3 Root Port Preset                                                         |              |        |                       | 根端口Gen3均衡的预设值，取值范围为1-11                                                                                                                                                                                                                                                                                      |
| Gen3 Endpoint Preset                                                          |              |        |                       | Gen3均衡的端点预设值，取值范围为0-10                                                                                                                                                                                                                                                                                        |
| Gen3 Endpoint Hint                                                            |              |        |                       | Gen3均衡的端点提示值，取值范围为0-10                                                                                                                                                                                                                                                                                        |
| Lane 0-15                                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PEG Gen3 RxCTLE Control                                                       |              |        |                       | PEG Gen3 RxCTLE for Bundle0(Lane0, Lane1)，取值范围0-255                                                                                                                                                                                                                                                                    |
| Bundle0-7                                                                     |              |        |                       | Gen3 RxCTLE setting for Bundle0 (Lane0, Lane1)/ Bundle1 (Lane2, Lane3)/ ... / Bundle7 (Lane14, Lane15)                                                                                                                                                                                                                      |
| RxCTLE Override                                                               |              |        |                       | 启用时它将覆盖PEG的RxCTLE自适应行为                                                                                                                                                                                                                                                                                         |
| **PEG Port Feature Configuration**                                            |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Detect Non-Compliance Device                                                  |              |        |                       | 检测不兼容设备                                                                                                                                                                                                                                                                                                              |
| **PCH-IO Configuration**                                                      |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| DCI enable (HDCIEN)                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| USB/UART                                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Debug Port Selection                                                          |              |        |                       | 调试端口选择                                                                                                                                                                                                                                                                                                                |
| GNSS                                                                          |              |        |                       | 北斗                                                                                                                                                                                                                                                                                                                        |
| GNSS Device Model                                                             |              |        |                       | 北斗设备型号                                                                                                                                                                                                                                                                                                                |
| PCH LAN Controller                                                            |              |        |                       | PCH LAN控制器                                                                                                                                                                                                                                                                                                               |
| Sensor Hub Type                                                               |              |        |                       | 传感器坞类型                                                                                                                                                                                                                                                                                                                |
| DeepSx Power Policies                                                         |              |        |                       | 深度睡眠状态下的电源管理策略                                                                                                                                                                                                                                                                                                |
| LAN Wake From DeepSx                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Wake on LAN Enable                                                            |              |        |                       | 在Moff时启用PCH内部局域网唤醒                                                                                                                                                                                                                                                                                               |
| SLP_LAN# Low on DC Power                                                      |              |        |                       | 当主机WoL和可管理性硬件被禁用时，本选项可以用来关闭不需要打开的电源轨                                                                                                                                                                                                                                                       |
| K1 off                                                                        |              |        |                       | 启用/禁用K1 off特性（CLKREQ）                                                                                                                                                                                                                                                                                               |
| Wake on WLAN and BT Enable                                                    |              |        |                       | 开启/关闭PCI Express无线局域网和蓝牙以唤醒系统                                                                                                                                                                                                                                                                              |
| DeepSx Wake on WLAN and BT Enable                                             |              |        |                       | 开启/关闭PCI Express无线局域网和蓝牙，使系统从DeepSx唤醒                                                                                                                                                                                                                                                                    |
| Disable DSX ACPRESENT PullDown                                                |              |        |                       | 在DeepSX或G3退出时启动或关闭PCH internal ACPRESENT                                                                                                                                                                                                                                                                          |
| CLKRUN# logic                                                                 |              |        |                       | 开启或关闭CLKRUN#逻辑以停止PCI时脉                                                                                                                                                                                                                                                                                          |
| Serial IRQ Mode                                                               |              |        |                       | 串行IRQ模式。选择Quit为需要时才激活，选择Continuous为一直激活                                                                                                                                                                                                                                                               |
| Port 61h Bit-4 Emulation                                                      |              |        |                       | 61h端口的4 bit模拟                                                                                                                                                                                                                                                                                                          |
| High Precision Timer                                                          |              |        |                       | 高精度计时器                                                                                                                                                                                                                                                                                                                |
| State After G3                                                                |              |        |                       | 指定在电源故障后重新应用电源时切换到什么状态                                                                                                                                                                                                                                                                                |
| Port 80h Redirection                                                          |              |        |                       | 80h端口重定向                                                                                                                                                                                                                                                                                                               |
| Enhance Port 80h LPC Decoding                                                 |              |        |                       | 80h端口的LPC解码增强                                                                                                                                                                                                                                                                                                        |
| Compatible Revision ID                                                        |              |        |                       | 兼容的修改ID                                                                                                                                                                                                                                                                                                                |
| PCH Cross Throttling                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Disable Energy Reporting                                                      |              |        |                       | 禁止电能报告                                                                                                                                                                                                                                                                                                                |
| Enable TCO Timer                                                              |              |        |                       | 开启TCO计时器                                                                                                                                                                                                                                                                                                               |
| Pcie Pll SSC                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| IOAPIC 24-119 Entries                                                         |              |        |                       | IRQ24-119硬件中断性访问入口，建议保持开启以提升芯片组的响应速度                                                                                                                                                                                                                                                             |
| Unlock PCH P2SB                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PMC READ DISABLE                                                              |              |        |                       | 禁止对PMC的读操作                                                                                                                                                                                                                                                                                                           |
| Flash Protection Range Registers (FPRR)                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| SPD Write Disable                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| ChipsetInit HECI Message                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Bypass ChipsetInit sync reset                                                 |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **PCI Express Configuration**                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| PCI Express Clock Gating                                                      |              |        |                       | PCI Express根端口时钟门控                                                                                                                                                                                                                                                                                                   |
| Legacy IO Low Latency                                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| DMI Link ASPM Control                                                         |              |        |                       | DMI链路的有源状态控制电源管理                                                                                                                                                                                                                                                                                               |
| Port8xh Decode                                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Port8xh Decode Port#                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Peer Memory Write Enable                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PCIe-USB Glitch W/A                                                           |              |        |                       | 连接到PCIE/PEG接口背后的，对于不良USB设备的PCIe-USB glitch W/A                                                                                                                                                                                                                                                              |
| PCIe function swap                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **PCI Express Gen3 Eq Lanes**                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| PCIE1-20  Cm                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PCIE1-20  Cp                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Override SW EQ settings**                                                   |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Coeff0-4 Cm                                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Coeff0-4 Cp                                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **PCI Express Root Port 1-24**                                                |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| PCI Express Root Port 1-24                                                    |              |        | Enabled               | 控制PCI Express根端口                                                                                                                                                                                                                                                                                                       |
| Topology                                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| ASPM                                                                          |              |        |                       | 控制PCIe Active State电源管理设置（ASPM，Active State Power Management）                                                                                                                                                                                                                                                    |
| L1 Substates                                                                  |              |        |                       | PCI Express L1子状态设置                                                                                                                                                                                                                                                                                                    |
| Gen3 Eq Phase3 Method                                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| UPTP                                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| DPTP                                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| ACS                                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| URR                                                                           |              |        |                       | PCI Express不支持的请求报告（URR，Unsupported Request Reporting）                                                                                                                                                                                                                                                           |
| FER                                                                           |              |        |                       | PCI Express设备致命错误报告（FER，Fatal Error Reporting）                                                                                                                                                                                                                                                                   |
| NFER                                                                          |              |        |                       | PCI Express设备非致命错误报告（NFER，Non-Fatal Error Reporting）                                                                                                                                                                                                                                                            |
| CER                                                                           |              |        |                       | PCI Express设备可纠正错误报告（CER，Correctable Error Reporting）                                                                                                                                                                                                                                                           |
| CTO                                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| SEFE                                                                          |              |        |                       | Root PCI Express系统错误致命错误（SEFE，System Error on Fatal Error）                                                                                                                                                                                                                                                       |
| SENFE                                                                         |              |        |                       | 非致命错误的PCI Express系统错误（SENFE，System Error on Non-Fatal Error）                                                                                                                                                                                                                                                   |
| SECE                                                                          |              |        |                       | Root PCI Express系统错误纠正错误（SECE，System Error on Correctable Error）                                                                                                                                                                                                                                                 |
| PME SCI                                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Hot Plug                                                                      |              |        |                       | 热插拔                                                                                                                                                                                                                                                                                                                      |
| Advanced Error Reporting                                                      |              |        |                       | 高级错误报告                                                                                                                                                                                                                                                                                                                |
| PCIe Speed                                                                    |              |        |                       | PCIE设备速度（低模式省电，高模式提供高性能）                                                                                                                                                                                                                                                                                |
| Transmitter Half Swing                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Extra Bus Reserved                                                            |              |        |                       | 外置总线保留                                                                                                                                                                                                                                                                                                                |
| Reserved Memory                                                               |              |        |                       | 保留内存                                                                                                                                                                                                                                                                                                                    |
| Reserved I/O                                                                  |              |        |                       | 保留I/O                                                                                                                                                                                                                                                                                                                     |
| PCH PCIE1 LTR                                                                 |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Snoop Latency Override                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Snoop Latency Value                                                           |              |        |                       | 设置PCH PCIE的LTR Snoop时延                                                                                                                                                                                                                                                                                                 |
| Snoop Latency Multiplier                                                      |              |        |                       | 设置PCH PCIE的LTR窥探延时倍增器                                                                                                                                                                                                                                                                                             |
| Non Snoop Latency Override                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Non Snoop Latency Value                                                       |              |        |                       | 设置PCH PCIE的LTR Non Snoop时延                                                                                                                                                                                                                                                                                             |
| Non Snoop Latency Multiplier                                                  |              |        |                       | 设置PCH PCIE的LTR非Snoop延时倍数                                                                                                                                                                                                                                                                                            |
| Force LTR Override                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PCIE1 LTR Lock                                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PCIE1 CLKREQ Mapping Override                                                 |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CLKREQ Number                                                                 |              |        |                       | 要求运行的时钟                                                                                                                                                                                                                                                                                                              |
| **USB Configuration**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| SSIC Port 1/2                                                                 |              |        |                       | SSIC端口1/2                                                                                                                                                                                                                                                                                                                 |
| XHCI Disable Compliance Mode                                                  |              |        |                       | XHCI链路符合性模式                                                                                                                                                                                                                                                                                                          |
| xDCI Support                                                                  |              |        |                       | xDCI（USB OTG 装置）                                                                                                                                                                                                                                                                                                        |
| USB Port Disable Override                                                     |              |        |                       | USB接口禁用复写功能                                                                                                                                                                                                                                                                                                         |
| USB SS Physical Connector #0-13                                               |              |        |                       | USB SuperSpeed物理连接器                                                                                                                                                                                                                                                                                                    |
| USB Sensor Hub                                                                |              |        |                       | USB传感器坞                                                                                                                                                                                                                                                                                                                 |
| **SATA And RST Configuration**                                                |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| SATA Controller(s)                                                            |              |        |                       | SATA控制器的启用和禁用                                                                                                                                                                                                                                                                                                      |
| SATA Mode Selection                                                           |              |        |                       | SATA控制器模式。AHCI模式（SATA 模式）/Intel PST Premium（RAID）模式/Optane 模式/IDE模式（模仿古老的大宽硬盘并行接口）                                                                                                                                                                                                       |
| PCIe Storage Dev On Port 1/3/5/7/9/11/13/15/17/21/23                          |              |        |                       | 端口1/3/5/7/9/11/13/15/17/21/23的PCIe存储设备                                                                                                                                                                                                                                                                               |
| SATA Test Mode                                                                |              |        |                       | SATA测试模式                                                                                                                                                                                                                                                                                                                |
| RAID Device ID                                                                |              |        |                       | RAID设备ID                                                                                                                                                                                                                                                                                                                  |
| SATA Controller Speed                                                         |              |        |                       | SATA控制器速度                                                                                                                                                                                                                                                                                                              |
| **Serial ATA Port 0-7**                                                       |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Hot Plug                                                                      |              |        |                       | SATA热插拔功能                                                                                                                                                                                                                                                                                                              |
| Mechanical Presence Switch                                                    |              |        |                       | 对主板初次通电时系统中是否存在驱动器进行机械检测                                                                                                                                                                                                                                                                            |
| Spin Up Device                                                                |              |        |                       | 硬盘交错旋转加速功能                                                                                                                                                                                                                                                                                                        |
| SATA Device Type                                                              |              |        |                       | 所安装的SATA设备类型                                                                                                                                                                                                                                                                                                        |
| Topology                                                                      |              |        |                       | SATA拓撲                                                                                                                                                                                                                                                                                                                    |
| SATA Port 0 DevSlp                                                            |              |        |                       | DevSlp是一个信号，通过发送这个信号让盘进入一个非常省电的状态                                                                                                                                                                                                                                                                |
| DITO Configuration                                                            |              |        |                       | DITO配置                                                                                                                                                                                                                                                                                                                    |
| DITO Value                                                                    |              |        |                       | 设备休眠空闲超时值DITO（Device Sleep Idle Timeout），指定HBA在驱动DEVSLP信号之前应等待的时间量（大约1毫秒粒度）                                                                                                                                                                                                             |
| DM Value                                                                      |              |        |                       | DITO乘数值DM（DITO Multiplier），指定HBA应用于指定DITO值的DITO乘数，有效地将DITO的范围从1ms扩展到16368ms                                                                                                                                                                                                                    |
| **Software Feature Mask Configuration**                                       |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| HDD Unlock                                                                    |              |        | Enabled               | 激活硬盘密度解锁功能                                                                                                                                                                                                                                                                                                        |
| LED Locate                                                                    |              |        |                       | LED定位                                                                                                                                                                                                                                                                                                                     |
| Use RST Legacy OROMDM                                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| RAID0/1/5/10                                                                  |              |        |                       | RAID 0/1/5/10模式                                                                                                                                                                                                                                                                                                           |
| Intel Rapid Recovery Technology                                               |              |        |                       | 设置Intel Rapid Recovery Technology                                                                                                                                                                                                                                                                                         |
| OROM UI and BANNER                                                            |              |        |                       | 显示OROM UI                                                                                                                                                                                                                                                                                                                 |
| IRRT Only on eSATA                                                            |              |        |                       | 仅在eSATA使用英特尔快速恢复技术                                                                                                                                                                                                                                                                                             |
| Smart Response Technology                                                     |              |        |                       | 快速反应技术                                                                                                                                                                                                                                                                                                                |
| OROM UI Normal Delay                                                          |              |        |                       | OROM UI Splash画面在一般状态下的延迟时间                                                                                                                                                                                                                                                                                    |
| RST Force Form                                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| System Acceleration with Intel(R) Optane(TM) Memory                           |              |        |                       | 通过Intel Optane技术加速系统                                                                                                                                                                                                                                                                                                |
| **HD Audio Subsystem Configuration Settings**                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| HD Audio                                                                      |              |        |                       | 高清音频                                                                                                                                                                                                                                                                                                                    |
| Audio DSP                                                                     |              |        |                       | 数字信号处理器DSP（Digital Signal Processors）                                                                                                                                                                                                                                                                              |
| Audio DSP Compliance Mode                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| HDA-Link Codec Select                                                         |              |        |                       | HDA解码器选择                                                                                                                                                                                                                                                                                                               |
| iDisplay Audio Disconnect                                                     |              |        |                       | 显示器音频断开连接                                                                                                                                                                                                                                                                                                          |
| PME Enable                                                                    |              |        |                       | 电源管理事件PME（Power Management Event），将PC设置为在收到来自连接的设备或本地网络的信号后的指定时间唤醒                                                                                                                                                                                                                   |
| **HD Audio Subsystem Advanced Configuration Settings**                        |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| I/O Buffer Ownership                                                          |              |        |                       | I/O缓冲区所有权                                                                                                                                                                                                                                                                                                             |
| I2S Configuration Select                                                      |              |        |                       | I2S配置选择                                                                                                                                                                                                                                                                                                                 |
| I/O Buffer Voltage Select                                                     |              |        |                       | I/O缓冲区电压选择                                                                                                                                                                                                                                                                                                           |
| HD Audio Link Frequency                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| iDisplay Link T-Mode                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Audio DSP NHLT Endpoints Configuration**                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| DMIC                                                                          |              |        |                       | DMIC设备                                                                                                                                                                                                                                                                                                                    |
| Bluetooth                                                                     |              |        |                       | 蓝牙                                                                                                                                                                                                                                                                                                                        |
| I2S                                                                           |              |        |                       | I2S设备                                                                                                                                                                                                                                                                                                                     |
| **Audio DSP Feature Support**                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| WoV (Wake on Voice)                                                           |              |        |                       | 语音唤醒                                                                                                                                                                                                                                                                                                                    |
| DSP based Speech Pre-Processing Disabled                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Voice Activity Detection                                                      |              |        |                       | 声音行为检测                                                                                                                                                                                                                                                                                                                |
| **Audio DSP Pre/Post-Processing Module Support**                              |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| IntelSST Speech                                                               |              |        |                       | 智能声音技术SST（Smart Sound Technology）                                                                                                                                                                                                                                                                                   |
| Intel WoV                                                                     |              |        |                       | 语音唤醒WOV（Wake On Voice）                                                                                                                                                                                                                                                                                                |
| **Security Configuration**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| RTC Lock                                                                      |              |        |                       | 在RTC RAM的低/高128字节存储区中锁定38h-3Fh字节                                                                                                                                                                                                                                                                              |
| BIOS Lock                                                                     | 0x8FB        | 0x0    | Disabled              | BIOS锁                                                                                                                                                                                                                                                                                                                      |
| **SerialIo Configuration**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Touch Panel                                                                   |              |        |                       | 触控面板                                                                                                                                                                                                                                                                                                                    |
| BT/UART Mux Select                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| I2C0-I2C5 Controller                                                          |              |        |                       | I2C0-I2C5控制器                                                                                                                                                                                                                                                                                                             |
| SPI0/1 Controller                                                             |              |        |                       | SPI0/1控制器                                                                                                                                                                                                                                                                                                                |
| UART0/1/2 Controller                                                          |              |        |                       | UART0/1/2控制器                                                                                                                                                                                                                                                                                                             |
| GPIO Controller                                                               |              |        |                       | GPIO控制器                                                                                                                                                                                                                                                                                                                  |
| WITT/MITT Test Device                                                         |              |        |                       | WITT/MITT测试设备                                                                                                                                                                                                                                                                                                           |
| WITT/MITT Device selection                                                    |              |        |                       | WITT/MITT设备选择                                                                                                                                                                                                                                                                                                           |
| UART Test Device                                                              |              |        |                       | UART测试设备                                                                                                                                                                                                                                                                                                                |
| UCSI/UCMC device                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **SerialIO timing parameters**                                                |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| StandardSpeed SCL High for I2C1                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| StandardSpeed SCL Low for I2C1                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| StandardSpeed SDA Hold for I2C1                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| FastSpeed SCL High for I2C1                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| FastSpeed SCL Low for I2C1                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| FastSpeed SDA Hold for I2C1                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| FastSpeedPlus SCL High for I2C1                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| FastSpeedPlus SCL Low for I2C1                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| FastSpeedPlus SDA Hold for I2C1                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| D0->D3 idle timeout for I2C1 (screen off)                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| D0->D3 idle timeout for I2C1 (screen on)                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| D0->D3 idle timeout for SPI1 (screen off)                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| D0->D3 idle timeout for SPI1 (screen on)                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| D0->D3 idle timeout for UART1 (screen off)                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| D0->D3 idle timeout for UART1 (screen on)                                     |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| **Serial IO I2C0/1 Settings**                                                 |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Connected device                                                              |              |        |                       | 已连接的设备                                                                                                                                                                                                                                                                                                                |
| Interrupt mode                                                                |              |        |                       | 中断模式                                                                                                                                                                                                                                                                                                                    |
| Device's bus address                                                          |              |        |                       | 设备总线地址                                                                                                                                                                                                                                                                                                                |
| Device's HID address                                                          |              |        |                       | 设备HID地址                                                                                                                                                                                                                                                                                                                 |
| Device's bus speed                                                            |              |        |                       | 设备总线速度                                                                                                                                                                                                                                                                                                                |
| **Serial IO I2C2-5 Settings**                                                 |              |        |                       | I2C IO电压选择                                                                                                                                                                                                                                                                                                              |
| I2C IO Voltage Select                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| **Serial IO SPI0 Settings**                                                   |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| ChipSelect polarity                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Serial IO SPI1 Settings**                                                   |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Enable Finger Print Sensor                                                    |              |        |                       | 开启指纹传感器                                                                                                                                                                                                                                                                                                              |
| Finger Print Sensor                                                           |              |        |                       | 指纹传感器                                                                                                                                                                                                                                                                                                                  |
| Interrupt mode                                                                |              |        |                       | 中断模式                                                                                                                                                                                                                                                                                                                    |
| **Serial IO UART0 Settings**                                                  |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Bluetooth Device                                                              |              |        |                       | 蓝牙设备                                                                                                                                                                                                                                                                                                                    |
| BT Interrupt Mode                                                             |              |        |                       | BT中断模式                                                                                                                                                                                                                                                                                                                  |
| Wireless Charging Mode                                                        |              |        |                       | 无线充电模式                                                                                                                                                                                                                                                                                                                |
| Hardware Flow Control                                                         |              |        |                       | 硬件流控制                                                                                                                                                                                                                                                                                                                  |
| **Serial IO UART1/2 Settings**                                                |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Hardware Flow Control                                                         |              |        |                       | 硬件流控制                                                                                                                                                                                                                                                                                                                  |
| **GPIO Configuration**                                                        |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| GPIO IRQ Route                                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **SkyCam Configuration**                                                      |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| SkyCam CIO2 Device                                                            |              |        |                       | SkyCam CI02设备                                                                                                                                                                                                                                                                                                             |
| Control Logic 0-3                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Link0-3                                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PORT-A/B/C/D HS-RXEN/TEM-EN Override                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PORT-A/B/C/D CTLE                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PORT-A/B/C/D CTLE CAP Value                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PORT-A/B/C/D CTLE RES Value                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PORT-A/B/C/D TRIM                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PORT-A/B/C/D Data Trim Value                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| PORT-A/B/C/D Clk Trim Value                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Link0-3**                                                                   |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Sensor Model                                                                  |              |        |                       | 传感器型号                                                                                                                                                                                                                                                                                                                  |
| Lanes Clock division                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| GPIO control                                                                  |              |        |                       | GPIO控制                                                                                                                                                                                                                                                                                                                    |
| Flash Support                                                                 |              |        |                       | 闪光支持                                                                                                                                                                                                                                                                                                                    |
| Privacy LED                                                                   |              |        |                       | 隐私LED                                                                                                                                                                                                                                                                                                                     |
| Rotation                                                                      |              |        |                       | 旋转                                                                                                                                                                                                                                                                                                                        |
| PMIC Position                                                                 |              |        |                       | 电源管理IC，PMIC（Power Management IC）                                                                                                                                                                                                                                                                                     |
| Voltage Rail                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| LaneUsed                                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| MCLK                                                                          |              |        |                       | 微控制器核心MCLK（the microcontroler core）                                                                                                                                                                                                                                                                                 |
| EEPROM Type                                                                   |              |        |                       | 电子抹除式可复写唯读记忆体EEPROM/E 2 PROM（Electrically-Erasable Programmable Read-Only Memory）是可以通过电子方式多次复写的半导体储存装置，用特定电压抹除晶片上的数据，以便写入新的数据                                                                                                                                    |
| VCM Type                                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Number of I2C Components                                                      |              |        |                       | I2C组件数量                                                                                                                                                                                                                                                                                                                 |
| I2C Channel                                                                   |              |        |                       | I2C设备频道                                                                                                                                                                                                                                                                                                                 |
| **Control Logic options**                                                     |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Control Logic Type                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CRD Version                                                                   |              |        |                       | 自定义资源定义CRD（Custom Resource Definition）                                                                                                                                                                                                                                                                             |
| PMIC Flash Panel                                                              |              |        |                       | 电源管理IC，PMICs（Power management ICs）                                                                                                                                                                                                                                                                                   |
| I2C Channel                                                                   |              |        |                       | I2C设备频道                                                                                                                                                                                                                                                                                                                 |
| I2C Address                                                                   |              |        |                       | I2C设备地址                                                                                                                                                                                                                                                                                                                 |
| WLED1 Type                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| WLED1 Flash Max Current                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| WLED1 Torch Max Current                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| WLED2 Type                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| WLED2 Flash Max Current                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| WLED2 Torch Max Current                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Number of GPIO Pins                                                           |              |        |                       | GPIO Pin值                                                                                                                                                                                                                                                                                                                  |
| **GPIO 0-3**                                                                  |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Group Pad Number                                                              |              |        |                       | 平板组号                                                                                                                                                                                                                                                                                                                    |
| Group Number                                                                  |              |        |                       | 组号                                                                                                                                                                                                                                                                                                                        |
| Function                                                                      |              |        |                       | 功能                                                                                                                                                                                                                                                                                                                        |
| Active Value                                                                  |              |        |                       | 活跃值                                                                                                                                                                                                                                                                                                                      |
| Initial Value                                                                 |              |        |                       | 初始值                                                                                                                                                                                                                                                                                                                      |
| **SCS Configuration**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| eMMC 5.0 Controller                                                           |              |        |                       | eMMC5.0控制器                                                                                                                                                                                                                                                                                                               |
| eMMC 5.0 HS400 Mode                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Driver Strength                                                               |              |        |                       | 驱动强度，数值越大表示信号强度越高                                                                                                                                                                                                                                                                                          |
| SDCard 3.0 Controller                                                         |              |        |                       | SD卡3.0控制器                                                                                                                                                                                                                                                                                                               |
| SDCard Sideband Events                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **ISH Configuration**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| ISH Controller                                                                |              |        |                       | 集成传感器中枢ISH（Integrated Sensor Hub）                                                                                                                                                                                                                                                                                  |
| **TraceHub Configuration Menu**                                               |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| TraceHub Enable Mode                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| MemRegion 0-1 Buffer Size                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Pch Thermal Throttling Control**                                            |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Thermal Throttling Level                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| T0-T2 Level                                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| DMI Thermal Setting                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Thermal Sensor 0-3 Width                                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| SATA Thermal Setting                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Port 0-1**                                                                  |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| T1-T3 Multipler                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Tdispatch                                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Tinactive                                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Boot**                                                                      |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Setup Prompt Timeout                                                          |              |        | 2 Note 1              | 进入系统前BIOS等待按Delete的时间                                                                                                                                                                                                                                                                                            |
| Bootup NumLock State                                                          |              |        |                       | 开机时开启小键盘                                                                                                                                                                                                                                                                                                            |
| Quiet Boot                                                                    |              |        |                       | 快速启动                                                                                                                                                                                                                                                                                                                    |
| Fast Boot                                                                     |              |        | Disabled              | 快速启动                                                                                                                                                                                                                                                                                                                    |
| SATA Support                                                                  |              |        |                       | SATA支持                                                                                                                                                                                                                                                                                                                    |
| VGA Support                                                                   |              |        |                       | VGA支持                                                                                                                                                                                                                                                                                                                     |
| PS2 Devices Support                                                           |              |        |                       | PS2设备支持                                                                                                                                                                                                                                                                                                                 |
| NetWork Stack Driver Support                                                  |              |        |                       | 网络堆栈驱动支持                                                                                                                                                                                                                                                                                                            |
| Redirection Support                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Advanced**                                                                  |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Internal Pointing Device                                                      |              |        |                       | 设置定点设备                                                                                                                                                                                                                                                                                                                |
| Wake On Lid Open                                                              |              |        |                       | 开盖唤醒                                                                                                                                                                                                                                                                                                                    |
| Intel Virtualization Technology                                               |              |        |                       | 开放部分CPU缓存接口以提升虚拟机的IO性能（会降低CPU缓存性能）                                                                                                                                                                                                                                                                |
| Intel AES-NI                                                                  |              |        |                       | Intel进阶加密指令集AES-NI（Advanced Encryption Standard New Instructions）通过硬件加速AES的加密解密                                                                                                                                                                                                                         |
| VT-d                                                                          |              |        | Enabled               | 通过DMAR ACPI表向VMM报告I/O设备分配，为定向I/O（VT-d）启用/禁用Intel VT                                                                                                                                                                                                                                                     |
| **Graphics Configuration**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| DVMT Pre-Allocated                                                            |              |        |                       | 选择内部显卡使用的预分配图形内存大小（存在外部显卡时无效）                                                                                                                                                                                                                                                                  |
| **SATA Configuration**                                                        |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| SATA Mode Selection                                                           |              |        |                       | SATA模式选择                                                                                                                                                                                                                                                                                                                |
| **Boot**                                                                      |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Launch PXE OpROM policy                                                       |              |        |                       | 运行预启动执行环境兼容支持                                                                                                                                                                                                                                                                                                  |
| **I/O Interface Security**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Wireless Network Interface                                                    |              |        |                       | 无线网络接口                                                                                                                                                                                                                                                                                                                |
| LAN Network Interface                                                         |              |        |                       | LAN网络接口                                                                                                                                                                                                                                                                                                                 |
| SATA ODD Interface                                                            |              |        |                       | SATA ODD接口                                                                                                                                                                                                                                                                                                                |
| HD Audio Interface                                                            |              |        |                       | HD音频接口                                                                                                                                                                                                                                                                                                                  |
| **USB Interface Security**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| USB Interface                                                                 |              |        |                       | USB接口                                                                                                                                                                                                                                                                                                                     |
| External Ports                                                                |              |        | Disabled              | 系统将该接口识别为内置/外置接口                                                                                                                                                                                                                                                                                             |
| BlueTooth                                                                     |              |        |                       | 蓝牙                                                                                                                                                                                                                                                                                                                        |
| CMOS Camera                                                                   |              |        |                       | CMOS相机                                                                                                                                                                                                                                                                                                                    |
| Card Reader                                                                   |              |        |                       | 读卡器                                                                                                                                                                                                                                                                                                                      |
| Smart Card Reader                                                             |              |        |                       | 智能读卡器                                                                                                                                                                                                                                                                                                                  |
| Finger Print                                                                  |              |        |                       | 指纹记录                                                                                                                                                                                                                                                                                                                    |
| LTE                                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Secure Boot**                                                               |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Secure Boot Control                                                           |              |        | Disabled              | 安全引导功能                                                                                                                                                                                                                                                                                                                |
| **PCI Subsystem Settings**                                                    |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Above 4G Decoding                                                             |              |        |                       | 64位PCIe设备的内存映射I/O到4GB或更大的地址空间                                                                                                                                                                                                                                                                              |
| Hot-Plug Support                                                              |              |        |                       | 热插拔支持                                                                                                                                                                                                                                                                                                                  |
| **Trusted Computing**                                                         |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Security Device Support                                                       |              |        |                       | 安全设备支持                                                                                                                                                                                                                                                                                                                |
| **SMART Settings**                                                            |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| SMART Self Test                                                               |              |        |                       | SMART自检                                                                                                                                                                                                                                                                                                                   |
| **Memory Configuration**                                                      |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| MRC ULT Safe Config                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Maximum Memory Frequency                                                      |              |        |                       | 最大内存工作频率                                                                                                                                                                                                                                                                                                            |
| HOB Buffer Size                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| ECC Support                                                                   |              |        |                       | 错误校正码ECC（Error-Correcting Code）                                                                                                                                                                                                                                                                                      |
| Max TOLUD                                                                     |              |        |                       | 设定TOLUD的最大值。动态分配将依据所安装的显示控制器的最大MMIO长度来自动调整TOLUD值                                                                                                                                                                                                                                          |
| SA GV                                                                         |              |        |                       | SA速度增强速度步长                                                                                                                                                                                                                                                                                                          |
| SA GV Low Freq                                                                |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Retrain on Fast Fail                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Command Tristate                                                              |              |        |                       | 开启DRAM节电功能                                                                                                                                                                                                                                                                                                            |
| Enable RH Prevention                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Row Hammer Solution                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| RH Activation Probability                                                     |              |        |                       | 以可调的概率刷新行                                                                                                                                                                                                                                                                                                          |
| Exit On Failure (MRC)                                                         |              |        |                       | 退出时显示失败信息                                                                                                                                                                                                                                                                                                          |
| MC Lock                                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Probeless Trace                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| GDXC IOT size                                                                 |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| GDXC MOT size                                                                 |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Memory Trace                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Enable/Disable IED (Intel Enhanced Debug)                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Ch Hash Support                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Ch Hash Mask                                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Ch Hash Interleaved Bit                                                       |              |        |                       | 校验和和数据有效性/完整性检查                                                                                                                                                                                                                                                                                               |
| VC1 Read Metering                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| VC1 RdMeter Time Window                                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| VC1 RdMeter Threshold                                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Strong Weak Leaker                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Memory Scrambler                                                              |              |        |                       | 内存扰频器，用于支持内存错误纠正                                                                                                                                                                                                                                                                                            |
| Force ColdReset                                                               |              |        |                       | MRC执行期间需要冷引导时使用                                                                                                                                                                                                                                                                                                 |
| Channel A DIMM Control                                                        |              |        |                       | 启用/禁用A DIMM插槽通道                                                                                                                                                                                                                                                                                                     |
| Channel B DIMM Control                                                        |              |        |                       | 启用/禁用B DIMM插槽通道                                                                                                                                                                                                                                                                                                     |
| Force Single Rank                                                             |              |        |                       | 启用后每个内存只使用Rank0                                                                                                                                                                                                                                                                                                   |
| Memory Remap                                                                  | 0x8A7        | 0x1    | Enabled               | 支持64位系统重新指派内存地址（如果主板上安装了4GB或以上的内存，PCI内存资源将与总物理内存重叠。此功能将重叠的物理内存重新分配到高于总物理内存的位置）                                                                                                                                                                        |
| Time Measure                                                                  |              |        |                       | 时长测量                                                                                                                                                                                                                                                                                                                    |
| DLL Weak Lock Support                                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Pwr Down Idle Timer                                                           |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Mrc Fast Boot                                                                 |              |        |                       | MRC快速启动，没有内存训练，可以加快启动                                                                                                                                                                                                                                                                                     |
| Lpddr Mem WL Set                                                              |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| EV Loader                                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| EV Loader Delay                                                               |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Memory Training Algorithms**                                                |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Early Command Training                                                        |              |        |                       | 延迟并偏移DDR时钟，以使所有命令行与时钟同步                                                                                                                                                                                                                                                                                 |
| SenseAmp Offset Training                                                      |              |        |                       | 感测放大器将感测来自表示存储单元中存储的数据位（1或0）的位线的低功率信号，并将较小的电压摆幅放大到可识别的逻辑电平，以便可以由存储器外部的逻辑正确地解释数据                                                                                                                                                                |
| Early ReadMPR Timing Centering 2D                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Read MPR Training                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Receive Enable Training                                                       |              |        |                       | 调整I/O并使接收器准备好以所需速度/定时开始运行                                                                                                                                                                                                                                                                              |
| Jedec Write Leveling                                                          |              |        |                       | 向DDR3添加写平衡，以消除命令/地址/控制/时钟总线与每个DRAM数据总线之间的时滞                                                                                                                                                                                                                                                 |
| Early Write Time Centering 2D                                                 |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Early Write Drive Strength/Equalization                                       |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Early Read Time Centering 2D                                                  |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Write Timing Centering 1D                                                     |              |        |                       | 调整跨DIMM写入DRAM的延迟，测试并调整终止设置（仅影响写延迟）                                                                                                                                                                                                                                                                |
| Write Voltage Centering 1D                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Read Timing Centering 1D                                                      |              |        |                       | 内存控制器会将测试数据写入RAM，然后将其接收回去，以执行步长和延迟补偿                                                                                                                                                                                                                                                       |
| Dimm ODT Training*                                                            |              |        |                       | 片上端接ODT（On-Die Termination）减少内存总线信号中的电噪声，并消除这些电信号在高速传输线上的电信号反弹痕迹                                                                                                                                                                                                                 |
| Max RTT_WR                                                                    |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| DIMM RON Training*                                                            |              |        |                       | 此功能有助于使DIMM的I/O上的电阻趋于稳定                                                                                                                                                                                                                                                                                     |
| Write Drive Strength/Equalization 2D*                                         |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Write Slew Rate Training*                                                     |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Read ODT Training*                                                            |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Read Equalization Training*                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Read Amplifier Training*                                                      |              |        |                       | 正确恢复/均衡通过引脚接收的数据                                                                                                                                                                                                                                                                                             |
| Write Timing Centering 2D                                                     |              |        |                       | 测试在DRAM无法成功写入之前，每个方向DRAM可移动多少步（会同时测试时间和延迟）                                                                                                                                                                                                                                                |
| Read Timing Centering 2D                                                      |              |        |                       | 测试在DRAM无法成功写入之前，每个方向DRAM可移动多少步（会同时测试时间和延迟）                                                                                                                                                                                                                                                |
| Command Voltage Centering                                                     |              |        |                       | 存储器控制器的电压幅度测试                                                                                                                                                                                                                                                                                                  |
| Write Voltage Centering 2D                                                    |              |        |                       | 测试在DRAM无法成功写入之前，每个方向DRAM可移动多少步（会同时测试时间和延迟）                                                                                                                                                                                                                                                |
| Read Voltage Centering 2D                                                     |              |        |                       | 测试在DRAM无法成功写入之前，每个方向DRAM可移动多少步（会同时测试时间和延迟）                                                                                                                                                                                                                                                |
| Late Command Training                                                         |              |        |                       | 调整高层延迟并运行模式测试                                                                                                                                                                                                                                                                                                  |
| Round Trip Latency                                                            |              |        |                       | RTL参数定义在发出读取CAS命令后数据返回到存储控制器之前经过的存储控制器周期数。 RTL设置用于微调DRAM缓冲区输出延迟                                                                                                                                                                                                            |
| Turn Around Timing Training                                                   |              |        |                       | 命令/数据从内存控制器发送到DIMM，然后返回到内存控制器时的延迟                                                                                                                                                                                                                                                               |
| Rank Margin Tool                                                              |              |        |                       | 显示有关上面列出的各种设置的测试结果和边距数据                                                                                                                                                                                                                                                                              |
| Memory Test                                                                   |              |        |                       | 内存测试                                                                                                                                                                                                                                                                                                                    |
| DIMM SPD Alias Test                                                           |              |        |                       | 对正确SPD数据的逻辑检查                                                                                                                                                                                                                                                                                                     |
| Receive Enable Centering 1D                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Retrain Margin Check                                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Write Drive Strength Up/Dn independently                                      |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CMD Slew Rate Training                                                        |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CMD Drive Strength / Tx Equalization                                          |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| CMD Normalization                                                             |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| **Memory Thermal Configuration**                                              |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Memory Thermal Management                                                     |              |        | Enabled               | 内存热管理开关                                                                                                                                                                                                                                                                                                              |
| PECI Injected Temperature                                                     |              |        |                       | 通过PECI注入处理器的内存温度                                                                                                                                                                                                                                                                                                |
| EXTTS# via TS-on-Board                                                        |              |        |                       | 将板载TS路由注入PCH上的EXTTS#引脚                                                                                                                                                                                                                                                                                           |
| EXTTS# via TS-on-DIMM                                                         |              |        |                       | 插入到PCH引脚上的TS-on-DIMM路由                                                                                                                                                                                                                                                                                             |
| Virtual Temperature Sensor (VTS)                                              |              |        |                       | 虚拟温度传感器                                                                                                                                                                                                                                                                                                              |
| **Memory Power and Thermal Throttling**                                       |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| DDR PowerDown and idle counter                                                |              |        |                       | 控制DDR掉电模式和空闲计数器的方式。PCODE为硬件算法控制，BIOS为BIOS控制                                                                                                                                                                                                                                                      |
| REFRESH_2X_MODE                                                               |              |        |                       | 启用REFRESH 2X模式，当温度为WARM或HOT时，通过提高DRAM刷新率以维持可接受的总体错误率。0表示禁用，1表示WARM或HOT时启用，2表示HOT时启用                                                                                                                                                                                        |
| LPDDR Thermal Sensor                                                          |              |        |                       | 启用后MC使用MR4读取LPDDR热传感器                                                                                                                                                                                                                                                                                            |
| SelfRefresh Enable                                                            |              |        |                       | 允许用户在处理器不可用以刷新DRAM阵列时将数据保存在DRAM中                                                                                                                                                                                                                                                                    |
| SelfRefresh IdleTimer                                                         |              |        |                       | 可用范围64K-1到512                                                                                                                                                                                                                                                                                                          |
| Throttler CKE Min Defeature                                                   |              |        |                       | /                                                                                                                                                                                                                                                                                                                           |
| Throttler CKE Min Timer                                                       |              |        |                       | CKE计时器最小值。可用范围[255;0]                                                                                                                                                                                                                                                                                            |
| **Dram Power Meter**                                                          |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Use user provided power weights, scale factor, and channel power floor values |              |        |                       | 是否使用用户提供的功率权重/比例因子/基于DIMM的通道电源下限值，禁用则由BIOS设置功率权重，由系统设置比例因子/基于DIMM的通道电源下限值                                                                                                                                                                                         |
| Energy Scale Factor                                                           |              |        |                       | 能量比例因子，范围[7;0]=[7.3pJ;931.3pJ]                                                                                                                                                                                                                                                                                     |
| Idle Energy Ch0/1Dimm0/1                                                      |              |        |                       | 当cke开启且DIMM处于空闲状态时，DIMM消耗一个时钟周期的空闲能量。范围[63;0]                                                                                                                                                                                                                                                   |
| PowerDown Energy Ch0/1Dimm0/1                                                 |              |        |                       | 当cke关闭且DIMM处于关闭状态且空闲时，DIMM在一个时钟周期内消耗的能量。范围[63;0]                                                                                                                                                                                                                                             |
| Activate Energy Ch0/1Dimm0/1                                                  |              |        |                       | 激活能量贡献。范围[255;0]                                                                                                                                                                                                                                                                                                   |
| Read Energy Ch0/1Dimm0/1                                                      |              |        |                       | 读能源贡献。范围[255;0]                                                                                                                                                                                                                                                                                                     |
| Write Energy Ch0/1Dimm0/1                                                     |              |        |                       | 写能源贡献。范围[255;0]                                                                                                                                                                                                                                                                                                     |
| **Memory Thermal Reporting**                                                  |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Lock Thermal Management Registers                                             |              |        | Disabled              | 锁定热管理寄存器，开启时几个与DDR电源/热量管理相关的PCU寄存器将无法写入                                                                                                                                                                                                                                                     |
| Extern Therm Status                                                           |              |        |                       | pcode使用/忽略外部热状态                                                                                                                                                                                                                                                                                                    |
| Closed Loop Therm Manage                                                      |              |        |                       | 使用CTLM pcode算法计算内存热状态（CLTM优先于OLTM）                                                                                                                                                                                                                                                                          |
| Open Loop Therm Manage                                                        |              |        |                       | 使用OLTM pcode算法计算内存热状态（CLTM优先于OLTM）                                                                                                                                                                                                                                                                          |
| Warm Threshold Ch0 Dimm0                                                      |              |        |                       | 范围[255;0]                                                                                                                                                                                                                                                                                                                 |
| Warm Threshold Ch0 Dimm1                                                      |              |        |                       | 范围[255;0]                                                                                                                                                                                                                                                                                                                 |
| Warm Threshold Ch1 Dimm0                                                      |              |        |                       | 范围[255;0]                                                                                                                                                                                                                                                                                                                 |
| Warm Threshold Ch1 Dimm1                                                      |              |        |                       | 范围[255;0]                                                                                                                                                                                                                                                                                                                 |
| Hot Threshold Ch0 Dimm0                                                       |              |        |                       | 范围[255;0]                                                                                                                                                                                                                   |
| Hot Threshold Ch0 Dimm1                                                       |              |        |                       | 范围[255;0]                                                                                                                                                                                                                   |
| Hot Threshold Ch1 Dimm0                                                       |              |        |                       | 范围[255;0]                                                                                                                                                                                                                |
| Hot Threshold Ch1 Dimm1                                                       |              |        |                       | 范围[255;0]                                                                                                                                                                                                                |
| Warm Budget Ch0 Dimm0                                                         |              |        |                       | 范围[255;0]                                                                                                                                                                                                                 |
| Warm Budget Ch0 Dimm1                                                         |              |        |                       | 范围[255;0]                                                                                                                                                                                                                    |
| Hot Budget Ch0 Dimm0                                                          |              |        |                       | 范围[255;0]                                                                                                                                                                                                                     |
| Hot Budget Ch0 Dimm1                                                          |              |        |                       | 范围[255;0]                                                                                                                                    |
| Warm Budget Ch1 Dimm0                                                         |              |        |                       | 范围[255;0]                                                                                                                                     |
| Warm Budget Ch1 Dimm1                                                         |              |        |                       | 范围[255;0]                                                                                                                                                                                                                       |
| Hot Budget Ch1 Dimm0                                                          |              |        |                       | 范围[255;0]                                                                                                                                                                                                                        |
| Hot Budget Ch1 Dimm1                                                          |              |        |                       | 范围[255;0]                                                                                                                                                                                                                  |
| **Memory RAPL**                                                               |              |        |                       |                                                                                                                                                                                                                                                                                                                             |
| Rapl Power Floor Ch0                                                          |              |        |                       | 功率预算，范围[255;0]，其中0=5.3W                                                                                                                                                                                                                                                                                           |
| Rapl Power Floor Ch1                                                          |              |        |                       | 功率预算，范围[255;0]，其中0=5.3W                                                                                                                                                                                                                                                                                           |
| RAPL PL Lock                                                                  |              |        |                       | 锁定RAPL限制寄存器                                                                                                                                                                                                                                                                                                          |
| RAPL PL 1 enable                                                              |              |        |                       | RAPL 1功率限制                                                                                                                                                                                                                                                                                                              |
| RAPL PL 1 Power                                                               |              |        |                       | RAPL 1的DDR域功率限制，范围[0,2047]，即[0,2047.875]W                                                                                                                                                                                                                                                                        |
| RAPL PL 1 WindowX                                                             |              |        |                       | RAPL 1的DDR功率限制时间长度的X值，实际时间长度计算公式为`(1/1024)*(1+(x/4))*(2^y)`，单位为秒                                                                                                                                                                                                                                |
| RAPL PL 1 WindowY                                                             |              |        |                       | RAPL 1的DDR功率限制时间长度的Y值，实际时间长度计算公式为`(1/1024)*(1+(x/4))*(2^y)`，单位为秒                                                                                                                                                                                                                                |
| RAPL PL 2 enable                                                              |              |        |                       | RAPL 2功率限制                                                                                                                                                                                                                                                                                                              |
| RAPL PL 2 Power                                                               |              |        |                       | RAPL 2的DDR域功率限制，范围[0,2047]，即[0,2047.875]W                                                                                                                                                                                                                                                                        |
| RAPL PL 2 WindowX                                                             |              |        |                       | RAPL 2的DDR功率限制时间长度的Y值，实际时间长度计算公式为`(1/1024)*(1+(x/4))*(2^y)`，单位为秒                                                                                                                                                                                                                                |
| RAPL PL 2 WindowY                                                             |              |        |                       | RAPL 2的DDR功率限制时间长度的Y值，实际时间长度计算公式为`(1/1024)*(1+(x/4))*(2^y)`，单位为秒                                                                                                                                                                                                                                |
| CPU Fan Speed                                                                 | 0x2C4        | 0x64   | Maximum               | 风扇转速                                                                                                                                                                                                                                                                                                                    |
| FAN1 Device                                                                   | 0x2EA        | 0x1    | Enabled               | FAN1设备                                                                                                                                                                                                                                                                                                                    |
| FAN2 Device                                                                   | 0x2EB        | 0x1    | Enabled               | FAN2设备                                                                                                                                                                                                                                                                                                                    |
| Generic Device 4                                                              | 0x371        | 0x1    | Enabled               | 开启第四个核心                                                                                                                                                                                                                                                                                                              |

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

## Is it possible to unlock a clock-locked CPU?

```
https://www.quora.com/Is-it-possible-to-unlock-a-clock-locked-CPU
```

## Kontron KTQM87-mITX User Manual

```
https://www.manualsdir.com/manuals/637903/kontron-ktqm87-mitx-ktqm87.html
```

## Kontron KTHM76-mITX User Manual

```
https://www.manualsdir.com/manuals/637900/kontron-kthm76-mitx-kthm76-ktqm77-mitx-ktqm77.html
```

## RUNNING AVERAGE POWER LIMIT – RAPL

```
https://01.org/blogs/2014/running-average-power-limit-%E2%80%93-rapl
```

## How To Software OverClock Your CPU With ClockGen Fast And Easy Way to OC

```
https://www.youtube.com/watch?v=7yXW4pqLE70
```

## 英特尔® 至尊调试实用程序 (XTU)-英特尔®️ 官网

```
https://www.intel.cn/content/www/cn/zh/gaming/resources/overclocking-xtu-guide.html
```

## 如何超频 RAM -英特尔®️ 官网

```
https://www.intel.cn/content/www/cn/zh/gaming/resources/overclock-ram.html
```

## 如何超频您未锁频的英特尔® 酷睿™ 处理器-英特尔®️ 官网

```
https://www.intel.cn/content/www/cn/zh/gaming/resources/how-to-overclock.html
```

## 如何通过 BIOS 超频 CPU - 英特尔® 官网

```
https://www.intel.cn/content/www/cn/zh/gaming/resources/bios-overclocking.html
```

## roncapat/W230SD-Unlocked-AMI-BIOS

```
https://github.com/roncapat/W230SD-Unlocked-AMI-BIOS
```

## Fujitsu-Platform-Settings-V1.0.2-CFL-RevB.2019-11-01.xml

```
http://www.spec.org/cpu2017/flags/Fujitsu-Platform-Settings-V1.0.2-CFL-RevB.2019-11-01.xml
```

## Achieving Persistent DRAM on PowerQUICC III and QorIQ Processors

```
https://www.nxp.com/docs/en/application-note/AN4531.pdf
```

## The ThrottleStop Guide

```
http://forum.notebookreview.com/threads/the-throttlestop-guide.531329/page-72
```

## Intelligent Platform & Services Business Unit Embedded Computing (3.5” CPU Board) EBC 357 User Manual

```
http://files.nexcom.com/Driver/EBC357/User_Manual_EBC357_191031.pdf
```

## DK37 User manual

```
http://www.giadatech.com/upload/20200715/f403582d5ef80bd156bdbcf75b867b03.pdf
```

## IoT Automation Solutions Business Group Embedded Computing (3.5” CPU Board) EBC 357X User Manual

```
https://www.spectra.de/files/produkte/KA007500/web/Manual-EBC357X-170308.pdf
```

## Kontron mITX-SKL-S-C236 User Manual

```
https://www.manualslib.com/manual/1323297/Kontron-Mitx-Skl-S-C236.html#page=44-manual
```

## ASUS P10S WS User Guide

```
https://manualsbrain.com/en/manuals/1556610/
```

## Configuring the Z87 FTW BIOS

```
https://www.evga.com/support/manuals/files/BIOS/141-HW-E877_BIOS_Guide.pdf
```

## Kontron KTHM76-mITX User Manual

```
https://www.manualsdir.com/manuals/637900/kontron-kthm76-mitx-kthm76-ktqm77-mitx-ktqm77.html
```

## 英特尔® Ready Mode 技术-英特尔® 官网

```
https://www.intel.cn/content/www/cn/zh/architecture-and-technology/intel-ready-mode-technology.html
```

## ASUS P10S-M WS 用户指南

```
https://manualsbrain.com/zh/manuals/1556486/
```

## 超能课堂(210) 笔记本中常常听说的PL1、PL2到底如何影响CPU性能

```
https://www.bilibili.com/read/cv4099352/
```

## 8750H no more limits/throttle

```
https://www.techpowerup.com/forums/threads/8750h-no-more-limits-throttle.278278/
```

## R161-R12 High Efficiency Liquid Cooling System User Manual

```
https://download.gigabyte.com/FileList/Manual/server_manual_r161-r12_e_10.pdf
```

## Supermicro C9Z490-PG User Manual

```
https://www.manualslib.com/manual/1920265/Supermicro-C9z490-Pg.html
```

## TQMx80UC User's Manual

```
https://www.tq-group.com/filedownloads/files/products/embedded/manuals/x86/embedded-modul/COM-Express-Compact/TQMx80UC/TQMx80UC.UM.0102.pdf
```

## 【经验之谈】当PL1成为“摆设”——“黑匣子”DPTF

```
https://zhuanlan.zhihu.com/p/62254032
```

## Quick CPU - Advanced CPU settings

```
https://coderbag.com/product/quickcpu/features/advanced-cpu-settings
```

## BIOS User Guide H410MH / H410MHP

```
https://www.biostar-europe.com/upload/Manual/IH41A-MHS%20&%20IH41B-MHS_BIOS_200429.pdf
```

## higher trefi vs lower CAS latency which would you choose?

```
https://www.reddit.com/r/overclocking/comments/cdib4y/higher_trefi_vs_lower_cas_latency_which_would_you/
```

## ECS H110I-C4P User Manual Page

```
https://mans.io/files/viewer/539002/58
```

## Platform Hierarchy

```
https://ebrary.net/24759/computer_science/platform_hierarchy
```

## Supermicro C7Z270-CG User Manual

```
https://www.manualslib.com/manual/1320957/Supermicro-C7z270-Cg.html
```

## Supero C7Z270-CG-L User Manual

```
https://www.manualslib.com/manual/1263032/Supero-C7z270-Cg-L.html
```

## Karbon 700 BIOS Manual

```
https://static.onlogic.com/resources/manuals/K700-SE-BIOS-Manual-V2.0.pdf
```

## X11SCA/X11SCA-W/X11SCA-F USER MANUAL

```
https://cdn.competec.ch/documents2/6/8/2/51826286/51826286.pdf
```

## MIO-5373 User Manual

```
https://www.mouser.com/catalog/additional/Advantech_MIO_5373_User_Manual_Ed1_FINAL.pdf
```

## Intel® 7th Gen. Core™ U-series ® SoC User Manual

```
https://www.rjconnect.co.za/images/PDF/RJ_QPI_Series_User_Manual_V12.pdf
```

## 用户手册 Lenovo - ThinkPad W530, ThinkPad T530, ThinkPad T530i

```
http://www.pdfshouce.com/manuals/15037/lenovo-thinkpad-w530-thinkpad-t530-thinkpad-t530i.html
```

## IGN400 BIOS Manual

```
http://static.onlogic.com/resources/manuals/IGN400-BIOS-Manual-V1.pdf
```

## Karbon 300 BIOS Manual

```
https://static.onlogic.com/resources/manuals/Karbon-300-BIOS-Manual-OnLogic.pdf
```

## Avalue EPI-QM67 User Manual

```
https://www.manualsdir.com/manuals/734186/avalue-epi-qm67.html
```

## Ampro COM 830 Reference Manual

```
https://www.manualslib.com/manual/447916/Ampro-Com-830.html
```

## SBC35-427 Supplemental BIOS Manual

```
https://resources.winsystems.com/product-manuals/SBC35-427-BIOS-v1.0.pdf
```

## 内存时序是什么？对性能影响有多大？终于懂了

```
https://www.sohu.com/a/406711779_100082659
```

## NP3020M4-BIOS设置

```
http://www.4008600011.com/archives/8236
```

## ASUS Z10PE-D16 WS USER GUIDE

```
https://manualsbrain.com/en/manuals/1562289/
```

## EMBC-1000 USER Manual

```
https://v1.cecdn.yun300.cn/100001_2004035551/EMBC-1000UserManual.pdf
```

## C7H270-CG-ML USER’S MANUAL

```
https://www.supermicro.org.cn/wdl/ISO_Extracted/CDR-C7_V1.31_for_Intel_C7_platform/MANUALS/C7H270-CG-ML.pdf
```

## IVH-9000 USER Manual

```
http://www.vecow.com/dispUploadBox/PJ-VECOW/Files/2379.pdf
```

## Blackbird (VL-EPU-4562) BIOS Reference Manual

```
https://www.versalogic.com/wp-content/themes/vsl-new/assets/pdf/manuals/MEPU_4462_4562_BRM.pdf
```

## ECM-SKLH User’s Manual

```
http://www.bcmcom.com/admin/Manual/ECM-SKLH_Manual_E2047392801R.pdf
```

## MINI-ITX 带 VGA/LVDS/HDMI/2LAN/6COM

```
http://www.evoc.cn/uploadfiles/2018/06/EC7-1820-%E4%B8%AD%E8%8B%B1%E6%96%87%E8%AF%B4%E6%98%8E%E4%B9%A6.pdf
```

## C7Z170-OCE USER'S MANUAL

```
https://www.manualslib.com/manual/1257502/Supermicro-C7z170-Oce.html
```

## ROG MAXIMUS XI HERO (WI-FI) BIOS Manual

```
https://images-eu.ssl-images-amazon.com/images/I/C1spIFYcHzS.pdf
```

## 如何理解Power rails, voltage rails?

```
https://www.eda365.com/thread-166559-1-1.html
```

## PCIe SSD支持的ASPM是什么?

```
http://www.ssdfans.com/?p=4022
```

## NVME中的OBFF是什么东西？

```
https://www.zhihu.com/question/52959132
```

## IK70 Motherboard User Manual

```
https://dc.winmate.com.tw/_downloadCenter/2017/Lcd/9171111I103R_IK70_SBC_User_Manual_(Kaby_Lake)_V1.3.pdf
```

## РУКОВОДСТВО ПОЛЬЗОВАТЕЛЯ ДЛЯ ASUS P10S-M WS

```
https://manualsbrain.com/ru/manuals/1556486/
```

## 服务手册 Dell - XPS One 2710

```
http://www.pdfshouce.com/manuals/14522/dell-xps-one-2710.html
```

## American Megatrends MI980 User Manual

```
https://www.manualslib.com/manual/672846/American-Megatrends-Mi980.html
```

## NC7L Series User’s Manual

```
http://www.jetwayipc.com/Manual/G03-NC7L-F.pdf
```

## TS300-E9-PS4服务器用户手册

```
https://dlsvr04.asus.com/pub/ASUS/server/TS300-E9-PS4/Manual/C11404_TS300-E9-PS4_UM_WEB.pdf
```

## Port 8xh Decode - 004 - ID:631119 | Intel® 500 Series Chipset Family On-Package Platform Controller Hub Datasheet Volume 1

```
https://edc.intel.com/content/www/tw/zh/design/ipla/software-development-platforms/client/platforms/tiger-lake-mobile-y/intel-500-series-chipset-family-on-package-platform-controller-hub-datasheet-v/004/port-8xh-decode/
```

## C785S_C582S_C381S-IM-AA_UM_WEB

```
https://dlsvr04.asus.com.cn/pub/ASUS/mb/Embedded_IPC/C582S-IM-AA/E15424_C785S_C582S_C381S-IM-AA_UM_WEB.pdf
```

## On my X58 when enabling AHCI in BIOS I get the options "Mechanical presence switch" and "Cold Presence switch" what are these options?

```
https://www.evga.com/support/faq/FAQdetails.aspx?faqid=58653
```

## AHCI 1.3.1 Device Sleep Technical Proposal

```
https://www.intel.com/content/dam/www/public/us/en/documents/technical-specifications/serial-ata-ahci-tech-proposal-rev1_3_1.pdf
```

## 各種虛擬化技術支援AES-NI 狀況

```
https://lab.howie.tw/2014/01/intel-aes-ni-support-on-each-hypervisor-platform.html
```

## Rowhammer mitigation: RH activation probability

```
https://coreboot.coreboot.narkive.com/HCYWSk2m/rowhammer-mitigation-rh-activation-probability
```

## Chipset 菜单

```
https://product-help.schneider-electric.com/Machine%20Expert/V1.1/zh/RackiPC/RackiPC/iPC_-_Configuration_of_the_BIOS/iPC_-_Configuration_of_the_BIOS-5.htm
```

## H3C UniServer T1100 G3服务器Greenlow平台BIOS用户指南-5W102

```
http://www.h3c.com/cn/d_201801/1055806_30005_0.htm
```

## Supero C7Z270-PG User Manual

```
https://www.manualslib.com/manual/1261568/Supero-C7z270-Pg.html
```

## 18--A55M-S41 BIOS设置讲解 <2013-4-21>

```
https://forum-sc.msi.com/index.php?topic=81.0
```

## Lenovo ThinkSystem Documentation

```
https://thinksystem.lenovofiles.com/help/index.jsp?topic=%2Fuefi_amd_1p_rome%2Ftrusted_computing.html
```

## mITX-SKL-S-C236 USER GUIDE

```
https://webcache.googleusercontent.com/search?q=cache:9UiwU_ONcWgJ:https://www.kontron.com/downloads/manuals/m/mitx-skl-s_c236_user-guide_rev1.6_2020-11-04.pdf%3Fproduct%3D145236+&cd=2&hl=zh-CN&ct=clnk&gl=hk
```

## AMD Ryzen Master超频工具食用建议

```
https://zhuanlan.zhihu.com/p/31246328
```

## 你不一定都知道，微星AfterBurner软件全接触

```
https://www.expreview.com/17454-all.html
```

## EVGA PrecisionX 16 的正确使用方法

```
https://www.chiphell.com/thread-1270827-1-1.html
```

## AMD锐龙内存超频计算器使用教程——DRAM Calculator for Ryzen及台风下载

```
https://www.troyqi.com/archives/836/amd%E9%94%90%E9%BE%99%E5%86%85%E5%AD%98%E8%B6%85%E9%A2%91%E8%AE%A1%E7%AE%97%E5%99%A8%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B-dram-calculator-for-ryzen%E4%B8%8B%E8%BD%BD/
```

## CoffeeLake Intel(R) Firmware Support Package (FSP) Integration Guide: FSP_M_CONFIG Struct Reference

```
https://documentation.help/CoffeeLake-Intel-Firmware/struct_f_s_p___m___c_o_n_f_i_g.html
```
