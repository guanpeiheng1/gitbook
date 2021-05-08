---
title: Google正确食用指南
categories: World
abbrlink: How-To-Access-Google
date: 2019-06-02 17:01:00
tags:
---

![](https://i.loli.net/2019/06/03/5cf4e62026ce662099.jpg)

Google是一个强大的搜索引擎，本文主要介绍访问Google的方法。

<!-- more -->

# 资料

```
https://guide.v2fly.org/
https://www.v2fly.org/
https://docs.cfw.lbyczf.com/
```

# 相关知识

## 原生/广播IP

原生IP指由当地ISP运营商提供的本地IP，广播IP是IP分配机构指派在某个地区使用的IP。可通过以下网站查看IP地址的归属地，归属地与服务器所在地一致的为原生IP。

```
https://bgp.he.net/
```

## DNS

DNS服务器可分为DNS over HTTPS（DoH）和DNS over TLS（DoT）。

DoH为以https://开头的DNS服务器，拥有更好的伪装性，且几乎不可能被运营商或网络管理封锁，但查询效率和安全性可能略低。

DoT为以tls://开头的DNS服务器，拥有更高的安全性和查询效率，但端口有可能被管制或封锁。

## 运行模式

Clash/Surge可开启接管互联网功能，管理连接到同一局域网中的设备，该模式被称为网关模式。

通过软路由安装翻墙插件的方式，可使部分无法安装翻墙工具的设备实现翻墙，该代理方法被称为透明代理。

| 类型     | 说明                                         |
| -------- | -------------------------------------------- |
| 全局直连 | 所有的流量会被直接转发，不会经过任何处理     |
| 自动分流 | 通过设定的规则动态控制相关的请求后续如何处理 |
| 全局代理 | 所有的流量都会经过代理转发                   |

## 访问方法

访问网络通过`数据包的传递`完成。网站实际上是一个服务器，用户向服务器发送数据包，服务器返回相应的数据包，就完成了访问网站的流程。

每台服务器都有一个`IP地址`，比如百度的IP地址是14.215.177.39，Google的IP地址经常变动。为了便于记忆，用域名把每个IP地址一一对应，这种对应则需要`DNS`。向DNS提供域名，DNS返回正确的IP地址，即可访问到正确的网站。高墙则建在用户和网站之间。可通过以下方式完成与外网的连接。

### SSH隧道

SSH原本用于UNIX类系统的远程登录和管理。由于SSH可以通过客户端软件，在本地做一个SOCKS代理进行加密转发，因此也被用于网络代理。配合浏览器插件和自动列表，可实现对不同网址有选择性地代理。

有一台支持SSH的墙外服务器后，在客户端执行如下命令即可打通隧道。

```
ssh -D 7001 username@remote-host
```

上述命令中-D表示动态绑定，7001表示本地SOCKS代理的侦听端口，username@remote-host为登录远程服务器的用户名和主机。

### VPN

VPN通过建立虚拟网卡的方式接管流量，无限接近最底层。

现VPN基本无法用于翻墙。比较常见的VPN隧道协议有PPTP VPN、L2TP VPN、OpenVPN、SSH代理等。

#### PPTP VPN

PPTP协议是点对点隧道协议，其将TCP控制包与数据包分开，适合在没有墙限制的网络中使用。

#### L2TP VPN

L2TP能以隧道方式使PPP包通过各种网络协议，但没有任何加密措施，更多是和IPSec协议结合使用提供隧道验证。支持PPTP的设备基本都支持L2TP，需要选择L2TP/IPSec PSK方式且设置预共享密钥PSK

L2TP使用UDP协议，一般可穿透防火墙，适合在有防火墙限制的局域网用户，如公司、网吧、学校场合等使用。PPTP和L2TP二个连接类型在性能上差别不大。

#### OpenVPN

OpenVPN是一个基于SSL加密的纯应用层VPN协议，是SSL VPN的一种，支持UDP与TCP两种方式。通常UDP的效率会比较高，速度也相对较快。其运行在纯应用层，避免了PPTP和L2TP在某些NAT设备后面不被支持的情况，并且可以绕过一些网络的封锁。基本上能上网的地方就能用OpenVPN。

OpenVPN客户端软件可以很方便地配合路由表，实现不同线路（如国内和国外）的路由选择，实现一部分IP走VPN，另一部分IP走原网络。

OpenVPN目前已被墙用流量特征识别等技术手段侦测并受到严重封杀。

#### 协议比较

```
// 易用性
PPTP>L2TP>OpenVPN

// 速度
PPTP>OpenVPN UDP>L2TP>OpenVPN TCP

// 安全性
OpenVPN>L2TP>PPTP

// 稳定性
OpenVPN>L2TP>PPTP

// 网络适用性
OpenVPN>PPTP>L2TP
```

### 代理

#### 正向代理

正向代理一般都是C/S架构的，它通过客户端，将你要访问某站的请求打包，然后发给代理服务器，而代理服务器负责将你的请求发送给目标站。类型分为HTTP代理、FTP代理、SSL代理、Socks代理等。正向代理又会根据高度匿名与否，分为高度匿名代理、普通匿名代理、透明代理等。

##### Socks代理

Socks代理通过TCP连接把目标主机和客户端连接在一起，并转发所有的流量，而不关心协议。该代理能在Session层之上的任何端口或协议下运行。Socks4只支持TCP连接，而Socks5在其基础上增加了安全认证以及对UDP协议的支持，即Socks5支持密码认证以及转发UDP流量。

Socks代理在任何情况下都不会中断server与client之间的数据，当墙不允许访问外网时，可以通过Socks代理来上网。

Socks协议非明文，但在Socks代理服务器上可以还原出TCP和UDP的原始流量。

大多数的浏览器都支持Socks代理。浏览器上网的时候需要与目标主机建立TCP连接，这个时候浏览器就会告诉Socks代理，它想与目标主机进行通讯，然后Socks代理就会转发浏览器的数据，并向目标主机发出请求，再把返回的数据转发回来。

##### HTTP代理

HTTP代理跟Socks的原理类似，用处也基本相同，都是让处于防火墙下的主机与外界建立连接。

与Socks代理不同的是，HTTP代理可以中断连接（即在中间截断数据流），因为HTTP代理是以HTTP请求为基础的，而这些请求大多以明文形式存在，所以HTTP代理可以在客户端和下游服务器中间窃听，修改数据。且HTTP不支持转发UDP。

由于HTTP代理只能处理HTTP请求，所以HTTP代理服务器就可以根据你提交的数据来把资源缓存下来，提升访问的速度。很多ISP采用HTTP代理，因为用户都需要访问80端口，而80端口可由ISP进行控制。

HTTP协议属于应用层，而SOCKS协议属于传输层。传输层在网络层之下，这使得Socks代理的作用更为广泛。

#### 反向代理

假设大型网站服务器有一台，而后端负责计算的服务器有N台。真正的服务器在这N台服务器，它们通过某些机制同步内容。前端的服务器只负责收发数据，在前端接收到请求之后，就会根据后台某服务器的空闲资源完成请求的反向代理，从而达到了负载均衡的目的。

#### 透明代理

此处的透明并不是正向代理的分支，而是路由器上的各种代理，这种代理相对电脑来说是透明的。在路由器上使用透明代理，一般是使用Socks5代理或VPN。

### 专门协议

包括V2Ray、shadowsocks（SS）、shadowsocksR（SSR）、wireguard、brook、outline等，这些协议隐蔽性相对较高，加密方式相对完善。现SSR遭封禁严重，V2Ray和SS是主流协议。

这些协议都是通过连接到一台墙未封禁的国外服务器，让该服务器访问被墙封锁的网站后返回相应数据，从而达到翻墙的目的，向国外服务器传输数据的过程是加密的、不具有明显特征的。这些服务器一般称为VPS，VPS（Virtual Private Server，虚拟专用服务器）可以理解为安装到电脑上的虚拟机，这些虚拟机相互独立。

#### Shadowsocks

Shadowsocks有服务端和客户端。客户端监听1080端口，并将数据转发到Socks服务器上。这种转发在会话层，是加密的，所以墙过滤的难度很大。服务端收到数据后将数据发送到目标请求，收到反馈后再传给本地客户端的1080端口，完成TCP连接。

Shadowsocks(R)的历史可查看以下链接。

```
https://github.com/KeiKinn/ShadowsocksBio
```

##### obfs混淆

obfs混淆最大的作用是对Shadowsocks流量进行伪装。添加obfs http模式之后，通过运营商的流量会被识别为设定好的网址的流量。

tls模式安全性高于http模式。

#### 自建VPS

自己购买VPS并搭建协议。

#### 机场

`机场`指提供节点服务的供应商。`节点`可以理解为服务器，机场会通过`Subscribe`提供等级不同的节点，形式是一个`URL`。

可通过以下链接查看各机场测速结果。

```
https://www.duyaoss.com/
```

## 线路

### CN2线路

CN2即CNCN，相比普通的163骨干网更好。注意区分单向CN2和双向CN2。

### BGP线路

BGP即边界网关协议，可识别运营商并自动选择最优线路。

### PCCW线路

香港线路。

### IPLC/IEPL专线

直接用网线连接中国大陆与其它地区，无需过墙，如深港IPLC专线。

## 安全须知

不要使用360系和百度系的产品，其会自动上报IP地址，很容易使所用服务器遭到封禁。不要用国内浏览器，推荐用`Google Chrome`或`Microsoft Edge`。浏览器可打开`不跟踪`请求，且可将系统语言改为服务器所在地语言会进一步提升安全性。同时尽量减少让国内网站走代理的情况出现，会增加被墙几率。

浏览网页时可通过以下插件，强制使用HTTPS。

```
https://www.eff.org/https-everywhere
```

## 墙封锁技术

### 网站封锁列表

```
https://github.com/gfwlist/gfwlist
```

### 发帖与回溯流程

|                情况               |                      浏览流程                      |                                                回溯流程                                                | 暴露概率 |
|-----------------------------------|----------------------------------------------------|--------------------------------------------------------------------------------------------------------|----------|
| 不用代理                          | 发帖人-ISP-服务器托管商-服务器                     | 网监-服务器IP-发帖人IP-发帖人ISP-档案                                                                  | 100%     |
| 一层私有VPN                       | 发帖人-ISP-VPN-服务器托管商-服务器                 | 网监-服务器IP-VPN的IP-路由日志-链接者IP-链接者ISP-办网档案                                             | 100%     |
| 一层共用VPN                       | 发帖人-ISP-VPN-服务器托管商-服务器                 | 网监-服务器IP-VPN的IP-入侵VPN服务器-日志-链接者IP-链接者ISP-办网档案                                   | 80%      |
| 两层私有VPN                       | 发帖人-ISP-VPN1-VPN2-服务器托管商-服务器           | 网监-服务器IP-VPN2的IP-入侵VPN2服务器-日志查VPN1-路由日志-链接者IP-链接者ISP-办网档案                  | 40%      |
| 两层共用VPN                       | 发帖人-ISP-VPN1-VPN2-服务器托管商-服务器           | 网监-服务器IP-VPN2的IP-入侵VPN2服务器-日志查VPN1-日志-链接者IP-链接者ISP-办网档案                      | 25%      |
| N层私有/公用VPN，发帖人有QQ       | 发帖人-ISP-VPN1-VPN2-VPN3-VPN*n服务器托管商-服务器 | 网监-服务器IP-访问网站-发帖人常用用户名-谷歌（百度）一下-查到QQ-找腾讯查近期登录IP                     | 100%     |
| N层私有/公用VPN，发帖人无QQ       | 发帖人-ISP-VPN1-VPN2-VPN3-VPN*n服务器托管商-服务器 | 网监-服务器IP-访问网站-发帖人常用用户名-谷歌（百度）一下-查到其他论坛注册的ID-找管理员查注册（登录）IP | 100%     |
| N层私有/公用VPN，发帖人无其它信息 | 发帖人-ISP-VPN1-VPN2-VPN3-VPN*n服务器托管商-服务器 | 网监-服务器IP-访问网站-无常用用户名-根据发帖内容（如北京市海淀区XX小区）-监控整个小区宽带-分析-找到IP  | 80%      |

### IP封锁

#### 原理

墙通过将IP拉入黑名单以完成封锁。Goagent、XX-NET、Hosts方式通过IP访问谷歌或者其他被屏蔽的网站。现在大量谷歌IP被拉黑，而这些方法依然在扫描隐藏的谷歌IP，待IP消耗完毕后，这些方法则会失效。

2018年初开始，墙的封锁方式从全部协议封锁改为了TCP封锁（阻断），主要特征为Ping IP正常连通（ICMP协议），而TCPing IP连接超时无回应（TCP协议）。因此可通过Ping IP和TCPing IP确定服务器IP是否被墙。

服务器IP被TCP封锁（阻断）后，依然可以正常的向服务器发送数据，但服务器返回数据时，墙发现IP在黑名单中，于是就会阻断、拦截，SSR上表现为超时或空连。

目前的代理软件基本都在使用TCP协议进行传输。TCP协议要传输数据先要进行握手，而当墙对海外代理服务器回程TCP阻断的时候，就会导致代理客户端与服务端无法完成握手，自然也无法使用代理。

墙通过分析流量特征判断该链接为代理的可能性，当可能性达到不同的程度时，墙做出不同的处理。其逻辑举例如下（10%/50%/80%等仅用作举例），

检测到一个链接是代理的可能性低于10%，那么就无视；高于10%，则将IP的相关信息（及收集的证据、特征等）记录到数据库中，当下次再发现这个链接时，继续去收集证据判断是否为代理，若代理几率降低则无视，否则进一步关注；高于50%，可能单独封一个端口（代理端口），若换个端口后继续做代理，则进一步关注，到了80%，所有端口都被封禁。

#### 解决方法

转换协议可以复活被墙的IP。转换为ICMP速度太慢，故转换为UDP较好，但国内运营商往往会对UDP QOS限速。能把TCP转为UDP的软件有KCPTun、Dragonite、V2ray（自带KCP协议）、Goflyway（自带KCP协议）。

也可使用CDN中转代理，而该方式需要WebSocket，故可用Goflyway的WS+CDN模式或V2ray的WS+CDN模式。同时需要通过伪装以保证CDN IP的安全性，在使用Goflyway时往往搭配HTTP伪装，而使用V2ray时往往搭配HTTP/HTTPS伪装。HTTP/HTTPS伪装有真正的网页，即浏览器打开域名会看到正常的网页，而代理客户端访问建立代理链接。

也可等待墙解封IP。墙考虑到海外大量的IDC和国内购买海外服务器正常使用者 的体验和情绪，会定期解封IP。每个做代理被墙的海外服务器IP，墙都会根据不同情况设定一个解封时间，
如果解封时间内该海外服务器IP没有向国内发送数据或者尝试建立代理连接，那么墙就会解封该IP。被解封的海外服务器IP，短时间内再用于代理用途，往往很快就又会封禁。

因此可以关闭或删除服务器上面的代理软件，然后闲置该服务器，不要Ping/TCPing这个服务器，一段时间后应当会解封。墙还可能定期对被墙IP的服务器主动探测，看看代理软件是否还在运行，所以必须要关闭代理软件。

#### 封锁几率

在日本、美国被墙几率高。

使用SSR代理时可搭配服务端的redirect参数做真实网站，或就用原版混淆插件（plain）。ShadowsocksR服务端伪装成正常网站流量的教程如下。

```
https://doubibackup.com/hi10k-7p-5.html
```

### DNS污染

#### 原理

DNS污染是指一些刻意制造或无意中制造出来的域名服务器分组，把域名指往不正确的IP地址。

一般来说，网站在互联网上一般都有可信赖的域名服务器，但为减免网络上的交通，一般的域名都会把外间的域名服务器数据暂存起来，待下次有其他机器要求解析域名时，可以立即提供服务。一旦有相关网域的局域域名服务器的缓存受到污染，就会把网域内的电脑导引往错误的服务器或服务器的网址。

假设A为用户端，B为DNS服务器，C为A到B链路中一个节点的网络设备（路由器、交换机、网关等）。现A需访问Google，故A向B通过UDP方式发送查询请求。这个数据在前往B时要经过C。DNS端口为53，C对此进行特定端口监视扫描，对UDP明文传输的DNS查询请求进行特征和关键词匹配分析，从而立刻返回一个错误的解析结果。作为链路上的一个节点，C必定比B更快的返回结果到A，而目前的DNS解析机制策略是只认第一，因此C所返回的查询结果就被A当作了最终结果，由此获得了错误的IP。

干扰也可以通过网络服务提供商提供的DNS服务器进行DNS欺骗，当使用此DNS服务器的网络用户访问此特定网站时，DNS服务便给出虚假的IP地址，导致访问网站失败，甚至返回ISP运营商提供的出错页面和广告页面。

可通过`nslookup`命令查看域名被解析出的IP地址。如果IP与域名服务器明显不同，如`8.7.198.45`，则可证明受到了DNS污染。

全球一共有13组根域名服务器，目前中国大陆有F、I这2个根域DNS镜像，但现在均已因为多次DNS污染外国网络，而被断开与国际互联网的连接。

#### 解决方法

对于站长，换域名或者IP即可。

对于用户，可使用翻墙软件。也可在hosts添加受到污染的DNS地址，或使用DNSCrypt软件。

若使用Firefox，则可打开远程DNS解析功能。在地址栏中输入`about:config`，
找到`network.proxy.socks_remote_dns`，改成true即可。

### SSL连接阻断

墙会阻断特定网站的SSL加密连接，方法是通过伪装成对方向连接两端的计算机发送RST包（RESET）干扰两者间正常的TCP连接，进而打断与特定IP地址之间的SSL（HTTPS，443端口）握手（如Gmail、Google文件、Google网上论坛等的SSL加密连接），从而导致SSL连接失败。

### HTTP劫持

#### 原理

HTTP传输协议是明文的，若网站服务器在海外，访问网站时就要通过国际宽带出口。在通过时，若被墙扫描到违规关键词，墙则会阻断TCP连接，主要错误为链接已重置、该网站已永久移动到其他地址等。

#### 解决方法

HTTP劫持可通过加SSL证书解决，网站全部内容都会被加密。但由于HTTPS在建立加密连接的时候需要一次握手，而这次握手是明文的，所以如果域名被重点关注，即使加上了SSL证书，也会在首次握手的时候由于关键词匹配而阻断TCP连接。

另一解决方法为域名备案并使用国内服务器，则不会通过墙。

### 封锁方式的选择

采用IP封锁时，所有的骨干路由器和国际出口的路由器都添加上和Google服务有关的IP黑名单，带有相关IP地址的数据包直接在半路就被丢包，这会给路由器带来很大负担且成本非常高。这种方法针对的多为非常重要的网站，对于普通网站，采用DNS污染即可。

### 对于Google的特殊操作

墙对Google部分服务器的IP地址实施某些端口的自动封锁，按时段对`www.google.com`和`mail.google.com`的几十个IP地址的443端口实施自动封锁，具体是每10或15分钟可以连通，接着断开，10或15分钟后再连通，再断开，如此循环，令中国大陆用户和Google主机之间的连接出现间歇性中断，使其各项服务出现问题，由此显得问题出在Google自身。

## 网络层级

以下层级由高到低。

|    层级    |        在本层运行的工具       |
|------------|-------------------------------|
| 应用层     | 浏览器                        |
| 表示层     |                               |
| 会话层     | SS/V2Ray/Trojan               |
| 传输层     | 游戏                          |
| 网络层     | ICMP（ping/trace）            |
| 数据链路层 | VPN（PPTP/L2TP/OpenVPN）/IEPL |
| 物理层     | IPLC                          |

## TCP与UDP

TCP为两者点对点连接，UDP为广播连接。

## 网页

### 论坛

```
https://limbopro.xyz/
https://fanqiangdang.com/
https://www.ssrshare.com/
https://fangeqiang.com/
https://www.flyzy2005.com/
https://iyideng.cloud/
https://52bp.org/index.html
https://vpncn.blogspot.com/
https://doubibackup.com/
https://www.vpsdawanjia.com/
https://program-think.blogspot.com/
https://program-think.medium.com/

// 已遍历至2020-10-29【2020年双11活动集中分享！看这里就够了！】
https://51.ruyo.net/
```

### 免费VPS

```
https://freeserver.us/
https://www.youneed.win/
```

# 现成工具

## 电脑端软件

### 翻墙Chrome浏览器

```
// 适用于XP以上系统
https://github.com/Alvin9999/new-pac/wiki/%E9%AB%98%E5%86%85%E6%A0%B8%E7%89%88

// 适用于XP
https://github.com/Alvin9999/new-pac/wiki/%E4%BD%8E%E5%86%85%E6%A0%B8%E7%89%88
```

### 翻墙Firefox浏览器

```
https://github.com/Alvin9999/new-pac/wiki/%E7%81%AB%E7%8B%90%E7%BF%BB%E5%A2%99%E6%B5%8F%E8%A7%88%E5%99%A8
```

### 无界浏览

```
// Windows
https://www.lanzous.com/i4eyu4b

// Mac
https://www.lanzous.com/i4eyt1c
```

### Hotspot Shield

可在美区的Mac App Store找到。

### windscribe

```
https://chn.windscribe.com/download
```

### ProtonVPN

```
https://protonvpn.com/download
```

### Psiphon

```
https://psiphon.ca/
```

### IPUnblock

```
http://ipunblock.com/
```

### tuxler

```
https://tuxler.com/
```

### Privatix

```
https://privatix.com/
```

### WireGuard

打开以下链接以下载客户端。

```
https://www.wireguard.com/install/
```

打开WireGuard，选择add empty tunnel，复制public key。打开以下链接并将刚才的内容复制到your wireguard public key，点击Add key继续。

```
https://cryptostorm.is/wireguard
```

将生成的配置文件复制到WireGuard的public key下方配置信息框内，并删除以下字段。点击active激活即可。

```
[Interface]
PrivateKey = YOUR_PRIVATE_KEY
```

### OpenVPN

下载以下客户端。

```
https://openvpn.net/download-open-vpn/
```

也可下载以下第三方客户端。

```
https://www.sparklabs.com/viscosity/
```

可通过以下网站获取OpenVPN的配置文件。下载配置文件后导入到客户端即可。

```
https://www.vpnbook.com/freevpn
https://www.vpngate.net/en/
```

## 手机端软件

### Betternet/VPN 360

可在美区的App Store找到。

### VPN Plus

iOS版可在美区的App Store找到。安卓版下载地址如下。

```
（Android）https://www.lanzous.com/i4ez8wd
```

### 老王VPN

可在Google Play下载。若能连接到Google Play，可找到非常多类似的软件。

```
https://www.lanzous.com/i4ezbla
```

### 蚂蚁加速器

```
https://b.lausera.com/c-1/a-agWjQ/
```

## 谷歌镜像

### 基本知识

#### 分类

镜像网站有如下类别。

| 种类 |                                                   含义                                                   |
|------|----------------------------------------------------------------------------------------------------------|
| F1   | 通过服务器（如Nginx）反向代理，服务器在海外或为IPV6                                                      |
| F2   | 通过爬虫程序实时爬取并展示Google搜索的结果页                                                             |
| F3   | 通过定时全量数据同步，进行镜像分身，达到持久化的镜像，例如composer镜像或npm镜像，谷歌镜像不能使用这种方法 |

#### 和代理的区别

代理是设置代理服务器，代理服务器转发下载请求，从而实现类似谷歌镜像的需求。

镜像是镜像站点已经提前镜像（下载）了资源，访问时直接从它的服务器上获取资源。

### 网站

```
https://www.gosearchresults.com/
https://www.lovec.ltd/
https://www.sanzhima.com/
http://g.histsci.org/
http://google.seek68.cn/
https://x.glgoo.top/scholar/
https://ac.scmor.com/
https://v2ray.party/
http://scholar.hedasudi.com/
https://ac.scmor.com/
https://google2.jiongjun.cc/
https://siguso.com/google/
https://ac.scmor.com/
https://coderschool.cn/1853.html
```

## Chrome插件

### 谷歌访问助手破解版

```
https://www.lanzous.com/ial9x8b
```

### Ghelper

```
http://googlehelper.net/
```

### Chrome商店访问助手

```
http://www.ggfwzs.com/
```

### SetupVPN

```
https://setupvpn.com/download/
```

### Tunhello

```
https://tunnello.com/
```

## 代理

该方法适合所有的系统，包括Linux和较旧的Windows系统，如XP。

### 寻找代理节点

#### 通过网站

在Google搜索`免费代理节点`即可。

```
http://free-proxy.cz/
http://www.gatherproxy.com/zh/sockslist
https://www.vpngate.net/en/
http://www.proxylists.net/
http://www.freeproxylists.net/zh/
https://free-proxy-list.net/
https://www.my-proxy.com/free-proxy-list.html
https://sockslist.net/
https://premproxy.com/list/
```

#### 通过工具

下载`ProxyScrape`，打开后选择`Check proxies`，填写所要求的代理服务器的`ping值`，ping值越大，表明延迟越大。一般来说，公用的服务器ping值都会在100ms以上，因此可取400-500。填写检查`代理服务器的数量`，选择`代理服务器的协议类型`，选择`Check from ProxyScrape.com`，工具就会开始筛选代理，并把可用的代理用绿色表示出来。

```
https://proxyscrape.com/proxy-checker
```

### 配置网络代理

#### 设置软件代理

以网易云音乐为例，点击设置-工具，在代理选项卡下有三个选项，若选择使用IE代理设置，则需要提前设置好IE的代理。如果选择自定义代理，则在下拉菜单中选择代理类型，并输入服务器与端口（比如说所得结果为`1.2.3.4:5`，则服务器地址为1.2.3.4，端口为5），保存即可。

#### 设置IE代理

打开IE，点击右上方设置-Internet选项，选择连接-局域网设置，勾选`为LAN使用代理服务器`，填好地址和端口，并勾选`对于本机地址不使用代理服务器`，保存并退出。

#### 设置全局代理

打开系统的设置-网络和Internet-代理，选择`使用代理服务器`，填好地址和端口，并保存。

#### 设置浏览器代理

以Chrome为例，打开Chrome网上应用店，搜索`SwitchyOmega`并安装（需要外网环境）。

下载完成后点击插件的选项，进入配置页面。点击情景模式的`proxy`，填写代理协议、代理服务器和端口。点击`auto switch`，切换规则只留`proxy`，默认情景模式设为`直接连接`，在导入在线规则列表下，点击`添加规则列表`并填入下面网址，点击`立即更新情景模式`，保存后退出。此时可以禁用系统设置中的网络代理。

点击插件，选择auto switch或proxy，即可访问外网。其中proxy相当于全局模式，auto switch相当于PAC模式。

```
https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt
```

## 机场及公用节点

### 友明互联

0.01元/天。

```
http://idc.ymhlw.cn/index.php/buy/index/
```

### 乐云

2元/月。

```
https://www.renzhijia.com/buy/index/7/
```

### Goflyway免费账号

```
https://github.com/Alvin9999/new-pac/wiki/Goflyway%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7
```

### 公用节点

```
https://freevpn4you.net/ru/free-proxy.php
https://github.com/hugetiny/awesome-vpn/blob/master/READMECN.md
https://free-ss.site/
https://yzzz.ml/freessr/
https://www.liesauer.net/yogurt/subscribe?ACCESS_TOKEN=DAYxR3mMaZAsaqUb
https://prom-php.herokuapp.com/cloudfra_ssr.txt
https://view.freev2ray.org/
https://node.umelabs.dev/
https://www.youneed.win/free-ssr
https://www.youneed.win/free-ss
https://lncn.org/
https://www.nutgeek.com/ssshadowsocks/
https://usky.ml/tool/free_ssr
https://trial.ssbit.win/
https://www.go2free.xyz/
https://gdmi.weebly.com/3118523398online.html
https://free.yitianjianss.com/
https://ssrtool.us/tool/free_ssr
https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7
https://github.com/Alvin9999/new-pac/wiki/v2ray%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7
http://ss.pythonic.life/
https://www.vpngate.net/cn/
```

## SSH隧道

### 使用

打开以下链接以创建账号。

```
https://www.speedssh.com/
```

下载以下客户端并输入从上述网站获取的SSH信息，22为SSH默认端口。登录后进入Services，勾选`SOCKS/HTTP proxy forwarding`，然后按照设置代理的方式进行设置即可。

```
https://www.bitvise.com/ssh-client-download
```

也可通过以下网站获取SSH账号。

```
https://skyssh.com/
https://www.mytunneling.com/
https://bestvpnssh.com/
https://fullssh.com/
https://www.sshagan.net/
https://www.portssh.com/
https://www.jetssh.com/
http://free-ssh.xyz/
https://sshkit.com/
https://www.monthlyssh.com/ssh
https://speedssh.com/
https://sshdropbear.net/
https://fastssh.com/
https://createssh.com/
https://cloudssh.us/
```

## 获取工具的方式

### 搜索引擎

使用github/qwant/telegram搜索相关翻墙软件的关键词，可使用similarsitesearch查询相似站点。

### 邮件

有些提供翻墙工具的公司/组织会开设一个邮箱，用于自动回复科学上网工具以及页面镜像地址，如发送标题`help`到`get@psiphon3.com`。

### P2P下载

连上某个eMule服务器后即可搜索Tor等相关工具。

```
https://www.emule-project.net/home/perl/general.cgi?l=42
```

# 自建服务器

通过国外服务器上搭建专门的翻墙协议，实现翻墙效果。

## Google Cloud Platform

### 申请

在Google搜索GCP即可进入Google Cloud Platform的官网。GCP是谷歌提供的服务器，新用户注册可免费获取300美元的赠金，在12个月内使用。

注册GCP的关键是通过银行卡认证。注册完成后点击左上角的菜单，选择`结算`，可以看到剩余赠金和剩余试用日期。信用卡验证相关操作可查看附录。

### 搭建

#### 创建防火墙规则

点击菜单中的VPC网络-防火墙规则，进入防火墙规则页面后，点击`创建防火墙规则`，进入防火墙规则创建界面，配置如下。完成配置后点击创建即可。

|        项目        |        值        |
|--------------------|------------------|
| 名称               | entrance         |
| 优先级             | 1                |
| 流量方向           | 入站             |
| 对匹配项执行的操作 | 允许             |
| 目标               | 网络中的所有实例 |
| 来源IP地址范围     | 0.0.0.0/0        |
| 协议和端口         | 全部允许         |


同理，再创建一个防火墙实例，名称改为`exit`，流量方向为`出站`，其余与上面的一致，保存。

#### 创建VM实例

点击菜单中的Compute Engine-VM实例，点击创建，进入VM实例创建页面，配置如下。

|   项目   |                     值                     |
|----------|--------------------------------------------|
| 区域     | 建议asia-east2（香港）或asia-east1（台湾） |
| 机器类型 | 建议微型                                   |
| 防火墙   | 勾选允许HTTP流量、允许HTTPS流量            |
| **网络** |                                            |
| 网络标记 | 刚才创建好的防火墙规则（enterance、exit）  |

创建完成后会有一个内部IP和一个外部IP，打开命令提示符，输入`ping [外部IP]`查看ping值。如果ping值过大，则再创建一个新的实例，直到得到一台ping值小的服务器，注意不要把原来的实例删除，不然会分配到同一台服务器。正常服务器ping值在100ms以内。

#### 搭建V2Ray协议

##### 运行一键脚本

点击VM实例上的`SSH`按钮，等待命令行窗口出现。在命令行中输入以下命令以获取管理员权限，并执行V2Ray一键安装脚本。

```
sudo -i
bash <(curl -s -L https://git.io/v2ray.sh)
```

##### 协议配置

###### TCP协议

在一键脚本配置协议时选择默认的`TCP`协议即可。TCP协议速度快，但有被封端口的风险。

###### WS+TLS协议

该协议安全性高，但需要域名且速度不及TCP协议。

进入以下网站并注册一个cloudflare账号，然后把注册好的域名加进去，添加时选择免费套餐。

```
https://www.cloudflare.com/zh-cn/
```

向域名的`DNS`添加一个A记录，Name为二级域名，比如`hk.nionguan.ga`，Value为服务器的IP地址，注册时`Proxy status`点成`DNS Only`。

然后到购买域名网页的后台，更改域名的Nameserver为cloudflare中指定的网址。更改完成后返回cloudflare，点击`Done, Check...`并等待生效。

完成网站配置后，在脚本配置协议时选择WS+TLS协议即可。

##### 多用户配置

请在完成TCP协议和WS+TLS协议的配置后进行以下操作。

在服务器输入以下命令以编辑V2Ray的配置文件。

```
vim /etc/v2ray/config.json
```

找到inbound段的代码并进行如下修改，注意每个用户的UUID应当一致。

```
"inbounds": [{
  "port": 8089, // WS+TLS
  "protocol": "vmess",
  "settings": {
    "clients": [{
      "id": "71880ee2-4d15-47da-87b2-xxxxxxxxxx",
      "alterId": 64
    }]
  },
  "streamSettings": {
    "network": "ws",
    "wsSettings": {
      "path": "/nidebeibei"
    }
  }
}, {
  "port": 8288, // TCP
  "protocol": "vmess",
  "settings": {
    "clients": [{
      "id": "71880ee2-4d15-47da-87b2-xxxxxxxxxx",
      "alterId": 64
    }]
  }
}]
```

修改完成后重启V2Ray服务即可生效。

```
sudo systemctl restart v2ray
```

##### 配置分享

得到V2Ray配置信息后复制，并输入`v2ray ssqr`生成二维码，以得到客户端配置。客户端配置方法可查看客户端配置一节。

### 功能

#### 文件传输

在浏览器中点击SSH按钮并弹出窗口后，可点击窗口右上角，选择文件传输功能以从服务器下载内容，或上传内容到服务器。

#### 快照

GCP提供的快照功能能够备份磁盘内容，当内容丢失后可以用于恢复。新建VM实例时也可通过快照建立。

点击Compute Engine下的快照，建立即可。建立完成后，直接点击快照详情，即可用该快照建立实例，其磁盘内容完全一致。

#### 镜像

GCP可通过Docker Hub上的镜像部署VM实例。在部署示例时勾选`将镜像应用到该实例`，并复制该实例的完整地址即可，如`hub.docker.com/_/wordpress`。

## GoormIDE

GoormIDE可以永久免费使用一个含1GB内存、10GB存储空间的VPS，但不能保持常开。

### 申请

打开以下链接并完成注册即可。

```
https://ide.goorm.io/
```

### 搭建

进入控制台后点击`New Container`。地区最好选择韩国，类型选择Private，选择新建。新建完成后点击Run，然后点击Terminal进行连接。

#### Socks5连接协议

输入以下命令安装。

```
bash <(curl -s -L https://raw.githubusercontent.com/guleonseon/goorm-auto/master/install.sh)
```

回到VPS设置页，在Port Forwarding处填写`1080`并点击完成。客户端配置如下。

```
类型 / socks5
地址 / VPS的IP地址
端口 / 原端口（不是1080）
```

#### vmess连接协议

输入以下命令以通过一键脚本安装，安装完成后记下配置，不要关闭窗口。

```
bash <(curl -s -L https://git.io/v2ray.sh)
```

回到VPS设置页，在Port Forwarding处填写刚才记下的端口号并点击完成，然后记下后面系统分配的新的端口号。在添加连接时，需要用新端口号替换旧的端口号，其余配置与脚本提供的一致。

返回终端页，运行以下命令以保持V2Ray运行。使用过程中需一直停留在该标签页上，每次重新开启服务器时，需查看配置是否被改变。

```
/usr/bin/v2ray/v2ray -config /etc/v2ray/config.json
```

## Heroku

### 申请

打开以下链接并注册，注意需要翻墙环境。

```
https://dashboard.heroku.com/
```

### 搭建

#### V2Ray

打开以下链接部署V2Ray，注意数据中心即为翻墙服务器地址。

```
https://dashboard.heroku.com/new?template=https%3A%2F%2Fgithub.com%2Fbclswl0827%2Fv2ray-heroku
```

完成部署后在后台查看刚才新建的项目，点击`Reveal Config Vars`并复制UUID，在`Domains`下复制域名。打开客户端并进行配置，其中地址为刚才的域名，端口为443，用户ID为刚才的UUID，alterID为64，传输协议为ws，打开TLS并允许不安全连接即可。

#### Shadowsocks

步骤基本同上，仓库如下。

```
https://github.com/onplus/shadowsocks-heroku
```

客户端配置中，地址为app域名，端口为80，混淆为websocket，路径为`/`。

### Heroku CLI

Heroku CLI可用于在本地通过命令部署应用，下载链接如下。

```
https://devcenter.heroku.com/articles/heroku-cli#download-and-install
```

## EUserv

### 申请

打开以下链接以注册。注意注册时地址选择中国，地址尽量真实，不需要翻墙环境。审核需要24-48小时，会核对注册时所用的IP地址和选择的地区是否匹配。若注册不通过则会被删除账号。

```
https://www.euserv.com/en/virtual-private-server/root-vserver/v2/vs2-free.php
```

### 搭建

#### 重装系统

在后台点击`VSERVER`，看到服务器申请状态成功，即可点击`Select`进入服务器详情。进行系统安装时系统选择`CentOS 7`，然后点击左侧的`Serverdata`查看服务器信息，若Default-Password出现，则系统安装成功。

#### SSH连接

由于该服务器只提供IPV6地址，而普通路由器不具备解析IPV6地址的能力，因此需要通过其它方式进行连接。

##### 通过手机热点

手机热点自带IPV6地址。将电脑连接手机热点后，输入以下命令以确认服务器是否能够连接。

```
// Windows
ping -6 [IPV6地址]

// Mac
ping6 [IPV6地址]
```

然后通过正常的SSH连接即可。

也可在手机端直接完成。安装`1.1.1.1`这一APP并连接，然后通过SSH软件连接即可。

##### 通过网站

打开以下网站并注册，然后依次新建组织、项目、资产。

```
https://console.heyterm.com/
```

其中新建资产时相关配置如下。

```
IP地址 / IPV6地址或服务器域名
用户名 / root
密码 / Default-Password的内容
服务器节点 / 荷兰或德国
```

新建完成后点击终端按钮即可完成连接。

#### IPV4配置

输入以下命令以更改DNS解析，使其可访问IPV4网络。

```
echo -e "nameserver 2001:67c:2b0::4\nnameserver 2001:67c:2b0::6" > /etc/resolv.conf
```

可通过以下命令查看是否设置成功。

```
vim /etc/resolv.conf
```

然后通过以下命令修改文件，将`enable=1`改为`enable=0`后按照vim的方法保存（按ESC后输入:wq并回车）。

```
vi /etc/yum/pluginconf.d/fastestmirror.conf
```

然后执行以下命令以进行升级并安装带有IPV6的宝塔。

```
yum update
curl -sSO http://download.bt.cn/install/new_install.sh && bash new_install.sh
```

安装完成后输入`bt`进入管理界面，执行8修改端口为8080，执行5和6修改用户名和密码。退出管理脚本后执行以下命令以取消登录面板安全保护。

```
rm -f /www/server/panel/data/admin_path.pl
```

为访问宝塔面板，需要进行域名解析。进入Cloudflare的后台，在`DNS
`页选择一个域名后添加一条记录，内容如下。

```
类型 / AAAA
名称 / 二级域名，可任意
内容 / IPV6地址
代理状态 / 已代理

// 若上一条记录无法生效，则使用以下配置
类型 / CNAME
名称 / 二级域名，可任意
内容 / 域名，即Server name
代理状态 / 已代理
```

然后在`SSL/TLS`页修改模式为`Full (Strict)`，在`Firewall`页将`Security Level`改为`Essentially Off`。

#### 宝塔配置

完成添加后通过以下链接访问宝塔面板，输入改好的用户名和密码即可登录。

```
[二级域名].[主域名]:8080
```

登录后安装推荐的LNMP，安装完成后点击左侧的`软件商店`，分类选择`一键部署`，选择`博客`类型下的`WordPress`。域名填写登录宝塔面板的域名，注意不包括端口号，然后提交。

复制弹出窗口中的数据库账号资料，然后点击访问站点下的链接进行部署，其中相关信息需要填写刚才复制下来的内容。点击安装并等待完成即可。

部署完成后回到宝塔面板，点击左侧的`网站`，可以看到刚才部署的WordPress博客。点击`设置`，在`SSL`下选择`Let's Encrypt`，勾选域名并安装SSL证书。完成后可访问博客，查看能否正常运行。

#### 安装V2Ray

在完成网站搭建后即可开始安装V2Ray。通过以下命令使用一键脚本安装即可。

```
bash <(curl -s -L https://git.io/v2ray.sh)
```

安装完成后利用宝塔面板的文件功能打开etc/v2ray/config.json，用以下配置替换。其中clients-id为自己生成的UUID。

```
{
  "inbounds": [
    {
      "port": 10000, // 可任意
      "listen":"127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
          "id": "8195adc7-221d-4390-a9b2-d7642e44e46d", // 见下面说明
          "level": 1,
          "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/hello" // 可任意
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}

```

完成后回到宝塔面板，点击`网站`-`设置`-`配置文件`，在最后一个`}`前加入以下代码。

```
location /hello { // 与上述配置文件中path下的目录一致
        proxy_redirect off;
        proxy_pass http://127.0.0.1:10000; // 端口与上述配置文件中的port一致
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;
        # Show realip in v2ray access.log
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
```

然后输入以下命令使V2Ray和nginx开机自启，并重启V2Ray使配置生效。

```
systemctl enable v2ray nginx
systemctl restart v2ray
```

然后需要在宝塔打开端口。点击`安全`，添加上述所使用的端口为放行端口，此处为10000。添加完成后服务端配置即完成，客户端配置如下。

```
地址 / 访问WordPress的地址
端口 / 443
用户ID / 配置文件的clients-id
额外ID / 64
传输协议 / ws
路径 / 配置文件的streamSettings-path
TLS / 打开，允许不安全
```

注意，安装完成后千万不要运行bbrplus加速脚本，否则将导致服务器无法连接。

## ~~Okteto~~

现Okteto政策已不允许将云用于搭建代理。

### 失效教程

<details>
<summary></summary>

打开以下网页并用Github登录。

```
https://cloud.okteto.com
```

需在本机安装Okteto工具以完成部署。对于MacOS，在终端运行以下命令。

```
brew install okteto
```

对于Windows，下载以下两个文件并放到`C:\Windows\System32`。

```
https://downloads.okteto.com/cli/okteto.exe
https://storage.googleapis.com/kubernetes-release/release/v1.18.0/bin/windows/amd64/kubectl.exe
```

其它平台可查看以下链接。

```
https://okteto.com/docs/getting-started/index.html
https://kubernetes.io/docs/tasks/tools/install-kubectl
```

macOS下进入终端，Windows下管理员身份运行cmd，执行以下命令。如果无法登录，则更换系统环境。

```
okteto login
okteto namespace
```

新建文本文件，名称为`v2ray.yml`，内容如下。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: v2-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: v2-app
  template:
    metadata:
      labels:
        app: v2-app
    spec:
      containers:
      - image: gingko/v2ray-nginx-websocket
        name: v2-app

---

apiVersion: v1
kind: Service
metadata:
  name: v2-app
  annotations:
    dev.okteto.com/auto-ingress: "true"
spec:
  type: ClusterIP  
  ports:
  - name: "http-port-tcp"
    port: 8080
  selector:
    app: v2-app
```

保存后执行以下命令以部署。

```
kubectl apply -f [yml文件路径]
```

在客户端添加以下配置即可。

```
地址 / Endpoints的地址
端口 / 443
用户ID / f3c9cb27-746f-4e41-acf2-820bd3002676
传输协议 / ws
路径 / /fuckgfw_letscrossgfw
TLS / 打开，允许不安全
额外ID / 100
```
</details>

### 失效教程（旧版）

<details>
<summary></summary>

打开以下仓库并fork到自己的Github上。

```
https://github.com/byxiaopeng/okteto-reboot
```

在自己Fork的项目下点击`Settings`-`Secrets`，建立以下Secret。

```
名称 / APITOKEN
内容 / 在Okteto控制台点击Settings，复制API Token到此

名称 / NAMESPACE
内容 / 控制台左上角的标注
```

回到控制台，点击`Deploy`，选择`Deploy from Git Repository`，仓库地址为刚才Fork的仓库，进行部署。

部署完成后，在客户端进行以下配置即可。

```
地址 / Endpoints的地址
端口 / 443
用户ID / ad806487-2d26-4636-98b6-ab85cc8521f7
传输协议 / ws
路径 / /ws
TLS / 打开，允许不安全
```
</details>

## ~~Kubesail~~

现在需要自行提供服务器。

### 失效教程

<details>
<summary></summary>

打开以下链接并完成注册。

```
https://kubesail.com
```

打开以下链接，Fork一份到自己的Github。然后在Kubesail后台点击`Repos`，确认已经连接到自己的Github账号。

```
https://github.com/bclswl0827/v2ray-openshift
```

打开以下链接，Fork一份后点击左侧的`Templates`，进入刚才fork的项目，点击`Settings`，将`Container Image`的路径更改为`[Github用户名]/v2ray-openshift`。点击`Launch Template`开始部署。点击`Status`，看到V2Ray在运行时则部署成功。

若部署失败，则点击左侧的`Resources`，将`Apps`下项目的`Settings`-`Container Image`更改回`bclswl0827/v2ray-openshift`，保存。

```
https://kubesail.com/template/bclswl0827/v2ray
```

部署成功后需要将项目暴露在域名下。点击`Domains`，添加一个已经在CloudFlare部署好的网址，注意不要保留二级域名和其它路径。系统将会要求添加一个TXT记录完成验证，在CloudFlare后台添加即可，名称填`@`或者`root`。完成验证后按照提示添加一个CNAME记录，名称填`@`或者`root`，内容填系统所给出的网址，类型选择`DNS Only`，保存。

完成网址部署后返回刚才的项目，点击`Network`-`Ingress`，`Domain`选择刚才新建的域名，`subdomain`留空，保存。

至此已将项目暴露到网页上，在客户端添加相关配置即可，具体如下。

```
地址 / 绑定的域名，注意没有二级域名和其它路径
端口 / 443
用户ID / 在项目中点击Edit YAML，在clients-id下
AlterID / 64
传输方式 / ws
路径 / /ws
TLS / 打开，且允许不安全
```
</details>

## ~~IBM Cloud~~

免费30天，免费期后会自动注销账号。

### 失效教程

<details>
<summary></summary>

打开以下链接进行注册即可。

```
https://cloud.ibm.com/
```

进入后台并点击`创建资源`，创建一个Cloud Foundray下的公共应用程序。区域选择达拉斯，套餐选择256MB，配置资源选择Go，名称填ibmyes，完成创建。

创建完成后进入项目并点击右上角的终端符号以进入终端，注意切换地区为Dallas。然后运行以下脚本进行安装即可。安装完成后用分享链接即可导入配置到客户端。

```
wget --no-check-certificate -O install.sh https://raw.githubusercontent.com/CCChieh/IBMYes/master/install.sh && chmod +x install.sh  && ./install.sh
```

由于IBM Cloud十天不操作就会关机，所以需要十天重启一次。在IBM Cloud终端输入以下命令以获取相关参数。

```
ibmcloud login
ibmcloud resource groups
```

其中Region中显示的地区需转换为区域编号，即`REGION_NUM`，对应关系如下。而第二个命令执行后ID一栏为资源组id，即`RESOURSE_ID`。

```
1 / au-syd
2 / in-che
3 / jp-tok
4 / kr-seo
5 / eu-de
6 / eu-gb
7 / us-south
8 / us-east
```

登录Github并Fork以下项目。

```
https://github.com/CCChieh/IBMYes
```

在自己Fork的项目下点击`Settings`-`Secrets`，建立以下四个Secret。

```
名称 / IBM_ACCOUNT
内容 / 第一行账号，第二行密码

名称 / IBM_APP_NAME
内容 / ibmyes

名称 / REGION_NUM
内容 / 前面记下的区域编号

名称 / RESOURSE_ID
内容 / 前面记下的资源组id
```

保存后点击上方Actions，应当看到IBM Cloud Auto Restart在执行。如果没有此Action，到自己仓库的/.github/workflows/ibm.yml，随便编辑后点击`Start commit`即可看到。
</details>

# 客户端配置

## 连接到服务器

### V2Ray

#### Windows XP以上

##### V2RayN

下载V2RayN-Core.zip，解压并打开`v2rayN.exe`。点击服务器，选择扫描屏幕上的二维码，客户端会自动扫描刚才生成的二维码并添加配置信息。如果无法生成二维码，则点击添加Vmess服务器，手动输入刚才保存的配置信息。

右键点击新增的服务器，选择设为活动服务器。然后在V2RayN的托盘图标点击右键，勾选启用http代理，并在http代理模式中选择PAC模式即可。

```
https://github.com/2dust/v2rayN/releases/
```

#### Windows XP

##### 安装

从以下链接下载Alvin9999包含V2Ray作为翻墙工具的Chrome翻墙浏览器包，此处为AllNew全新版。

```
https://github.com/Alvin9999/new-pac/wiki/%E4%BD%8E%E5%86%85%E6%A0%B8%E7%89%88
```

该包中V2Ray和goflyway均可用。对于goflyway，双击打开后可见本地代理，在浏览器中填写相应地址即可。

此处以V2Ray包进行说明。提取压缩包中的V2Ray文件夹，并修改里面的config.json为如下内容。

```
{
  "log": {
    "access": "",
    "error": "",
    "loglevel": "warning"
  },
  "inbound": {
    "port": 1080,
    "listen": "127.0.0.1",

// procotol为V2Ray所映射代理的协议，默认为socks（socks5），由于XP原生不支持socks5，故改为http
    "protocol": "http",
    "domainOverride": [
      "tls",
      "http"
    ],
    "settings": {
      "auth": "noauth",
      "udp": true,
      "ip": "127.0.0.1",
      "clients": null
    },
    "streamSettings": null
  },
  "outbound": {
    "tag": "agentout",
    "protocol": "vmess",
    "settings": {
      "vnext": [
        {

// 服务器地址及端口，需修改为自己的服务器IP或域名
          "address": "server", 
          "port": 10086,  

// 此处配置应与服务器配置的信息一致
          "users": [
            {
              "id": "b831381d-6324-4d53-ad4f-8cda48b30811", 
              "alterId": 0,
              "email": "t@t.tt",
              "security": "aes-128-gcm"
            }
          ]
        }
      ],
      "servers": null
    },
    "streamSettings": {
      "network": "tcp",
      "security": "",
      "tlsSettings": null,
      "tcpSettings": null,
      "kcpSettings": null,
      "wsSettings": null,
      "httpSettings": null
    },
    "mux": {
      "enabled": true
    }
  },
  "inboundDetour": null,
  "outboundDetour": [
    {
      "protocol": "freedom",
      "settings": {
        "response": null
      },
      "tag": "direct"
    },
    {
      "protocol": "blackhole",
      "settings": {
        "response": {
          "type": "http"
        }
      },
      "tag": "blockout"
    }
  ],
  "dns": {
    "servers": [
      "8.8.8.8",
      "8.8.4.4",
      "localhost"
    ]
  },
  "routing": {
    "strategy": "rules",
    "settings": {
      "domainStrategy": "IPIfNonMatch",
      "rules": [
        {
          "type": "field",
          "port": null,
          "outboundTag": "direct",
          "ip": [
            "0.0.0.0/8",
            "10.0.0.0/8",
            "100.64.0.0/10",
            "127.0.0.0/8",
            "169.254.0.0/16",
            "172.16.0.0/12",
            "192.0.0.0/24",
            "192.0.2.0/24",
            "192.168.0.0/16",
            "198.18.0.0/15",
            "198.51.100.0/24",
            "203.0.113.0/24",
            "::1/128",
            "fc00::/7",
            "fe80::/10"
          ],
          "domain": null
        }
      ]
    }
  }
}
```

精简文件夹，只保留以下文件。双击v2ray.exe即跳出V2Ray窗口，示意V2Ray已经运行。若运行wv2ray.exe，则不显示窗口，在后台静默运行。

```
├── doc
│   └── ...
├── config.json
├── geoip.dat
├── geosite.dat
├── vpoint_socks_vmess.json
├── vpoint_vmess_freedom.json
├── v2ctl.exe
├── v2ctl.exe.sig
├── v2ray.exe
├── w2vray.exe
└── w2ray.exe.sig
```

##### 设置代理

###### 软件代理

同理，这时V2Ray已经把服务器映射到本地代理127.0.0.1:1080，注意类型为http。XP中可以配置IE代理，也可在Chrome浏览器中使用SwitchOmega配置，配置如下。

```
类型 / http
服务器地址 / 127.0.0.1
端口 / 1080
```

###### 全局代理

若需要实现全局翻墙，则需要用到翻墙工具Proxifier。

安装并打开软件，点击`Profile`菜单下的`Proxy Servers`，添加代理，配置同上。然后点击`Proxification Rules`修改翻墙规则，其中v2ray.exe、wv2ray.exe和翻墙服务器的地址（34.xx.xx.xx）需要使用direct模式，否则将会出现循环代理的情况。配置完成后点击`Name Resolution`，勾选`Resolve hostnames through proxy`以防止DNS污染，保存即可。使用时打开v2ray.exe或wv2ray.exe，并打开Proxifier的开关即可。

为启动方便，可通过以下bat程序一键打开和关闭全局翻墙。打开记事本，复制代码并保存为相应文件后执行即可。

```
// 保存为startglobal.bat
start wv2ray.exe
stop Proxifier.exe

// 保存为stopglobal.bat
taskkill /f /im Proxifier.exe
taskkill /f /im wv2ray.exe
```

由于Proxifier支持socks5协议，而SwitchOmega在XP下并不支持，因此若V2Ray选用socks5协议，只能实现全局翻墙而不能实现单一浏览器翻墙。

#### Mac

##### V2RayX

与Windows客户端的配置类似。打开APP后点击菜单中的Configure，输入刚才保存的服务器配置信息。然后在Servers中选择刚才新建的服务器，确保V2Ray code处于load状态，并勾选PAC Mode，配置完成。

```
https://github.com/insisttech/v2rayX-copy/releases
```

##### V2RayU

与V2RayX类似，但提供订阅功能。

```
https://github.com/yanue/V2rayU/releases/
```

#### Android

##### V2RayNG

下载app-universal-release.apk，安装到手机并打开。点击上方+号并选择从二维码导入配置，扫描从服务端获取的二维码即可。也可点击手动输入进行导入。

然后点击左上角的菜单，选择设置，在路由模式中选择`绕过中国大陆`。回到主页面，点击右下角的启动按钮，允许连接。

```
https://github.com/2dust/v2rayNG/releases
```

#### Linux

Linux没有图形客户端，故须按照以下配置。

下载`v2ray-linux-64.zip`，解压后放于`/tmp/v2ray`，并改名为`v2ray.zip`。

```
https://github.com/v2ray/v2ray-core/releases
```

打开终端并输入命令，下载好的脚本会放于终端工作路径。若无法下载，可使用文末附有的代码。

```
wget https://install.direct/go.sh
```

用文本编辑器打开下载好的go.sh，找到`downloadV2Ray()`函数，删除函数内所有代码并`return 0`，如下所示。

```
downloadV2Ray(){
    return 0
}
```

修改完成后在终端输入命令，等待V2Ray安装完成。

```
./go.sh
```

安装完成后输入以下命令以打开文件管理器。

```
sudo nautilus
```

转到`/etc/v2ray/config.json`，打开并将内容用下面的代码覆盖。

```
{
  "log": {
    "access": "",
    "error": "",
    "loglevel": "warning"
  },
  "inbound": {
    "port": 1080,
    "listen": "127.0.0.1",

// SOCKS 地址及代理端口，在浏览器中需配置代理并指向这个端口，此处使用127.0.0.1:1080
    "protocol": "socks",
    "domainOverride": [
      "tls",
      "http"
    ],
    "settings": {
      "auth": "noauth",
      "udp": true,
      "ip": "127.0.0.1",
      "clients": null
    },
    "streamSettings": null
  },
  "outbound": {
    "tag": "agentout",
    "protocol": "vmess",
    "settings": {
      "vnext": [
        {
        
// 服务器地址及端口，需修改为自己的服务器 IP 或域名
          "address": "server", 
          "port": 10086,  

// 此处配置应与服务器配置的信息一致
          "users": [
            {
              "id": "b831381d-6324-4d53-ad4f-8cda48b30811", 
              "alterId": 0,
              "email": "t@t.tt",
              "security": "aes-128-gcm"
            }
          ]
        }
      ],
      "servers": null
    },
    "streamSettings": {
      "network": "tcp",
      "security": "",
      "tlsSettings": null,
      "tcpSettings": null,
      "kcpSettings": null,
      "wsSettings": null,
      "httpSettings": null
    },
    "mux": {
      "enabled": true
    }
  },
  "inboundDetour": null,
  "outboundDetour": [
    {
      "protocol": "freedom",
      "settings": {
        "response": null
      },
      "tag": "direct"
    },
    {
      "protocol": "blackhole",
      "settings": {
        "response": {
          "type": "http"
        }
      },
      "tag": "blockout"
    }
  ],
  "dns": {
    "servers": [
      "8.8.8.8",
      "8.8.4.4",
      "localhost"
    ]
  },
  "routing": {
    "strategy": "rules",
    "settings": {
      "domainStrategy": "IPIfNonMatch",
      "rules": [
        {
          "type": "field",
          "port": null,
          "outboundTag": "direct",
          "ip": [
            "0.0.0.0/8",
            "10.0.0.0/8",
            "100.64.0.0/10",
            "127.0.0.0/8",
            "169.254.0.0/16",
            "172.16.0.0/12",
            "192.0.0.0/24",
            "192.0.2.0/24",
            "192.168.0.0/16",
            "198.18.0.0/15",
            "198.51.100.0/24",
            "203.0.113.0/24",
            "::1/128",
            "fc00::/7",
            "fe80::/10"
          ],
          "domain": null
        }
      ]
    }
  }
}
```

保存后在终端`Ctrl+C`并输入下面的命令，使V2Ray开机自启，并启动V2Ray查看状态。如果显示绿色，证明V2Ray已成功运行。

```
sudo systemctl enable v2ray
sudo systemctl start v2ray
sudo systemctl status v2ray
```

这时V2Ray已经把服务器映射到本地代理127.0.0.1:1080，按照配置代理的方式即可。

```
类型 / Socks5
服务器地址 / 127.0.0.1
端口 / 1080
```

### Shadowsocks

#### Windows

##### shadowsocks-windows

```
https://github.com/shadowsocks/shadowsocks-windows/releases/
```

obfs插件下载链接如下。

```
https://github.com/shadowsocks/simple-obfs/releases
```

将插件obfs-local与Shadowsocks.exe放于同一路径下即可。obfs可以直接在Shadowsocks的服务器编辑页面修改参数，示例如下。

```
obfs=http;obfs-host=www.bing.com
```

#### Mac

##### ShadowsocksX-NG

内置obfs混淆。

```
https://github.com/shadowsocks/ShadowsocksX-NG/releases/
```

#### Android

可在Google Play上下载Shadowsocks应用，同时下载obfs插件Simple Obfuscation。

也可到以下链接下载。

##### shadowsocks-android

```
https://github.com/shadowsocks/shadowsocks-android/releases/
```

#### iOS

##### Shadowsocks

越狱设备可安装Shadowsocks插件。自带Bigboss源有该插件。

#### Linux

```
https://github.com/shadowsocks/shadowsocks-qt5/
```

### Trojan

#### Windows

##### V2RayN

与V2Ray的Windows客户端相同。

#### Android

##### igniter

```
https://github.com/trojan-gfw/igniter/releases/
```


### WireGuard

可使用TunSafe。注意该软件为全局代理软件。

打开以下链接并下载`Standalone TunSafe-TAP Installer 9.21.2`和`TunSafe 1.4 Standalone Zip for 64-bit systems`。

```
https://tunsafe.com/download
```

下载完成后需要获取配置文件。打开软件，选择File-Browse in Explorer，打开配置文件并选择对应节点即可。

### iOS端说明

未越狱iOS端没有各协议的官方客户端。常用的第三方客户端有Shadowrocket、Quantumult和Kitsunebi等，均需在美区App Store下载。

#### Kitsunebi

支持V2Ray（WS/TCP）、Shadowsocks。

打开Kitsunebi，选择服务器，点击右上方+号并选择扫二维码。然后把操作模式改到`Rule`，传出代理选择刚才添加的服务器。完成设置后，在状态页开启VPN开关即可。

#### Shadowrocket

支持V2Ray（WS/TCP）、Shadowsocks、Trojan。支持http和tls两种模式的obfs混淆。

#### Quantumult

支持V2Ray（WS/TCP）、Shadowsocks。

### 全平台程序

#### Clash

Clash可用于多个平台。各平台之间的配置文件通用。

##### 下载

###### Mac

可用ClashX和ClashX Pro。ClashX Pro相比ClashX增加了增强模式，可通过以下链接下载。

```
// ClashX
https://github.com/yichengchen/clashX/releases/

// ClashX Pro
https://install.appcenter.ms/users/clashx/apps/clashx-pro/distribution_groups/public
```

###### Android

```
https://github.com/Kr328/ClashForAndroid
```

###### Windows

```
https://github.com/Fndroid/clash_for_windows_pkg/
```

##### 配置

ClashX通过配置文件配置订阅、策略组和分流。打开后点击配置-托管配置-管理，添加远程配置文件，即可远程下载。

将配置文件保存为yaml后，打开ClashX的配置-打开本地配置文件夹，复制配置文件到此目录，然后在ClashX的配置中选择文件。

完成配置导入后，将出站模式选为规则判断，并勾选`设置为系统代理`即可使用。

配置文件中的通配符含义如下。可用`' '`包住整个通配符语句，以避免语法冲突。

| 通配符 |         含义         |                                                       示例                                                       |
|--------|----------------------|------------------------------------------------------------------------------------------------------------------|
| *      | 仅匹配第一层         | *.ipv6.microsoft.com可匹配win1901.ipv6.microsoft.com，不可匹配foo.win1901.ipv6.microsoft.com和ipv6.microsoft.com |
| .      | 匹配多层（不包括零） | .ipv6.microsoft.com可匹配win1901.ipv6.microsoft.com和foo.win1901.ipv6.microsoft.com，不可匹配ipv6.microsoft.com  |
| +      | 匹配多层（包括零）   | +.ipv6.microsoft.com可匹配win1901.ipv6.microsoft.com，和foo.win1901.ipv6.microsoft.com，以及ipv6.microsoft.com   |

###### 端口

```
# HTTP(S)代理端口
port: 7890

# SOCKS5代理端口
socks-port: 7891

# Linux和macOS的redir代理端口
# redir-port: 7892

# Linux的redir代理端口
# tproxy-port: 7893

# HTTP(S)与SOCKS5混合代理端口
# mixed-port: 7890
```

###### 模式

可为Rule（规则）、Global（全局代理）、Direct（全局直连）。

```
mode: Rule
```

###### 日志输出级别

默认级别silent，即不输出任何内容。可选择silent/info/warning/error/debug，共五个级别，级别越高表示日志输出量越大。

```
log-level: silent
```

###### 外部控制

通过RESTful API实现外部网站控制Clash。

```
# clash的RESTful API
external-controller: 127.0.0.1:9090

# 将静态网页资源（如clash-dashboard）放置在一个目录中，clash将会服务于${API}/ui
# 参数应填写配置目录的相对路径或绝对路径
# external-ui: folder

# RESTful API的口令
# secret: ""
```

###### 接口

```
interface-name: en0
```

###### 实验性功能

```
experimental:
  ignore-resolve-fail: true # 忽略 DNS 解析失败，默认值为 true
```

###### DNS

当访问一个域名时，nameserver与fallback列表内的所有服务器并发请求，得到域名对应的IP地址。若解析结果中IP地址属于国外，则Clash将选择fallback列表内解析最快的结果，否则将选取nameserver列表内解析最快的结果。

注意，nameserver应尽量选择中国大陆的DNS，fallback应尽量选择无污染的国外DNS，且数量不应超过四个。

增强模式enhanced-mode可为redir-host或fake-ip。在redir-host模式下，当客户端发起DNS请求时，Clash将需要解析的域名发送到每一个上游DNS服务器，返回最快的一个结果。在fake-ip模式下，当客户端发起DNS请求时，Clash直接返回一个虚假的IP。

示例如下。

```
dns:
  enable: true
  ipv6: false

# listen: 0.0.0.0:53
# enhanced-mode: redir-host

# fake-ip-range: 198.18.0.1/16

# fake-ip白名单列表
# fake-ip-filter: 
#   - '*.lan'
#   - localhost.ptlogin2.qq.com

  nameserver:
    - 1.2.4.8
    - 114.114.114.114
    - 223.5.5.5
    - tls://13800000000.rubyfish.cn:853
  # - https://13800000000.rubyfish.cn/

  fallback: 
    - tls://13800000000.rubyfish.cn:853
    - tls://1.0.0.1:853
    - tls://dns.google:853
  # - https://13800000000.rubyfish.cn/
  # - https://cloudflare-dns.com/dns-query
  # - https://dns.google/dns-query

  fallback-filter:
    geoip: true
    ipcidr: # 在这个网段内的IP地址会被考虑为被污染的IP
      - 240.0.0.0/4
```

###### Host

指定Hosts条目，支持通配符，注意静态域名比通配域名具有更高的优先级。Hosts在fake-ip模式下不生效。

```
hosts:
  '*.clash.dev': 127.0.0.1
  'alpha.clash.dev': '::1'
```

###### 添加节点

订阅节点在配置文件中的proxies部分定义，格式如下。注意IPV6地址需要用`[]`框住。

```
proxies:
  # Shadowsocks
  # 支持的加密方式如下
  # aes-128-gcm aes-192-gcm aes-256-gcm
  # aes-128-cfb aes-192-cfb aes-256-cfb
  # aes-128-ctr aes-192-ctr aes-256-ctr
  # rc4-md5 chacha20-ietf xchacha20
  # chacha20-ietf-poly1305 xchacha20-ietf-poly1305
  - name: "ss1"
    type: ss
    server: server
    port: 443
    cipher: chacha20-ietf-poly1305
    password: "password"
    # udp: true

  - name: "ss2"
    type: ss
    server: server
    port: 443
    cipher: chacha20-ietf-poly1305
    password: "password"
    plugin: obfs
    plugin-opts:
      mode: tls # or http
      # host: bing.com

  - name: "ss3"
    type: ss
    server: server
    port: 443
    cipher: chacha20-ietf-poly1305
    password: "password"
    plugin: v2ray-plugin
    plugin-opts:
      mode: websocket # no QUIC now
      # tls: true # wss
      # skip-cert-verify: true
      # host: bing.com
      # path: "/"
      # mux: true
      # headers:
      #   custom: value

  # Vmess
  # 支持的加密方式如下
  # auto aes-128-gcm chacha20-poly1305 none
  - name: "vmess"
    type: vmess
    server: server
    port: 443
    uuid: uuid
    alterId: 32
    cipher: auto
    # udp: true
    # tls: true
    # skip-cert-verify: true
    # servername: example.com # priority over wss host
    # network: ws
    # ws-path: /path
    # ws-headers:
    #   Host: v2ray.com

  - name: "vmess-h2"
    type: vmess
    server: server
    port: 443
    uuid: uuid
    alterId: 32
    cipher: auto
    network: h2
    tls: true
    h2-opts:
      host:
        - http.example.com
        - http-alt.example.com
      path: /
  
  - name: "vmess-http"
    type: vmess
    server: server
    port: 443
    uuid: uuid
    alterId: 32
    cipher: auto
    # udp: true
    # network: http
    # http-opts:
    #   # method: "GET"
    #   # path:
    #   #   - '/'
    #   #   - '/video'
    #   # headers:
    #   #   Connection:
    #   #     - keep-alive

  # Socks5
  - name: "socks"
    type: socks5
    server: server
    port: 443
    # username: username
    # password: password
    # tls: true
    # skip-cert-verify: true
    # udp: true

  # Http
  - name: "http"
    type: http
    server: server
    port: 443
    # username: username
    # password: password
    # tls: true # https
    # skip-cert-verify: true
    # sni: custom.com

  # Snell
  - name: "snell"
    type: snell
    server: server
    port: 44046
    psk: yourpsk
    # version: 2
    # obfs-opts:
      # mode: http # or tls
      # host: bing.com

  # Trojan
  - name: "trojan"
    type: trojan
    server: server
    port: 443
    password: yourpsk
    # udp: true
    # sni: example.com # aka server name
    # alpn:
    #   - h2
    #   - http/1.1
    # skip-cert-verify: true

  # ShadowsocksR
  # 支持的加密方式与Shadowsocks完全一致
  # 支持的obfs混淆如下
  # plain http_simple http_post
  # random_head tls1.2_ticket_auth tls1.2_ticket_fastauth
  # 支持的加密协议如下
  # origin auth_sha1_v4 auth_aes128_md5
  # auth_aes128_sha1 auth_chain_a auth_chain_b  
  - name: "ssr"
    type: ssr
    server: server
    port: 443
    cipher: chacha20-ietf
    password: "password"
    obfs: tls1.2_ticket_auth
    protocol: auth_sha1_v4
    # obfs-param: domain.tld
    # protocol-param: "#"
    # udp: true
```

也可通过订阅链接转换的方式获取，具体见翻墙进阶部分。

###### 订阅节点

订阅节点在配置文件中的proxy-provider部分定义，参数如下。其中url对应的文件内容即为配置文件中的proxies部分，与上面一致。path为该文件下载到本地后的存放位置。

|     参数     |                              说明                             |
|--------------|---------------------------------------------------------------|
| type         | http/file，分别为远程链接和本地文件                           |
| path         | 文件保存路径，可以为绝对路径，./相当于Clash配置文件所在文件夹 |
| url          | 类型为http时，填写的URL链接                                   |
| interval     | 类型为http时，订阅自动更新周期                                |
| health-check | 健康检查选项                                                  |

示例如下。

```
proxy-providers:
  provider:
    type: http
    url: https://example.com/example.yaml
    interval: 3600
    path: ./proxyset/provider.yaml
    health-check:
      enable: true
      interval: 600
      # lazy: true
      url: http://www.gstatic.com/generate_204
```

###### 策略组

策略组在配置文件中的proxy-groups部分定义。

若需要将订阅节点写到策略组中，则将订阅名称写到use一栏下。若需要将单个节点写到策略组中，则将节点名称写到proxies一栏下。

策略组类型如下。

|     名称     |   类型   |
|--------------|----------|
| select       | 手动选择 |
| url-test     | 延迟测试 |
| fallback     | 健康检查 |
| load-balance | 负载均衡 |
| relay        | 代理链   |

Clash内置的策略组为DIRECT和REJECT，但不包含PROXY。示例如下。

```
proxy-groups:
  - name: 手动选择
    type: select
    use:
      - provider
    proxies:
      - AUTO
      - DIRECT
      - example-proxy

  - name: 延迟测试
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    use:
      - provider

  - name: 健康检测
    type: fallback
    url: http://www.gstatic.com/generate_204
    interval: 300
    use:
      - provider

  - name: 负载均衡
    type: load-balance
    url: http://www.gstatic.com/generate_204
    interval: 300
    use:
      - provider

  # 流量流向为clash<->http<->vmess<->ss1<->ss2<->Internet
  - name: "relay"
    type: relay
    proxies:
      - select
      - vmess1
      - ss1
      - ss2
```

###### 规则集

规则集在配置文件中的rule-providers部分定义，示例如下，参数与订阅节点一致。

```
rule-providers:
  Advertising:
    type: http
    behavior: classical
    path: ./RuleSet/Guard/Advertising.yaml
    url: https://cdn.jsdelivr.net/gh/DivineEngine/Profiles@master/Clash/RuleSet/Guard/Advertising.yaml
    interval: 86400
```

###### 规则

规则在配置文件中的rules部分定义，类型如下。

|    匹配类型    |           匹配名称          |
|----------------|-----------------------------|
| DOMAIN-SUFFIX  | 域名后缀匹配                |
| DOMAIN         | 域名匹配                    |
| DOMAIN-KEYWORD | 域名关键字匹配              |
| IP-CIDR        | IPv4段匹配                  |
| IP-CIDR        | IPv6段匹配                  |
| SRC-IP-CIDR    | 源IP段匹配                  |
| GEOIP          | GEOIP数据库（国家代码）匹配 |
| DST-PORT       | 目标端口匹配                |
| SRC-PORT       | 源端口匹配                  |
| MATCH          | 兜底匹配                    |
| RULE-SET       | 规则集匹配                  |

示例如下，注意no-resolve选项表示跳过具有域请求的规则。应当在该部分指定规则集与策略组的关系，即该规则集使用哪个策略组。

```
rules:
  - DOMAIN-SUFFIX,google.com,auto
  - DOMAIN-KEYWORD,google,auto
  - DOMAIN,ad.com,REJECT
  - SRC-IP-CIDR,192.168.1.201/32,DIRECT
  - IP-CIDR,127.0.0.0/8,DIRECT
  - IP-CIDR,32.0.0.0/8,DIRECT,no-resolve
  - IP-CIDR6,2620:0:2d0:200::7/32,auto
  - GEOIP,CN,DIRECT
  - DST-PORT,80,DIRECT
  - SRC-PORT,7777,DIRECT
  - RULE-SET,providername,proxy
  - MATCH,auto
```

###### 绕过系统代理

绕过系统代理在配置文件中的cfw-bypass部分定义，支持通配符，示例如下。最后一行对应系统中`请勿将代理服务器用于本地(Intranet)地址`选项，请确保此项在最底部。

```
cfw-bypass:
  ... # 原有字段不用删除
  - 'music.163.com' # 网易云域名1
  - '*.music.126.net' # 网易云域名2
```

###### 脚本

通过脚本编写配置文件，可实现代码控制的功能。注意需要将mode修改为Script，示例如下。

```
mode: Script

script:
  # test规则集使用DIRECT策略组
  code: |
    def main(ctx, metadata):
      if ctx.rule_providers["test"].match(metadata):
        return "DIRECT"
  
  # test和test2规则集使用DIRECT策略组
  code: |
    def main(ctx, metadata):
      list = ['test', 'test2']
      
      for name in list:
        if ctx.rule_providers[name].match(metadata):
          ctx.log('[Script] matched %s' % name)
          return "DIRECT"

  # GEOIP数据库匹配
  code: |
    def main(ctx, metadata):
      code = ctx.geo_ip(ip)
      if code == 'CN' or code == 'LAN':
        return "DIRECT"

  # DNS解析错误时使用DIRECT策略组
  code: |
    def main(ctx, metadata): 
      ip = ctx.resolve_ip(metadata["host"])
      if ip == "":
        return "DIRECT"

  # 源端口为3000-4000
  code: |
    def main(ctx, metadata): 
      srcport = metadata["src_port"]
      if (3000 < srcport and srcport < 4000):
        return "DIRECT"

  # 关键词匹配
  code: |
    def main(ctx, metadata): 
    rejectkeywordlist = ["adservice"]
    for rejectkeyword in rejectkeywordlist:
      if rejectkeyword in metadata["host"]:
        ctx.log('[Script] matched keyword %s use reject' % rejectkeyword)
        return "REJECT"
    
  # 时间参数
  code: |
    def main(ctx, metadata): 
      now = time.now()
      if metadata["src_ip"] == "ip" and now.hour => 18 and now.hour <= 22:
        return "REJECT"

      return "DIRECT"
```

ctx可用方法如下。

|                函数名                 | 参数类型 | 返回值类型 |     作用    |
|---------------------------------------|----------|------------|-------------|
| resolve_ip(host)                      | string   | string     | DNS解析匹配 |
| resolve_process_name(metadata)        | Metadata | string     | 进程名匹配  |
| geoip(ip)                             | string   | string     | GEOIP匹配   |
| log(log)                              | string   | void       | 日志        |
| proxy_providers[name].match(metadata) | string   | boolean    | 策略组匹配  |
| rule_providers[name].match(metadata)  | string   | boolean    | 规则集匹配  |

metadata可用参数如下。

|  函数名  |       作用       |
|----------|------------------|
| type     | 连接方法类型     |
| network  | 网络类型         |
| host     | 主机名           |
| src_ip   | 请求的客户端IP   |
| src_port | 请求的客户端端口 |
| dst_ip   | 请求的目标IP     |
| dst_port | 请求的目标端口   |

##### 功能

###### 局域网共享

勾选配置-允许局域网连接，即可开启局域网共享。在控制台可查看端口，一般为8090。查看本机IP地址并记录，然后对连接到同一局域网即同一Wi-Fi下的设备设置代理`[IP地址]:8090`，即可使用本机的翻墙代理。

也可通过配置文件设置，示例如下。注意bind-address仅在allow-lan设置为true时生效，支持以下参数类型。

|             参数            |       含义       |
|-----------------------------|------------------|
| "*"                         | 绑定所有IP地址   |
| 192.168.122.11              | 绑定一个IPv4地址 |
| "[aaaa::a8aa:ff:fe09:57d8]" | 绑定一个IPv6地址 |

```
allow-lan: false

# bind-address: "*"

# 本地SOCKS5/HTTP(S)服务认证
# authentication:
#  - "user1:pass1"
#  - "user2:pass2"
```

##### 模式

###### TProxy

实现透明代理。

确保配置文件中没有tun字段。在Linux中需在终端输入以下命令，然后启动Clash即可。

```
# 以下几种模式选择一种即可
## Redir-Host
iptables -t nat -N clash
iptables -t nat -A clash -d 0.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 10.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 127.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 169.254.0.0/16 -j RETURN
iptables -t nat -A clash -d 172.16.0.0/12 -j RETURN
iptables -t nat -A clash -d 192.168.0.0/16 -j RETURN
iptables -t nat -A clash -d 224.0.0.0/4 -j RETURN
iptables -t nat -A clash -d 240.0.0.0/4 -j RETURN
iptables -t nat -A clash -d "$local_ipv4" -j RETURN
iptables -t nat -A clash -p tcp -j REDIRECT --to-port "$proxy_port"
iptables -t nat -I PREROUTING -p tcp -d 8.8.8.8 -j REDIRECT --to-port "$proxy_port"
iptables -t nat -I PREROUTING -p tcp -d 8.8.4.4 -j REDIRECT --to-port "$proxy_port"
iptables -t nat -A PREROUTING -p tcp -j clash

iptables -t nat -N CLASH_DNS
iptables -t nat -F CLASH_DNS 
iptables -t nat -A CLASH_DNS -p udp -j REDIRECT --to-port 1053
iptables -t nat -I OUTPUT -p udp --dport 53 -j CLASH_DNS
iptables -t nat -I PREROUTING -p udp --dport 53 -j REDIRECT --to 1053

## Fake-IP
iptables -t nat -N clash
iptables -t nat -A clash -d 0.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 10.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 127.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 169.254.0.0/16 -j RETURN
iptables -t nat -A clash -d 172.16.0.0/12 -j RETURN
iptables -t nat -A clash -d 192.168.0.0/16 -j RETURN
iptables -t nat -A clash -d 224.0.0.0/4 -j RETURN
iptables -t nat -A clash -d 240.0.0.0/4 -j RETURN
iptables -t nat -A clash -d "$local_ipv4" -j RETURN
iptables -t nat -A clash -p tcp -j REDIRECT --to-port "$proxy_port"
iptables -t nat -I PREROUTING -p tcp -d 8.8.8.8 -j REDIRECT --to-port "$proxy_port"
iptables -t nat -I PREROUTING -p tcp -d 8.8.4.4 -j REDIRECT --to-port "$proxy_port"
iptables -t nat -A PREROUTING -p tcp -j clash
iptables -t nat -A OUTPUT -p tcp -d 198.18.0.0/16 -j REDIRECT --to-port "$proxy_port"
iptables -t nat -N CLASH_DNS
iptables -t nat -F CLASH_DNS 
iptables -t nat -A CLASH_DNS -p udp -j REDIRECT --to-port 1053
iptables -t nat -I OUTPUT -p udp --dport 53 -j CLASH_DNS
iptables -t nat -I PREROUTING -p udp --dport 53 -j REDIRECT --to 1053

## 带有UDP的Redir-Host
# tcp
iptables -t nat -N clash
iptables -t nat -A clash -d 0.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 10.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 127.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 169.254.0.0/16 -j RETURN
iptables -t nat -A clash -d 172.16.0.0/12 -j RETURN
iptables -t nat -A clash -d 192.168.0.0/16 -j RETURN
iptables -t nat -A clash -d 224.0.0.0/4 -j RETURN
iptables -t nat -A clash -d 240.0.0.0/4 -j RETURN
iptables -t nat -A clash -d "$local_ipv4" -j RETURN
iptables -t nat -A clash -p tcp -j REDIRECT --to-port "$proxy_port"
iptables -t nat -I PREROUTING -p tcp -d 8.8.8.8 -j REDIRECT --to-port "$proxy_port"
iptables -t nat -I PREROUTING -p tcp -d 8.8.4.4 -j REDIRECT --to-port "$proxy_port"
iptables -t nat -A PREROUTING -p tcp -j clash

# udp
ip rule add fwmark 1 table 100
ip route add local default dev lo table 100
iptables -t mangle -N clash
iptables -t mangle -A clash -d 0.0.0.0/8 -j RETURN
iptables -t mangle -A clash -d 10.0.0.0/8 -j RETURN
iptables -t mangle -A clash -d 127.0.0.0/8 -j RETURN
iptables -t mangle -A clash -d 169.254.0.0/16 -j RETURN
iptables -t mangle -A clash -d 172.16.0.0/12 -j RETURN
iptables -t mangle -A clash -d 192.168.0.0/16 -j RETURN
iptables -t mangle -A clash -d 224.0.0.0/4 -j RETURN
iptables -t mangle -A clash -d 240.0.0.0/4 -j RETURN
iptables -t mangle -A clash -d "$local_ipv4" -j RETURN
iptables -t mangle -A clash -p udp -j TPROXY --on-port "$proxy_port" --tproxy-mark 1
iptables -t mangle -A PREROUTING -p udp -j clash
iptables -t nat -N CLASH_DNS
iptables -t nat -F CLASH_DNS 
iptables -t nat -A CLASH_DNS -p udp -j REDIRECT --to-port 1053
iptables -t nat -I OUTPUT -p udp --dport 53 -j CLASH_DNS
iptables -t nat -I PREROUTING -p udp --dport 53 -j REDIRECT --to 1053

## 带有UDP的Fake-IP
#tcp
iptables -t nat -N clash
iptables -t nat -A clash -d 0.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 10.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 127.0.0.0/8 -j RETURN
iptables -t nat -A clash -d 169.254.0.0/16 -j RETURN
iptables -t nat -A clash -d 172.16.0.0/12 -j RETURN
iptables -t nat -A clash -d 192.168.0.0/16 -j RETURN
iptables -t nat -A clash -d 224.0.0.0/4 -j RETURN
iptables -t nat -A clash -d 240.0.0.0/4 -j RETURN
iptables -t nat -A clash -d "$local_ipv4" -j RETURN
iptables -t nat -A clash -p tcp -j REDIRECT --to-port "$proxy_port"
iptables -t nat -I PREROUTING -p tcp -d 8.8.8.8 -j REDIRECT --to-port "$proxy_port"
iptables -t nat -I PREROUTING -p tcp -d 8.8.4.4 -j REDIRECT --to-port "$proxy_port"
iptables -t nat -A PREROUTING -p tcp -j clash
iptables -t nat -A OUTPUT -p tcp -d 198.18.0.0/16 -j REDIRECT --to-port "$proxy_port"

#udp
ip rule add fwmark 1 table 100
ip route add local default dev lo table 100
iptables -t mangle -N clash
iptables -t mangle -A clash -d 0.0.0.0/8 -j RETURN
iptables -t mangle -A clash -d 10.0.0.0/8 -j RETURN
iptables -t mangle -A clash -d 127.0.0.0/8 -j RETURN
iptables -t mangle -A clash -d 169.254.0.0/16 -j RETURN
iptables -t mangle -A clash -d 172.16.0.0/12 -j RETURN
iptables -t mangle -A clash -d 192.168.0.0/16 -j RETURN
iptables -t mangle -A clash -d 224.0.0.0/4 -j RETURN
iptables -t mangle -A clash -d 240.0.0.0/4 -j RETURN
iptables -t mangle -A clash -d "$local_ipv4" -j RETURN
iptables -t mangle -A clash -p udp -j TPROXY --on-port "$proxy_port" --tproxy-mark 1
iptables -t mangle -A OUTPUT -p udp -d 198.18.0.0/16 -j MARK --set-mark 1
iptables -t mangle -A PREROUTING -p udp -j clash
iptables -t nat -N CLASH_DNS
iptables -t nat -F CLASH_DNS 
iptables -t nat -A CLASH_DNS -p udp -j REDIRECT --to-port 1053
iptables -t nat -I OUTPUT -p udp --dport 53 -j CLASH_DNS
iptables -t nat -I PREROUTING -p udp --dport 53 -j REDIRECT --to 1053
```

###### TUN

使Clash接管所有程序，包括在普通情况下不遵守系统代理设置的程序。

对于Mac/Linux/OpenWRT/nftables，使用以下配置。stack可选择system或gvisor，以确定使用哪一个堆栈。dns-hijack用于DNS劫持，以在fake-ip模式下返回假IP。

```
tun:
  enable: true
  stack: system
  macOS-auto-route: true
  macOS-auto-detect-interface: true
  dns-hijack:
    - tcp://8.8.8.8:53
```

然后在终端输入以下命令。Mac如下。

```
sudo route -n add -net 1 198.18.0.1
sudo route -n add -net 2/7 198.18.0.1
sudo route -n add -net 4/6 198.18.0.1
sudo route -n add -net 8/5 198.18.0.1
sudo route -n add -net 16/4 198.18.0.1
sudo route -n add -net 32/3 198.18.0.1
sudo route -n add -net 64/2 198.18.0.1
sudo route -n add -net 128.0/1 198.18.0.1
```

Linux/OpenWRT如下。

```
# Based on https://github.com/Kr328/kr328-clash-setup-scripts/blob/master/setup-clash-tun.sh
ipset create localnetwork hash:net
ipset add localnetwork 127.0.0.0/8
ipset add localnetwork 10.0.0.0/8
ipset add localnetwork 169.254.0.0/16
ipset add localnetwork 192.168.0.0/16
ipset add localnetwork 224.0.0.0/4
ipset add localnetwork 240.0.0.0/4
ipset add localnetwork 172.16.0.0/12

ip tuntap add user root mode tun utun0
ip link set utun0 up

ip route replace default dev utun0 table 0x162

ip rule add fwmark 0x162 lookup 0x162

iptables -t mangle -N CLASH
iptables -t mangle -F CLASH
iptables -t mangle -A CLASH -p tcp --dport 53 -j MARK --set-mark 0x162
iptables -t mangle -A CLASH -p udp --dport 53 -j MARK --set-mark 0x162
iptables -t mangle -A CLASH -m addrtype --dst-type BROADCAST -j RETURN
iptables -t mangle -A CLASH -m set --match-set localnetwork dst -j RETURN
iptables -t mangle -A CLASH -d 198.18.0.0/16 -j MARK --set-mark 0x162
iptables -t mangle -A CLASH -j MARK --set-mark 0x162

iptables -t mangle -I OUTPUT -j CLASH
iptables -t mangle -I PREROUTING -m set ! --match-set localnetwork dst -j MARK --set-mark 0x162


sysctl -w net/ipv4/ip_forward=1
sysctl -w net.ipv4.conf.utun0.rp_filter=0
```

nftables如下。

```
# Based on https://github.com/Kr328/kr328-clash-setup-scripts/blob/master/setup-clash-tun.sh
# Make sure your clash DNS listen at 1053, otherwise you should modify some part
ip route replace default dev utun table 114

ip rule del fwmark 114514 lookup 114
ip rule add fwmark 114514 lookup 114

nft -f - << EOF
define LOCAL_SUBNET = {127.0.0.0/8, 224.0.0.0/4, 192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12, 169.254.0.0/16, 240.0.0.0/4}
define TUN_DEVICE = utun
define FORWARD_DNS_REDIRECT = {127.0.0.1:1053}
table inet clash
flush table inet clash
table inet clash {
    chain local {
        type route hook output priority 0; policy accept;
        
        ip protocol != { tcp, udp } accept
        
        ip daddr \$LOCAL_SUBNET accept
        
        ct state new ct mark set 114514
        ct mark 114514 mark set 114514
    }
    
    chain forward {
        type filter hook prerouting priority 0; policy accept;
        
        ip protocol != { tcp, udp } accept
    
        iif utun accept
        ip daddr \$LOCAL_SUBNET accept
        
        mark set 114514
    }
    
    chain local-dns-redirect {
        type nat hook output priority 0; policy accept;
        
        ip protocol != { tcp, udp } accept
        
        
        udp dport 53 dnat ip to $FORWARD_DNS_REDIRECT
        tcp dport 53 dnat ip to $FORWARD_DNS_REDIRECT
    }
    
    chain forward-dns-redirect {
        type nat hook prerouting priority 0; policy accept;
        
        ip protocol != { tcp, udp } accept
        
        udp dport 53 dnat ip to $FORWARD_DNS_REDIRECT
        tcp dport 53 dnat ip to $FORWARD_DNS_REDIRECT
    }
}

EOF

sysctl -w net/ipv4/ip_forward=1
```

对于Windows，需要使用以下配置。

```
tun:
  enable: true
  stack: gvisor # only gvisor
  dns-hijack:
    - 198.18.0.2:53
  macOS-auto-route: true
  macOS-auto-detect-interface: true
```

还需要下载以下程序，将wintun.dll复制到Clash目录。

```
https://www.wintun.net/
```

#### Surge

##### 下载

###### Mac

```
https://www.nssurge.com/
```

将以下内容保存为surge.sh，放置位置与Surge应用程序同级，用终端运行`bash [surge.sh路径]`即可。注意，对于最新版Surge，破解后过一段时间会闪退，因为该脚本的原理是修改时间，最新版的Surge会自动检测时间。断网后可保持Surge不闪退，但这也就无法正常使用。

```
#!/usr/bin/env bash

cd $(dirname "$0")

read -sp "Password: " pwd
echo
rm -rf ~/Library/Application Support/com.nssurge.surge-*
echo "${pwd}" | sudo -S date 010110002018
nohup ./Surge.app/Contents/MacOS/Surge &
sleep 20
echo "${pwd}" | sudo -S sntp -sS time.apple.com.
```

##### 基本配置

开启系统代理，出站模式为规则判断。

##### 配置文件

可以直接套用iOS的surge配置文件。

##### 模块

若导入iOS端Surge的模块到Mac端时报错，则检查模块是否有以下语句，有则删除即可。

```
#!system=ios
```

##### 远程控制

iOS的Surge可控制Mac的Surge。在Mac修改Surge的配置文件，添加以下行，其中password可以修改为其它内容。

```
external-controller-access = password@0.0.0.0:6155
```

打开iOS端的Surge，添加远程控制器，IP填写Mac端的IP，端口为6155，密码为password部分的内容，配置完成即可。

##### 第三方工具

常用的第三方工具有YASD和xurge。以YASD为例说明其使用方法。

打开以下网站，点击添加新主机，参数为Surge配置文件的[general]模块下的http-api，一般为0.0.0.0或127.0.0.1，保存即可使用。

```
http://yasd.nerdynerd.org/home
```

##### 软路由

###### 通过代理

Surge开启时，会自动开启http和socks5代理，实现局域网共享。在启动日志中可看到`SOCKS5 代理服务已启动，监听在 127.0.0.1，端口号 6153`和`HTTP 代理服务已启动，监听在 127.0.0.1，端口号 6152`的记录。

对于同一网络下的设备，若该设备需要翻墙，则在该设备上手动设置代理即可，其中服务器IP为安装有Surge的电脑IP，端口在记录中可查询到。

###### 通过增强模式

即网关模式、协议转换器，可以实现网络间的相互连接。

点击系统偏好设置-网络，选择当前使用的网络，点击高级-TCP/IP，在配置IPV4下选择手动，然后即可开启Surge的增强模式。

对于同一网络下的设备，若该设备需要翻墙，则在该设备上手动设置代理，其中服务器IP填写同网段内任意一个设备的IP，子网掩码保持不变（255.255.255.0），路由器地址/网关地址为
Mac的IP，DNS为Mac中网络偏好设置-DNS服务器下的记录，该DNS由surge创建，一般为198.18.0.2。

###### 通过DHCP服务器模式

该方法不需要配置代理设备，只需在Mac端配置即可。但配置出错会导致连接该网络的所有设备瘫痪。

要启用该功能，需要关闭现有的DHCP，因为DHCP将由Surge接管。DHCP通常由路由器提供，需要参考路由器的配置页面。

在Surge的配置中，需手动配置至少一个有效的DNS服务器，即Surge DNS不能全部指向内网。

本设备不应断电或断网，否则将影响其余所有设备，因此Surge会强制禁止电脑休眠。本设备应使用有线连接网络，无线网络可能导致严重的性能问题。

完成如上配置后，即可在Surge中点击设置-DHCP服务器，开启后记录详细网络参数以防配置错误。回到设备页，可以看到所有连接到该网络的设备。对于要设置翻墙的设备，右键点击使用surge作为网关即可。

##### 常见问题

###### 与AdGuard冲突

关闭surge系统代理。保证http和socks5代理打开的情况下，打开增强模式。在adguard的高级设置中，将adguard的上游代理（upstream.proxy）设置为surge的代理端（socks5://locaalhost:6153），不要打开udp即可。

也可通过另一种方式。进入Adguard高级设置并点击恢复默认，首选项关闭自动过滤应用程序流量，打开http代理。进入系统网络偏好设置，点击高级-代理-网页代理（HTTP），填入adguard的http代理内容，然后正常启动Surge即可。

#### Trojan-Qt5

```
https://github.com/charlieethan/Trojan-Qt5/releases
```

#### QV2Ray

打开以下网站下载QV2Ray软件以及SSR/Trojan插件。

```
https://github.com/Qv2ray/Qv2ray/releases
https://qv2ray.github.io/
```

打开QV2Ray，点击`插件`，选择`打开本地插件目录`，将下载的插件复制到此目录，重启应用即可加载插件。然后打开以下网站，下载V2Ray核心。在QV2Ray的安装目录下新建一个空文件夹，名称可以任意取，然后将解压后的V2Ray核心文件拖入其中。

```
https://github.com/v2ray/v2ray-core/releases
```

打开QV2Ray，点击首选项-内核设置，将`核心可执行文件路径`设置刚才所新建的文件夹下的v2ray.exe目录（Mac为v2ray文件），`V2ray资源目录`设置为刚才所新建的文件夹下的目录，然后点击`核心验证`确认配置完成。

#### Surfboard

适用于Android，兼容Surge配置文件。

```
https://manual.getsurfboard.com/
```

## 二级代理

二级代理即为在数据从服务端传回客户端时并不直接传回，而是通过另一个服务器中转。

### Shadowsocks

准备好客户端及其副本。打开客户端并连接中转节点，然后打开另一个客户端，将会提示端口被占用，因为默认端口均为1080。因此修改本客户端的本地端口为1081或其它值，然后设置该客户端连接服务器节点，代理模式选择全局模式即可。


# 翻墙进阶

## 可用性检查

### IP连通性

可通过以下网站测试IP的连通性。

```
https://www.vps234.com/ipchecker/
http://ping.pe/
https://torch.njs.app/
https://www.ipaddress.com/
https://www.ip-adress.com/
https://whoer.net/zh
https://tuna.moe/help/dns/
```

如果IP不通，则需要更换IP。如果IP全通，但翻墙连不上，则一般为端口被封，更换端口即可。

## 订阅链接

### 格式

去掉前缀并经base64解码后，可得到不同协议的节点配置格式。

#### Shadowsocks

```
ss://method:password@server:port
```

#### ShadowsocksR

```
ssr://ip:port:protocol:method:blending:password/?remarks=othertext

// 其余形式如下，其中obfsparam、protoparam、group、remarks可选
ssr://159.65.1.189:5252:auth_sha1_v4:rc4-md5:http_simple:NTJzc3IubmV0/?obfsparam=&protoparam=&group=d3d3LnNzcnNoYXJlLmNvbQ&remarks=RE1fTm9kZQ
```

#### V2Ray

```
{
  "ps": "别名",
  "add": "ip地址",
  "port": "端口",
  "id": "uuid",
  "aid": "alterid",
  "net": "传输协议",
  "type": "伪装类型",
  "host": " http header参数",
  "tls": "底层传输安全"
}
```

### 转换

#### 边缘转换

网址如下。

```
https://bianyuan.xyz/
```

以生成适用于Quantumult的订阅为例，`订阅链接`填写`vmess://`开头的链接或机场订阅，远程配置选择`No-Urltest`，并勾选`输出为Node List`，得到链接。

可直接将得到的链接作为订阅URL，也可在浏览器打开该链接下载文件得到文件内容。

#### QuantumultX-Surge-API

网页版地址如下。

```
https://dove.589669.xyz/web
```

仓库如下。

```
https://github.com/KOP-XIAO/QuantumultX-Surge-API
```

#### Surgio

```
https://surgio.js.org/
https://github.com/surgioproject/surgio
```

#### ConfigConverters

```
https://github.com/ImSingee/ConfigConverter
```

#### subconverter

```
https://github.com/tindy2013/subconverter
```

### 制作

为防止服务器配置更改导致节点失效，可通过制作订阅的方式。更新订阅即可更新服务器配置，无需逐个修改。

#### 通用方法

在Github上新建仓库，以用于存放配置文件。

以V2Ray节点为例，由于大部分V2Ray客户端的分享链接采用V2RayN标准，因此需先在V2RayN上完成各服务器的配置。选中所有需要配置成订阅的服务器，右键选择`批量导出分享URL至剪贴板`，然后打开以下网站，将刚才复制的内容编码成BASE64。

```
https://tool.oschina.net/encrypt?type=3
```

编码完成后回到Github并新建文件，文件名任意，后缀为txt。将编码后的内容复制到文本区域，保存后点击`Commit changes`，成功后点击`Raw`，复制得到的网址，即为所得订阅链接。本链接可用于大部分客户端。

#### 特殊处理

##### Quantumult

Quantumult无法使用以上链接，可通过订阅链接转换中的方式完成转换。需要注意，通过一键生成配置网站所生成的文件已经过处理，但每一个订阅都使用同一个group名称，从而会导致多个订阅链接更新时服务器的覆盖。因此需要手动base64解码，解码一次后得到服务器列表，去掉`vmess://`前缀后再次解码得到服务器信息，更改group后重新编码，即得到可用链接。

也可手动完成本操作。在已经配置好服务器的Quantumult上获取服务器的分享链接，删除`vmess://`前缀后放到上述BASE64网站进行解码，解码内容应如下所示。

```
[名称] = vmess, [地址], [端口], [加密方式], [UUID], [选项]
```

对于Vmess节点，在后面加上`, group=[名称]`，例子如下所示。注意，对于vmess节点，每一个订阅需要有唯一的group名称，否则在订阅更新时会被覆盖。若为Shadowsocks节点，则无需进行操作。

```
example = vmess, 1.2.3.4, 1234, aes-128-cfb, "12345678-8765-4321-1234-123456789abc", over-tls=false, certificate=1, group=TCP
```

重新编码为BASE64并添加`vmess://`前缀，得到一个可用的链接。其余服务器同理，从而得到一系列可用的链接，将这些链接进行一次BASE64编码即可。

如果手动编辑后的连接不可用，则尝试在后面加一个空格，再编码为BASE64并添加`vmess://`前缀。

对于Shadowsocks节点，则按照以下格式编写ss可用链接，将所有可用链接进行一次BASE64编码即可。

```
ss://[base64编码后的method:password]@[地址]:[端口]&group=[组名]#[名称]
```

##### Surge

Surge订阅格式如下所示，注意不能进行base64解码。

```
// Vmess
[名称] = vmess, [地址], [端口], username=[用户名], ws=true/false, (ws-path=[路径], ws-headers=[Host:地址],) tls=true/false

// Shadowsocks
[名称] = ss, [地址], [端口]&group=[组名], encrypt-method=[加密方式], password=[密码], tfo=true/false, udp-relay=true/false
```

#### 链接发布

##### 放到Github上

登录Github并新建一个Repository，类型为Public，需勾选`Initialize this repository with a README`，完成创建。删除README以减少被搜索出来的几率。

在仓库内新建一个文本文档并编辑，将上面制作好的订阅内容复制进去后保存。然后点击`Raw`，跳转到的链接即为订阅地址。

##### 放到自有服务器上

用SSH连接到服务器，新建一个php文档，内容如下。

```
<?php
$str = file_get_contents("./res.txt", "r") or die("Unable to open file!"); echo base64_encode($str);
?>
```

新建名称为`res.txt`的文本文件，将上面制作好的订阅内容复制进去即可。php文档的地址即为订阅地址。




## 反代网站

反代通过访问本网站，而本网站再访问其它网站的方式实现翻墙。

### 现成网站

```
https://search.snopyta.org/
http://webproxy.to/
https://weboas.is/
https://www.croxyproxy.com/
```

### 自行搭建

#### jsproxy

仓库如下。

```
https://github.com/EtherDream/jsproxy
```

##### 搭建到Cloudflare Worker

打开第一个网页，完成注册后登录。然后打开第二个网页并点击`Start building`，子域名可以任意填写，计划选择免费，并创建worker。

```
https://dash.cloudflare.com/
https://workers.cloudflare.com/
```

进入worker页面，更改三级域名，删除原始脚本。然后打开以下网页，复制代码到worker的脚本处，点击下方`保存并部署`，然后进行预览。使用时直接输入域名对应的网址即可。

```
https://github.com/EtherDream/jsproxy/blob/master/cf-worker/index.js
```

#### siteproxy

部署方法与jsproxy基本一致。仓库如下。

```
https://github.com/netptop/siteproxy
```

#### zmirror

需要VPS，通过以下脚本一键部署。仓库如下。

```
https://github.com/aploium/zmirror
https://github.com/aploium/zmirror-onekey
```

##### Ubuntu

支持Ubuntu 14.04、15.04(不支持HTTP2)、15.10、16.04+和Debian 8(不支持HTTP/2)。推荐Ubuntu 16.04 x86_64。

```
https://github.com/yumin9822/zmirror-docker/blob/master/zmirror-ubuntu.sh
```

##### Debian

```
https://github.com/yumin9822/zmirror-docker/blob/master/zmirror-debian.sh
```

##### CentOS 6

```
https://github.com/yumin9822/zmirror-docker/blob/master/zmirror-centos6.sh
```

#### jenssegers/proxy

可通过composer安装。

```
composer require jenssegers/proxy
```

仓库如下。

```
https://github.com/jenssegers/php-proxy
```


## 服务器加速

### BBR

#### 一键安装脚本

逗比版。

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/bbr.sh && chmod +x bbr.sh && bash bbr.sh
```

### 全类型

#### 一键安装脚本

在服务器输入以下命令以安装脚本。以安装BBR Plus为例，需要先安装BBR Plus内核，安装过程中弹框需选择`NO`。

```
wget --no-check-certificate -O tcp.sh https://github.com/cx9208/Linux-NetSpeed/raw/master/tcp.sh && chmod +x tcp.sh && ./tcp.sh
```

安装完成后再运行一次脚本，安装BBR Plus服务即可。

```
./tcp.sh
```

## CDN加速

登录CloudFlare，新建一个worker。用下面代码覆盖原有代码，把url.hostname替换成想要加速的V2Ray节点域名（不要添加前面的http和后面的路径）。

```
addEventListener(
  "fetch",event => {
     let url=new URL(event.request.url);
     url.hostname="xxxx-elecv2.cloud.okteto.net";
     let request=new Request(url,event.request);
     event. respondWith(
       fetch(request)
     )
  }
)
```

点击`保存`后，在客户端配置中复制一份原来的配置，把域名换成workers服务器的域名即可。原来的配置仍然可用。

## 处理DNS污染

可通过Accesser。以Mac为例，打开第一个链接并下载源码，解压后打开第二个链接下载dnscrypt-proxy，放到解压好的文件夹的dnscrypt目录下。

```
https://github.com/URenko/Accesser
https://github.com/jedisct1/dnscrypt-proxy/releases
```

打开终端并运行以下命令。

```
brew install openssl
pip3 install tld dnspython tornado
env ARCHFLAGS="-arch x86_64" LDFLAGS="-L/usr/local/opt/openssl/lib" CFLAGS="-I/usr/local/opt/openssl/include" pip install cryptography
pip3 install pyopenssl
```

打开网络偏好设置，选择当前连接的网络并点击`高级`，选择代理-自动代理配置，填写以下URL后保存。

```
http://127.0.0.1:7654/pac/
```

终端切换到源码目录并运行以下命令，在弹出的窗口点击导入证书，下载后双击安装。在弹出的`钥匙串访问`对话框中双击刚才安装的证书，在`信任`下选择始终信任。保持网页打开，即可访问受到DNS污染的网站。

```
python3 accesser.py
```

## Vmware虚拟机使用代理

虚拟机使用宿主机代理，不用设置额外VMware的转发，只需添加代理的地址与端口。手机也能使用虚拟机所配置的本机代理服务器，不需要同宿主机设置专属的VMware转发。

除默认配置的仅主机模式外，宿主机使用VPN会影响全局网络，虚拟机可以直接访问互联网。在NAT模式中，虚拟机通过宿主机器所在的公网网络来访问互联网（目前在墙内），Vmware使用代理软件转发端口监听任意地址，主机在代理中配置同一公网内的局域网IP与端口，完成网络之间的互联共享。在NAT模式中不考虑使用VPN或代理的情况下，IP地址是完全一致的。

虚拟机采用的是非全局性的独立网络，也因此在虚拟机使用VPN并不能使宿主机也能够访问互联网，但能够进行相关配置。

在Vmware中开启翻墙软件。下载Privoxy，该软件可将sock协议转换为http/https协议。

```
https://www.privoxy.org/
```

安装后配置相关参数为`0.0.0.0:8118`，将所有HTTP流量再转发至本机代理，然后在系统设置的代理中设置以上参数。打开命令行并通过`ipconfig`查看虚拟机的IP地址。在Vmware软件中设置端口映射，然后在主机的系统设置的代理中填入Vmware的IP地址和端口即可。

## 流媒体解锁

国外流媒体对IP有不同程度的要求。

### IP限制

#### Netflix

对于Netflix，原生IP可以解锁所有剧集，非原生IP有所区分。

对于谷歌云，35开头的IP可以观看带Netflix标志的电影，34开头的IP可以观看大多数片源。

对于亚马逊云，其IP可以观看大多数片源。Heroku的主机为亚马逊云，故使用Heroku的服务器为解锁所有Netflix影片最简单的方式。

### 解锁方法

#### DNS解锁服务器

该方法适用于当前没有服务器可解锁流媒体。

在服务器上安装V2Ray和宝塔面板，其中宝塔面板的安装命令如下。

```
// CentOS
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh

// Ubuntu/Deepin
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh

// Debian
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh

// Fedora
wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh
```

安装完成后进入宝塔界面，安装LNMP。在服务器输入以下命令，查看占用80端口的服务，一般为nginx。

```
netstat -apn
```

在宝塔界面点击软件商店-Nginx-设置-配置修改，在配置的最底部可以看到默认配置的路径位置，此处为/www/server/panel/vhost/nginx。利用宝塔界面的文件功能定位到该文件，将该文件的listen由80改为其它端口如801即可。

打开以下链接以购买流媒体解锁服务。

```
https://steamsv.com/
```

购买后在本机通过翻墙软件连接到刚才搭建的服务器，并设为全局模式。在流媒体解锁服务后台点击授权IP以绑定公网IP。

在服务器输入以下命令安装SNI proxy反向代理。

```
wget –no-check-certificate -O dnsmasq_sniproxy.sh https://raw.githubusercontent.com/myxuchangbin/dnsmasq_sniproxy_install/master/dnsmasq_sniproxy.sh && bash dnsmasq_sniproxy.sh -i
```

通过宝塔的文件功能编辑/etc/resolv.conf，将IP地址修改为流媒体解锁服务中的地址，保存退出即可。

SNI proxy仓库与相关错误排除链接如下。

```
https://github.com/myxuchangbin/dnsmasq_sniproxy_install
```

#### 流量转发

若已有一台可以解锁流媒体的服务器，可通过流量转发实现让其它服务器解锁流媒体。具体可查看翻墙协议的V2Ray配置部分。

## 翻墙协议

### Shadowsocks

#### 安装

##### 一键安装脚本

###### doubi

命令如下。

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/ss-go.sh && chmod +x ss-go.sh && bash ss-go.sh
```

###### 秋水逸冰

仓库如下。

```
https://github.com/teddysun/shadowsocks_install/tree/master
```

命令如下。配置文件地址为`/etc/shadowsocks-libev/config.json`。

```
// 安装
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log

// 卸载
./shadowsocks-all.sh uninstall

// 启动
/etc/init.d/shadowsocks-libev start

// 停止
/etc/init.d/shadowsocks-libev stop

// 重启
/etc/init.d/shadowsocks-libev restart

// 查看状态
/etc/init.d/shadowsocks-libev status
```


##### 原版

```
https://github.com/shadowsocks/shadowsocks/tree/master
```

##### go-shadowsocks2

以Go语言编写的Shadowsocks实现。

```
https://github.com/shadowsocks/go-shadowsocks2
```

##### shadowsocks-libev

Shadowsocks的轻量化实现。

```
https://github.com/shadowsocks/shadowsocks-libev
```

可使用一键脚本安装，仓库如下。

```
https://github.com/lrinQVQ/script
```

### ShadowsocksR

#### 安装

##### 一键安装脚本

###### 脚本一

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh
```

###### 脚本二

```
https://github.com/the0demiurge/CharlesScripts/blob/master/charles/bin/ssr
```

###### 脚本三

逗比版，支持单端口/多端口切换和管理。

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh
```

##### 原版

安装完成后，输入ssr help可以查看详细的命令列表。

###### 方法一

```
sudo apt install aptitude && sudo aptitude full-upgrade && sudo reboot
sudo aptitude install git
sudo aptitude install python-pip
sudo aptitude install curl libcurl3 libcurl3-dev php5-curl
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo aptitude full-upgrade && sudo apt-get install yarn
sudo yarn global add ssr-helper
sudo aptitude full-upgrade
cd ~
sudo git clone -b manyuser https://github.com/shadowsocksr-backup/shadowsocksr.git
ssr config ~/shadowsocksr
```

###### 方法二

```
sudo apt install aptitude && sudo aptitude full-upgrade && sudo reboot
sudo aptitude install git
sudo aptitude install npm
sudo aptitude install python-pip
sudo npm install -g ssr-helper
sudo aptitude full-upgrade
cd ~
sudo git clone -b manyuser https://github.com/shadowsocksr-backup/shadowsocksr.git
ssr config ~/shadowsocksr
```

### V2Ray

#### 安装

##### 官方安装

连接到VPS后，通过以下命令查看/修改时间。V2Ray要求时间准确，但时区会自动调整。

```
# 查看时间
# 输出示例Sun, 22 Jan 2017 10:10:36 -0500，其中-0500为西五区
date -R

# 修改时间
sudo date --set="2017-01-22 16:16:23"
```

输入`su`切换到root用户后，输入以下命令以安装curl。

```
# Debian/Ubuntu
apt update
apt install curl

# CentOS/RedHat
yum makecache
yum install curl

# Fedora
dnf makecache
dnf install curl

# openSUSE/SUSE
zypper refresh
zypper install curl
```

然后输入以下命令进行安装。

```
curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh
bash install-release.sh
```

安装完成后输入以下命令设置开机自启，并现在启动V2Ray。

```
systemctl enable v2ray
systemctl start v2ray
```

重新执行以下命令即可更新V2Ray。

```
bash install-release.sh
```

可通过以下命令查看脚本使用方法。

```
bash install-release.sh -h
```

##### 一键安装脚本

###### 脚本一

```
# 安装
source <(curl -sL https://multi.netlify.app/v2ray.sh) --zh

# 升级
source <(curl -sL https://multi.netlify.app/v2ray.sh) -k

# 卸载
source <(curl -sL https://multi.netlify.app/v2ray.sh) --remove
```

###### 脚本二

一键部署WebSocket+Tls+Nginx+Web。

```
wget -N --no-check-certificate -q -O install.sh "https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh" && chmod +x install.sh && bash install.sh
```

###### V2ray.Fun

V2ray控制脚本。

```
https://github.com/v2ray-fun/v2ray.fun
https://github.com/FunctionClub/V2ray.Fun
```

#### 配置

V2Ray通过config.json配置设置。可通过以下命令检查配置文件是否正确。

```
/usr/bin/v2ray/v2ray -test -config /etc/v2ray/config.json
```

##### 协议

仅列出重要的部分。

###### VMess

客户端配置如下。

```
  "outbounds": [
    {
      "protocol": "vmess", // 出口协议
      "settings": {
        "vnext": [
          {
            "address": "serveraddr.com", // 服务器地址，请修改为你自己的服务器 IP 或域名
            "port": 16823,  // 服务器端口
            "users": [
              {
                "id": "b831381d-6324-4d53-ad4f-8cda48b30811",  // 用户 ID，必须与服务器端配置相同
                "alterId": 64 // 此处的值也应当与服务器相同
              }
            ]
          }
        ]
      }
    }
  ]
```

服务端配置如下。

```
"inbounds": [
    {
      "port": 16823, // 服务器监听端口
      "protocol": "vmess",    // 主传入协议
      "settings": {
        "clients": [
          {
            "id": "b831381d-6324-4d53-ad4f-8cda48b30811",  // 用户 ID，客户端与服务器必须相同
            "alterId": 64
          }
        ]
      }
    }
  ]
```

###### Shadowsocks

客户端配置如下。

```
"outbounds": [
    {
      "protocol": "shadowsocks",
      "settings": {
        "servers": [
          {
            "address": "serveraddr.com", // Shadowsocks 的服务器地址
            "method": "aes-128-gcm", // Shadowsocks 的加密方式
            "ota": true, // 是否开启 OTA，true 为开启
            "password": "sspasswd", // Shadowsocks 的密码
            "port": 1024  
          }
        ]
      }
    }
  ]
```

服务端配置如下。

```
"inbounds": [
    {
      "port": 1024, // 监听端口
      "protocol": "shadowsocks",
      "settings": {
        "method": "aes-128-gcm",
        "ota": true, // 是否开启 OTA
        "password": "sspasswd"
      }
    }
  ]
```

###### HTTP

客户端配置如下。

```
"outbounds": [
      {
        "protocol": "http",
        "settings": {
          "servers": [
            {
              "address": "192.168.108.1",//服务器IP
              "port": 1024,//服务器端口
              "users": [
                {
                  "Username": "my-username",//将my-username改为你的用户名.
                  "Password": "my-password" //将my-password改为你的密码
                }
              ] 
            }
          ]
        },
        "streamSettings": {
          "security": "none", //如果是HTTPS代理,需要將none改為tls
          "tlsSettings": {
            "allowInsecure": false
            //检测证书有效性
        }
      }
    }
  ]
```

服务端配置如下。

```
"inbounds": [
    {
      "port": 1024, // 监听端口
      "protocol": "http",
      "settings": {
        "timeout:":0,
        "accounts":[
          {
            "user":"my-username",
            "pass":"my-password"
          }
        ],
        "allowTransparent":false,
        "userLevel":0
      }
    }
  ]
```

##### Mux

多路复用，仅需配置客户端。

```
  "outbounds": [
    {
      "mux": {"enabled": true}
    }
  ]
```

##### mKCP

再高丢包率网络下可开启该功能。注意会造成更多流量消耗。

服务端配置如下。

```
"inbounds": [
    {
      ...
      "streamSettings": {
        "network": "mkcp", //此处的 mkcp 也可写成 kcp，两种写法是起同样的效果
        "kcpSettings": {
          "uplinkCapacity": 5, // 上行链路容量（单位MB）
          "downlinkCapacity": 100, // 下行链路容量（单位MB）
          "congestion": true,
          "header": {
            "type": "none" // 数据包伪装（客户端与服务器要一致）
          }
        }
      }
    }
  ]
```

客户端配置如下。

```
"outbounds": [
    {
      ...
      "streamSettings": {
        "network": "mkcp",
        "kcpSettings": {
          "uplinkCapacity": 5,
          "downlinkCapacity": 100,
          "congestion": true,
          "header": {
            "type": "none"
          }
        }
      }
    }
  ]
```

##### 动态端口

仅适用于vmess。

服务端配置如下。客户端不用额外设定，客户端会先与服务器的主端口通信协商下一个使用的端口号。

```
"inbounds":[
  { //主端口配置
      "port": 37192,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "d17a1af7-efa5-42ca-b7e9-6a35282d737f",
            "alterId": 64
          }
        ],
        "detour": { //绕行配置，即指示客户端使用 dynamicPort 的配置通信
          "to": "dynamicPort"
        }
      }
    },
    {
      "protocol": "vmess",
      "port": "10000-20000", // 端口范围
      "tag": "dynamicPort",  // 与上面的 detour to 相同
      "settings": {
        "default": {
          "alterId": 64
        }
      },
      "allocate": {            // 分配模式
        "strategy": "random",  // 随机开启
        "concurrency": 2,      // 同时开放两个端口,这个值最大不能超过端口范围的 1/3
        "refresh": 3           // 每三分钟刷新一次
      }
    }
  ]
```

##### 日志

服务端与客户端配置一致。

```
"log": {
    "loglevel": "warning",
    "access": "/var/log/v2ray/access.log",
    "error": "/var/log/v2ray/error.log"
  }
```

loglevel为日志级别，由详细到简略分别为debug、info、warning、error、none。若不填写access和error，则日志会输出到命令行中。

##### 路由

###### 基本逻辑

路由主要是配置protocol。

以客户端outbounds的protocol为例，若为vmess则走翻墙，若为freedom则走直连，若为blackhole则阻止。示例如下。

```
  "outbounds": [ 
    {
      "protocol": "vmess", // 出口协议
      "settings": {
        "vnext": [
          {
            "address": "serveraddr.com", // 服务器 IP 地址
            "port": 16823,  // 服务器端口
            "users": [
              {
                "id": "b831381d-6324-4d53-ad4f-8cda48b30811",  // 用户 ID，须与服务器端配置相同
                "alterId": 64
              }
            ]
          }
        ]
      }
    },
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "protocol": "blackhole",
      "settings": {}
    }
  ]
```

在包含多个出口协议的情况下，只会以outbounds中的第一个出口作为默认的出口。

###### 域名文件

V2Ray内置有geosite.dat，该文件中包含常用的网站域名。

指定国内网站走直连的写法如下。

```
{
    "type": "field",
    "outboundTag": "direct",
    "domain": [
        "geosite:cn"
    ]
}
```

也可使用外置域名文件。可下载h2y.dat到V2Ray客户端运行目录，链接如下。

```
https://github.com/ToutyRater/V2Ray-SiteDAT/tree/master/geofiles
```

写法如下。

```
"rules":[
    {
        "type": "field",
        "outboundTag": "block", //拦截广告相关域名
        "domain": [
            "ext:h2y.dat:ad"
        ]
    },
    {
        "type": "field",
        "outboundTag": "proxy", //被 gfw 屏蔽的域名走代理
        "domain": [
            "ext:h2y.dat:gfw"
        ]
    },
    {
        "type": "field",
        "network":"tcp,udp",
        "outboundTag": "direct" // 默认直连
    }
]
```

###### 禁用BT

服务端配置如下。此配置阻断被转发到服务器的BitTorrent连接。

```
  "inbounds": [
    {
      "sniffing": {
        "enabled": true,
        "destOverride": [
          "http",
          "tls"
        ]
      },
      ...
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "block"
    }
  ],
  "routing": {
    "domainStrategy": "AsIs",
    "rules": [
      {
        "type": "field",
        "outboundTag": "block",
        "protocol": [
          "bittorrent"
        ]
      }
    ]
  }
```

客户端配置如下。

```
  "outbounds": [
    ...
    {
      "protocol": "freedom",
      "settings": {},
      "tag": "direct"
    }
  ],
  "routing": {
    "domainStrategy": "AsIs",
    "rules": [
      {
        "type": "field",
        "outboundTag": "direct",
        "protocol": [
          "bittorrent"
        ]
      }
    ]
  }
```

##### 中转

V2Ray的中转功能可以将特定流量转发到另一台服务器上。假设在服务器1上配置了至服务器2的中转，则流量走向如下。

```
本机-服务器1-服务器2-互联网
```

本功能可用于解锁Netflix。若已有一台可观看Netflix的服务器1，希望使服务器2也能观看Netflix，则可以在服务器2上进行配置，主要是指定普通流量直接走服务器1本身，而Netflix流量走服务器2。

在服务器2安装好V2Ray后，从服务器2下载/etc/v2ray/config.json到本地，打开并进行如下修改。

```
{
  ...
  "inbounds":[
    ...
    "sniffing": {
        "enabled": true, // 一定要开启sniffing，V2Ray才能识别Netflix的流量
        "destOverride": ["http", "tls"]
    },
    ...
  ],
  "outbounds": [
    ...
    {
      ...
    },
    ...
    {
      "tag": "VPS1",

      // 填写可观看Netflix的VPS信息，可通过V2RayN的导出服务器配置得到，示例如下
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "example.com",
            "port": 443,
            "users": [
              {
                "id": "abcd1234-1234-1234-1234-abcd12345678",
                "alterId": 0,
                "email": "t@t.tt",
                "security": "auto"
              }
            ]
          }
        ],
        "servers": null,
        "response": null
      },
      "streamSettings": {
        "network": "ws",
        "security": "tls",
        "tlsSettings": {
          "allowInsecure": true,
          "serverName": null
        },
        "tcpSettings": null,
        "kcpSettings": null,
        "wsSettings": {
          "connectionReuse": true,
          "path": "/",
          "headers": null
        },
        "httpSettings": null,
        "quicSettings": null
      },
      "mux": {
        "enabled": true,
        "concurrency": 8
      }
    }

    //include_out_config
    //
  ],
  "routing": {
    "domainStrategy": "IPOnDemand", 
    "rules": [
      {
            "type": "field",
            "outboundTag": "VPS1",
            "domain": [
              "geosite:netflix"
            ]
        },
      {
        ...
      },
      ...
      //include_ban_ad
      //include_rules
      //
    ]
  },
...
}
```

修改完成后将新文件上传到服务器2以覆盖原配置文件，并重启V2Ray即可。

##### 代理转发

代理转发的作用与中转基本一致，主要适用于服务器1和服务器2都无法修改配置的情况。通过直接修改客户端的配置，可以让客户端以服务器2为跳板，将Netflix的流量转发至服务器1。

注意，代理转发会使streamSettings失效。客户端配置文件示例如下。

```
{
  "inbounds": [
    {
      "port": 1080, 
      "protocol": "socks", 
      "sniffing": {
        "enabled": true, // 一定要开启sniffing，V2Ray才能识别Netflix的流量
        "destOverride": ["http", "tls"]
      },
      "settings": {
        "auth": "noauth"  
      }
    }
  ],
  "outbounds": [
    {
      // 服务器1的配置，可通过V2RayN的导出服务器配置得到，示例如下
      "tag": "VPS1"
      "protocol": "vmess",
      "settings": {
        "vnext": [{
          "address": "1.2.3.4", 
          "port": 10086,
          "users": [{
            "id": "23ad6b10-8d1a-40f7-8ad0-e3e35cd38297",
            "security": "auto",
            "alterId": 64
          }]
        }]
      },
      "proxySettings": {
        "tag": "VPS2"  // 表示VPS1的流量由VPS2转发
      }
    },
    {
      // 服务器2的配置，可通过V2RayN的导出服务器配置得到，示例如下
      "tag": "VPS2"
      "protocol": "vmess",
      "settings": {
        "vnext": [{
          "address": "2.2.3.5", 
          "port": 16823,
          "users": [{
            "id": "b831381d-6324-4d53-ad4f-8cda48b30811",
            "security": "auto",
            "alterId": 64
          }]
        }]
      }
    },
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {}
    }
  ],
  "routing": {
    "rules": [
      {
        "type": "field",
        "outboundTag": "direct",
        "domain": ["geosite:cn"] // 国内直连（本机-互联网）
      },    
      {
        "type": "field",
        "outboundTag": "VPS1",
        "domain": ["geosite:netflix"] // Netflix走VPS1（本机-VPS2-VPS1-互联网）
      },
      {
        "type": "field",
        "outboundTag": "VPS2",
        "network": "udp,tcp" // 其余走VPS2（本机-VPS2-互联网）
      }  
    ]
  }
}
```

代理转发可以配置为链式，示例如下。转发路径为PC-AliHK-AliSG-DOSG-DOUS-目标网站。

```
{
  "outbounds": [
    {
      ...
      "tag": "AliHK"
    },
    {
      ...
      "tag": "AliSG",
      "proxySettings": {
          "tag": "AliHK"  
      }
    },
    {
      ...
      "tag": "DOSG",
      "proxySettings": {
          "tag": "AliSG"  
      }
    },
    {
      ...
      "tag": "DOUS",
      "proxySettings": {
          "tag": "DOSG"  
        }
    },
  ]
}
```

##### HTTP伪装

将V2Ray的流量伪装成正常的HTTP协议。

服务器配置如下。

```
"inbounds": [
    {
      ...
      "streamSettings": {
        "network": "tcp",
        "tcpSettings": {
          "header": { // header 这一项是关于数据包伪装的设置，可自定义合理的内容，但要确保服务器与客户端一致
            "type": "http",
            "response": {
              "version": "1.1",
              "status": "200",
              "reason": "OK",
              "headers": {
                "Content-Type": ["application/octet-stream", "application/x-msdownload", "text/html", "application/x-shockwave-flash"],
                "Transfer-Encoding": ["chunked"],
                "Connection": ["keep-alive"],
                "Pragma": "no-cache"
              }
            }
          }
        }
      }
    }
  ]
```

客户端配置如下。

```
"outbounds": [
    {
      ...
      "streamSettings": {
        "network": "tcp",
        "tcpSettings": {
          "header": {  //这里的 header 要与服务器保持一致
            "type": "http",
            "request": {
              "version": "1.1",
              "method": "GET",
              "path": ["/"],
              "headers": {
                "Host": ["www.cloudflare.com", "www.amazon.com"],
                "User-Agent": [
                  "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.75 Safari/537.36",
                          "Mozilla/5.0 (iPhone; CPU iPhone OS 10_0_2 like Mac OS X) AppleWebKit/601.1 (KHTML, like Gecko) CriOS/53.0.2785.109 Mobile/14A456 Safari/601.1.46"
                ],
                "Accept-Encoding": ["gzip, deflate"],
                "Connection": ["keep-alive"],
                "Pragma": "no-cache"
              }
            }
          }
        }
      }
    },
    {
      "protocol": "freedom",
      "settings": {},
      "tag": "direct"
    }
  ],
```

##### 负载均衡

客户端配置如下。

```
{

  "inbounds": [
    ...
  ],
  "outbounds": [
    {
      "tag": "us1",
      ...
    },
    {
      "tag": "jp1",
      ...
    },
    {
      "tag": "jp2",
      ...
    },
    {
      "tag": "hk1",
      ...
    },
    {
      "tag": "direct",
      ...
    }
  ],
  "routing": {
    "domainStrategy": "IPOnDemand",
    "balancers": [
      {
        "tag": "b1",
        "selector": [
          "jp1",
          "jp2"
        ]
      },
      {
        "tag": "b2",
        "selector": [
          "us1",
          "hk1"
        ]
      }
    ],
    "rules": [
      {
        "type": "field",
        "outboundTag": "direct",
        "ip": [
          "geoip:private",
          "geoip:cn"
        ]
      },
      {
        "type": "field",
        "outboundTag": "direct",
        "domain": [
          "geosite:cn"
        ]
      },
      {
        "type": "field",
        "network": "tcp,udp",
        "balancerTag": "b1"
      }
    ]
  }
}
```

#### 原理

##### 架构

客户端与服务端的配置文件都有入站协议inbounds和出站协议outbounds。其中inbounds是关于如何与上一个节点连接的配置，outbounds是关于如何与下一个节点连接的配置。

对于客户端，inbounds与浏览器连接，进行代理相关配置，outbounds为节点列表。

inbounds中的相关选项作用如下。

|    选项    |                                        说明                                       |
|------------|-----------------------------------------------------------------------------------|
| "sniffing" | 从网络流量中识别出域名，以解决DNS污染、识别BT协议，对于IP流量可以应用域名路由规则 |

对于服务端，inbounds为服务器相关设置，outbounds为可以转发的域名，一般为freedom，即所有流量都从本服务器发出。

inbounds中的相关选项作用如下。

|   选项  |                                说明                                |
|---------|--------------------------------------------------------------------|
| alterId | 加强防探测能力（理论上越大越好，但越大会约占内存，建议设为30-100） |

数据包流向如下。

```
{浏览器}<--(socks)-->{V2Ray客户端inbound<->V2Ray客户端 outbound}<--(VMess)-->{V2Ray服务器inbound<->V2Ray服务器outbound}<--(Freedom)-->{目标网站}
```

##### WS+TLS

在V2Ray的配置文件config.json中，采用WS+TLS的写法示例如下。

```
{
  ...
  "inbounds":[
    ...
    {
      "port": 8089,
      "protocol": "vmess",
      "settings": {
        "clients": [{
          "id": "71880ee2-4d15-47da-87b2-xxxxxxxxxx",
          "alterId": 64
        }]
      },
      "streamSettings": {
        "network": "ws"
      },
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      },
    },
    ...
  ],
  ...
}
```

其中port所指定的端口用于将流量转发到指定域名。caddy实现该转发功能，通过以下命令可打开其配置文件。

```
vim /etc/caddy/Caddyfile
```

指定地址为127.0.0.1:8089即可，其中8089为上面的port。

### Brook

#### 安装

##### 一键安装脚本

##### 脚本一

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/brook.sh && chmod +x brook.sh && bash brook.sh
```

##### 脚本二

```
curl -L https://github.com/txthinking/brook/releases/download/v20200909/brook_linux_amd64 -o /usr/bin/brook
chmod +x /usr/bin/brook
// 启动brook并增加守护进程，端口设置为9999，密码设置为password
setsid ./brook server -l :9999 -p password
```

### Trojan

#### 安装

##### 一键安装脚本

```
bash -c "$(curl -fsSL https://raw.githubusercontent.com/atrandys/trojan/master/trojan_mult.sh)"
```

### WireGuard

WireGuard使用UDP。

#### 安装

##### 一键安装脚本

```
curl -O https://raw.githubusercontent.com/atrandys/wireguard/master/wg_mult.sh && chmod +x wg_mult.sh && ./wg_mult.sh
```

安装完成后需要下载/etc/wireguard/client.conf到本地。可通过以下命令直接打开该文件，然后复制文本内容至本地。

```
cat /etc/wireguard/client.conf
```

也可通过以下命令下载文件。

```
yum -y install lrzsz
sz /etc/wireguard/client.conf
```

使用客户端时配置文件选择刚才下载的conf即可。

### ZeroTier

通过ZeroTier，可将服务器IP与客户端IP置于同一个内网，从而达到翻墙的目的。

服务器需要提前安装好Ubuntu。以非root身份登录服务器，并输入以下命令以安装Zerotier。脚本完成后记录输出中方括号内的ZeroTier地址。

```
curl -s 'https://pgp.mit.edu/pks/lookup?op=get&search=0x1657198823E52A61' | gpg --import && if z=$(curl -s 'https://install.zerotier.com/' | gpg); then echo "$z" | sudo bash; fi
```

然后输入以下命令判断是否已经启用Linux内核数据转发，输出`net.ipv4.ip_forward = 0`为未启用，输出`net.ipv4.ip_forward = 1`为已启用。

```
sudo sysctl net.ipv4.ip_forward
```

若未启用，则输入以下命令。

```
sudo vi /etc/sysctl.conf
```

在文件底部增加以下行，然后按Esc后输入`:wq`退出。

```
net.ipv4.ip_forward = 1
```

输入以下命令以触发新的内核配置即可。

```
sudo sysctl -p
```

输入以下命令以显示网络接口的名称。

```
ip link show
# 或ip addr
```

示例如下。

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 72:2d:7e:6f:5e:08 brd ff:ff:ff:ff:ff:ff
3: zt0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 2800 qdisc pfifo_fast state UNKNOWN mode DEFAULT group default qlen 1000
    link/ether be:82:8f:f3:b4:cd brd ff:ff:ff:ff:ff:ff
```

输入以下命令以添加规则，并允许服务器的网卡eth0转发所有ZeroTier虚拟网卡zt0的流量。

```
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i zt0 -o eth0 -j ACCEPT
```

因为iptables规则会在服务器重启后丢失，因此需要输入以下命令安装iptables-persistent并保存添加的规则。

```
sudo apt install iptables-persistent
sudo netfilter-persistent save
```

运行时会有提示，分别保存IPv4和IPv6的规则。需根据服务器是否提供该类地址而选择是否保存，若只有IPV4地址，则只保存IPV4规则即可，无需保存IPV6规则。

通过以下命令查看已保存的规则。

```
sudo iptables-save
```

输出的规则内容如下。

```
# Generated by iptables-save v1.6.1 on Thu May 24 08:39:37 2018
*nat
:PREROUTING ACCEPT [256:29654]
:INPUT ACCEPT [230:28276]
:OUTPUT ACCEPT [2812:383290]
:POSTROUTING ACCEPT [2220:333986]
-A POSTROUTING -o eth0 -j MASQUERADE
-A POSTROUTING -o eth0 -j MASQUERADE
COMMIT
# Completed on Thu May 24 08:39:37 2018
# Generated by iptables-save v1.6.1 on Thu May 24 08:39:37 2018
*filter
:INPUT ACCEPT [39673:10536131]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [41822:10787996]
:sshguard - [0:0]
-A INPUT -j sshguard
-A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -i zt0 -o eth0 -j ACCEPT
COMMIT
# Completed on Thu May 24 08:39:37 2018
```

上述内容表示已保存转发规则，可以转发来自客户端的流量。

打开以下链接并注册，进入后台后点击Networks-Create，记录Network ID。保证Access Control为Certificate (Private Network)，IPv4 Auto-Assign需要勾选Auto-Assign from Range，下面的地址可以任意选择。

```
https://www.zerotier.com/
```

在服务器上输入以下命令以连接到网络，其中Network ID需要修改为刚才记下的值。

```
sudo zerotier-cli join [NetworkID]
```

在客户端下载Zerotier客户端，链接如下。打开客户端并输入刚才记下的Network ID以连接。

```
https://www.zerotier.com/download/
```

返回Zerotier后台，在Members下可看到服务端和客户端，勾选它们前面的`Auth?`选项，等待一段时间后分别在服务端和客户端输入以下命令以查看自己的IP地址。

```
ip addr sh zt0 | grep 'inet'
```

在服务端和客户端分别ping对方的地址，查看是否已经联通。回到Zerotier后台，在Managed Routes一栏下分别填写0.0.0.0/0和服务器的IP地址，表示由该服务器处理所有客户端的流量。

在客户端勾选Route all traffic through ZeroTier，即可使客户端的流量通过服务器。

### 全平台快速搭建

#### Streisand

在VPS上运行多个不同的翻墙工具。

```
https://github.com/StreisandEffect/streisand/blob/master/README-chs.md
```

#### ProxySU

适用于Windows。打开以下链接以下载ProxySU。

```
https://github.com/proxysu/ProxySU
```

完成后在本地打开，按照流程进行即可。

## 相关命令

### 关闭防火墙

适用于CentOS。

```
// 查看防火墙状态
firewall-cmd --state

// 停止防火墙
systemctl stop firewalld.service

// 禁止防火墙开机启动
systemctl disable firewalld.service
```

### 修改中国时区

```
\cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

### 利用NTP同步时间协议

```
// CentOS
yum install ntp ntpdate -y
// Ubuntu/Debian
apt-get install ntp ntpdate -y

service ntpd stop
ntpdate us.pool.ntp.org
service ntpd start
```

### 提示curl: command not found

未安装curl导致。通过以下命令安装。

```
// ubuntu/debian系统
apt-get update -y && apt-get install curl -y

// centOS系统
yum update -y && yum install curl -y
```

### 提示wget: command not found

未安装wget导致。通过以下命令安装。

```
// ubuntu/debian系统
apt-get update -y && apt-get install -y wget

// centOS系统
yum update -y && yum install -y wget
```

# 服务器进阶

## SSH连接

SSH可用于连接到服务器。

### 连接方法

#### 浏览器连接

GCP默认方式。点击服务器的`SSH`按钮即自动建立与服务器的SSH连接。

#### 命令行连接

在终端输入以下命令即可。

```
ssh -p 22 root@localhost
```

#### 手机APP连接

推荐使用HyperApp。

### 文件传输

可通过scp命令向连接到的服务器传输文件，命令格式如下。

```
// 上传文件到服务器
scp [本地文件路径] remote_username@remote_ip:[服务器文件路径]
// 上传文件夹到服务器
scp -r [本地文件夹路径] remote_username@remote_ip:[服务器文件夹路径]
```

下载文件或文件夹，则将后面参数对调即可。

### 服务器配置

一般服务器会提供SSH连接的入口，包括地址、密码等，端口默认为22。但有部分服务器需要手动配置。

#### Google Cloud Platform

通过浏览器进入服务器后台，输入以下命令以切换到root并编辑SSH配置文件。

```
sudo -i
vi /etc/ssh/sshd_config
```

在配置文件中修改PermitRootLogin和PasswordAuthentication为yes，如下。

```
# Authentication:
PermitRootLogin yes //默认为no，需要开启root用户访问改为yes

# Change to no to disable tunnelled clear text passwords
PasswordAuthentication yes //默认为no，改为yes开启密码登陆
```

保存后输入以下命令以给root用户设置密码。

```
passwd root
```

设置完成后输入以下命令重启SSH服务即可。

```
/etc/init.d/ssh restart
```

## 运行代码

### 配置服务器运行环境

以运行Python代码为例。连接到服务器并取得root权限，运行以下命令以安装Python 3.7和pip。

```
apt-get install python3.7
apt-get install pip
```

通过pip安装代码运行所需要的库，如下。

```
pip install [库名]
```

进入到用户目录并新建文件夹，作为代码存放的位置。

```
cd /home/[用户名]
mkdir [文件夹名]
```

### 配置本地运行环境

打开Pycharm Professional，新建工程后选择File-Settings，进入project interpreter，点击add，选择SSH interpreter。输入服务器信息后进行连接，连接完成后配置编译器路径为`/usr/bin/python3`。

保存后选择Tool-Deployment-Configuration，点击刚才新建的服务器，在Mappings标签页选择路径为在服务器端新建好的文件夹。保存后选择Upload即可。

### 运行代码

#### 通过Pycharm

运行前选择服务器端的编译器，运行即可。

#### 通过服务器自身

在需要运行的Python代码最前面加入以下代码。

```
#!/usr/bin/python3
```

进入服务器命令行，切换到代码目录后通过以下命令使代码后台运行并将输出存到文件。此时可以关闭该SSH连接。

```
nohup python3 -u [test.py] > [test.log] 2>&1 &
```

需要结束运行时，执行以下代码即可。

```
ps -aux | grep python3
kill [PID]
```

也可通过pm2的方式。安装pm2管理器后运行以下命令即可。

```
pm2 start [test.py] --name [name]
pm2 save
pm2 startup
```

结束运行的命令与上面一致。

## 网盘文件下载

### Google Drive

#### 通过wget

从Google Drive分享文件后获取其链接，示例如下。其中`1dt4SEYtvK_7LcFNaxKsldGCDd1SMbBlAFILEID`即为`FILEID`。

```
https://drive.google.com/file/d/1dt4SEYtvK_7LcFNaxKsldGCDd1SMbBlA/view?usp=sharing
```

对于小文件采用以下命令，其中`FILENAME`自己命名即可。

```
wget --no-check-certificate ‘https://docs.google.com/uc?export=download&id=FILEID’ -O FILENAME
```

对于大文件采用以下命令。

```
wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=FILEID' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=FILEID" -O FILENAME && rm -rf /tmp/cookies.txt
```

#### 通过gdown

在终端输入以下命令即可。

```
pip install gdown
gdown https://drive.google.com/uc?id=[文件ID]
```

#### 通过gdown脚本

下载以下脚本。

```
https://raw.githubusercontent.com/circulosmeos/gdown.pl/with-resume/gdown.pl
```

在终端切换到脚本所在目录，运行以下命令即可。

```
./gdown.pl [文件链接] [重命名的文件名]
```

## 创建图形界面系统

### 在Google Cloud Shell中

进入GCP并点击右上方的终端按钮，进入Cloud Shell。输入以下命令以部署并查看状态。

```
sudo -i
docker run -itd -p 6080:6080 -e PASSWORD=1234 chenjr0719/ubuntu-unity-novnc
docker ps
```

点击Cloud Shell右上角的预览，更改端口为6080，更改并预览即可。

### 在VM实例中

新建VM实例并通过SSH连接，然后输入以下命令。

```
sudo -i
apt update
apt-get install --assume-yes wget
wget https://dl.google.com/linux/direct/chrome-remote-desktop_current_amd64.deb
dpkg --install chrome-remote-desktop_current_amd64.deb
apt install --assume-yes --fix-broken
DEBIAN_FRONTEND=noninteractive apt install --assume-yes xfce4 desktop-base
bash -c 'echo "exec /etc/X11/Xsession /usr/bin/xfce4-session" > /etc/chrome-remote-desktop-session'
apt install --assume-yes xscreensaver

// （可选）安装全套Linux桌面应用，包括Firefox浏览器、LibreOffice办公应用套件和Evince PDF查看器
apt install --assume-yes task-xfce-desktop

systemctl disable lightdm.service

// （可选）安装Chrome浏览器
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
dpkg --install google-chrome-stable_current_amd64.deb
apt install --assume-yes --fix-broken

usermod -a -G chrome-remote-desktop $USER
logout
logout
```

重新连接到实例，然后打开以下链接，一路下一步。

```
https://remotedesktop.google.com/headless
```

复制含DISPLAY的代码到VM实例中并运行，然后打开以下链接以访问远程桌面。

```
https://remotedesktop.google.com/access/
```

## 安装matlab

通过以上过程安装图形界面，然后在图形界面中进入官网下载安装包即可。

安装完成后，通过SSH连接服务器并运行`matlab`即可调用matlab。

## 端口限制上行网速

在命令行输入以下命令，检查`iptables`和`tc`是否被安装，若未安装则通过apt等软件包管理器安装。

```
iptables -V
tc -V
```

通过以下命令查看网卡。需要找到外网网卡，可通过IP地址判断。

```
ifconfig
```

以eth0为例，命令如下。

```
// 清理iptables Mangle规则
iptables -t mangle -F

// 清理eth0上原有的队列类型
tc qdisc del dev eth0 root

// 给eth0添加一个根规则
tc qdisc add dev eth0 root handle 1: htb default 1

// 创建根类（100mbps替换成服务器的实际带宽）
tc class add dev eth0 parent 1: classid 1:1 htb rate 100mbps

// 创建支类限速（以下数据根据需要进行替换）
// rate1500Kbit代表最大带宽1536Kbit/s
// ceil 2048Kbit代表突发带宽2048Kbit/s
// ceil>=rate
// 1:5指每5秒钟检查一次
tc class add dev eth0 parent 1:1 classid 1:5 htb rate 1500Kbit ceil 2048Kbit prio 1

// 创建过滤器（flowid和上一条的classid对应）
tc filter add dev eth0 parent 1:0 prio 1 protocol ip handle 5 fw flowid 1:5

// 借助iptables针对端口限速（80为端口号）
iptables -A OUTPUT -t mangle -p tcp --sport 80 -j MARK --set-mark 5

// 限制多端口
// iptables -A OUTPUT -t mangle -p tcp --sport 80,25,443,8989 -j MARK --set-mark 5

// 限制10000-20000端口
// iptables -A OUTPUT -t mangle -p tcp --sport 10000:20000 -j MARK --set-mark 5
```

## 挂机赚钱

### Vagex

在命令行输入以下命令以安装容器。

```
docker run -d --name alpine-ssh-xfce4-vnc -p 22:22 -p 3389:3389 charliev5/alpine-desktop
```

安装完成后通过远程桌面连接到服务器，然后用Firefox打开以下网址，注册后安装插件并重启浏览器即可。

```
https://vagex.com/
```

可通过以下命令检查是否正常运行。

```
wget https://raw.githubusercontent.com/WangCharlie/alpine-ssh-xfce4-vnc/master/check.sh
sh check.sh
```

### eBesucher

官网如下。

```
https://www.ebesucher.com/
```

### Google Adsense

需要有个人网站。可用西联汇款收款。

```
https://www.google.com.tw/intl/zh-CN_cn/adsense/start/?utm_campaign=redirect-301
```

## Docker

### 机制

Docker即镜像，相当于打包好的系统，部署到服务器后即可直接使用。Docker一般可在Docker Hub上找到。

```
https://registry.hub.docker.com/
```

Docker部署到服务器后，该服务被称为Container，即容器。以Kubernetes集群为例，安装Docker时一般需要有yml文件以配置相关参数。

示例如下。将`v2-app`改为另一个名称，`pch18/baota:clear`更换为其他容器，`port: 8888`改为容器内暴露端口即可，其中暴露端口可看相关镜像的说明。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: v2-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: v2-app
  template:
    metadata:
      labels:
        app: v2-app
    spec:
      containers:
      - image: gingko/v2ray-nginx-websocket
        name: v2-app

---

apiVersion: v1
kind: Service
metadata:
  name: v2-app
  annotations:
    dev.okteto.com/auto-ingress: "true"
spec:
  type: ClusterIP  
  ports:
  - name: "http-port-tcp"
    port: 8080
  selector:
    app: v2-app
```

保存后执行以下命令部署即可。

```
kubectl apply -f [yml文件路径]
```

仓库和示例如下。

```
https://github.com/pch18-docker/baota
```

### yml文件

#### 搭建宝塔

完成后通过所给的网站即可访问。用户名为`username`，密码为`password`。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bt-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: bt-app
  template:
    metadata:
      labels:
        app: bt-app
    spec:
      containers:
      - image: baiyuetribe/baota-mini
        name: bt-app

---

apiVersion: v1
kind: Service
metadata:
  name: bt-app
  annotations:
    dev.okteto.com/auto-ingress: "true"
spec:
  type: ClusterIP  
  ports:
  - name: "http-port-tcp"
    port: 8888
  selector:
    app: bt-app
```

#### 搭建Google镜像网站

完成后通过所给的网站即可访问。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: google-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: google-app
  template:
    metadata:
      labels:
        app: google-app
    spec:
      containers:
      - image: jim3ma/google-mirror
        name: google-app

---

apiVersion: v1
kind: Service
metadata:
  name: google-app
  annotations:
    dev.okteto.com/auto-ingress: "true"
spec:
  type: ClusterIP  
  ports:
  - name: "http-port-tcp"
    port: 80
  selector:
    app: google-app
```

#### 搭建Linux

完成后通过所给的网站即可访问，默认以root身份登录。密码为`vncpassword`。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: google-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ubuntu-app
  template:
    metadata:
      labels:
        app: ubuntu-app
    spec:
      containers:
      - image: fallfor/ubuntuvnc
        name: ubuntu-app

---

apiVersion: v1
kind: Service
metadata:
  name: ubuntu-app
  annotations:
    dev.okteto.com/auto-ingress: "true"
spec:
  type: ClusterIP  
  ports:
  - name: "http-port-tcp"
    port: 6901
  selector:
    app: ubuntu-app
```

可用的其他镜像如下，默认以非root身份登录。密码也为`vncpassword`。

```
consol/centos-xfce-vnc
consol/ubuntu-xfce-vnc
consol/centos-icewm-vnc
consol/ubuntu-icewm-vnc
```

### Docker收集

#### 库

```
https://github.com/mritd/dockerfile
```

#### 签到

```
https://github.com/AragonSnow/qiandao
```

## 一键重装系统

### 原系统为Linux

特别注意OpenVZ构架不适用，安装之前备份重要数据。适用于由GRUB引导的CentOS/Ubuntu/Debian系统。使用官方发行版去掉模板预装的软件，同时也可以解决内核版本与软件不兼容的问题。

全自动安装默认root密码为Vicer。相关软件依赖如下。

```
// Debian/Ubuntu
apt-get update
apt-get install -y gawk sed grep
 
// RedHat/CentOS
yum update
yum install -y gawk sed grep
```

在安装Ubuntu时可能会遇到`Getting the time form a network time server...`并界面进度条很长时间不会动，可以等待其超时，或更换别的版本。

#### Debian 7

```
# x86
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 7 -v 32

# x64
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 7 -v 64
```

#### Debian 8

```
# x86
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 8 -v 32

# x64
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 8 -v 64
```

#### Debian 9

```
# x86
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 9 -v 32

# x64
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 9 -v 64
```

#### Ubuntu 14.04

```
# x86
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d trusty -v 32

# x64
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d trusty -v 64
```

#### Ubuntu 16.04

```
# x86
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d xenial -v 32

# x64
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d xenial -v 64
```

#### Ubuntu 17.04

```
# x86
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d zesty -v 32

# x64
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d zesty -v 64
```

#### CentOS

X64版本，root用户密码为`xiaofd.win`。

```
wget xiaofd.github.io/centos.sh && bash centos.sh
```

#### Windows

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && bash DebianNET.sh -dd 'https://moeclub.org/get-win7embx86-auto'
```

### 原系统为Windows

#### Linux

下载以下脚本并运行即可。

```
https://moeclub.org/attachment/WindowsSoftware/win32loader.bat
```

# 特殊工具

## Tor

Tor是经过层层加密的浏览器，从使用者本机直到出口节点的传输是强加密的，其他人无法偷窥到真实网络流量，除非Tor软件本身出现严重安全漏洞，或者碰到的出口节点是蜜罐节点（或称为陷阱节点），即故意引诱用户攻击的目标。

Tor主要用于隐匿上网身份，也可用于伪装别国网民、隐匿公网IP、保护隐私等。Tor的官方Wiki如下。

```
https://gitlab.torproject.org/legacy/trac/-/wikis/home
```

### 原理

#### Tor联网

当Tor客户端启动之后，会首先连接Tor的目录服务器，从目录服务器中获取全球的Tor节点信息。访问网站时，Tor客户端会随机挑选三个节点用于中转，如下。

| 节点类型 |                    特点                    |
|----------|--------------------------------------------|
| 入口节点 | 直接与本机或前置代理（若使用双重代理）相连 |
| 中间节点 | 介于入口节点和出口节点之间，该节点威胁最小 |
| 出口节点 | 直接与访问的目标网站相连，该节点威胁最大   |

只有出口节点会看到用户的上网行为，包括访问的网站、从该网站传输的流量等。其它节点完全无法了解上网行为。

如果这个网站使用HTTPS协议，出口节点看到的原始流量是HTTPS的密文。但如果这个网站使用HTTP 协议，那么出口节点看到的原始流量是HTTP的明文。

Tor为了加强隐匿性，会动态变化中转线路，每隔一段时间随机挑选三个节点，重新构造一条传输线路。因为线路动态变化，出口节点自然也动态变化，所以即使出口节点偷窥上网行为，也只能看到一个短暂的片段。

#### 流量混淆

若ISP和墙在监控流量，则可以判断出在使用Tor。因此需要使用流量混淆，把Tor流量伪装成其它的上网流量。出于软件架构方面的考虑，流量混淆通过插件的方式来提供，因为混淆流量的方式是多种多样的。

meek是常用的流量混淆插件，可以把Tor流量伪装成访问云计算平台的流量。当数据流量到达云计算平台之后，会经过一系列中转，最终转向真正访问的网站。由于传输流量经过伪装，墙比较难区分伪装的Tor流量和真正访问云计算的普通流量，而且meek依赖的云计算平台都是亚马逊、微软等大公司提供，墙不会轻易封锁其公网IP。

### 使用

#### 配置

打开以下链接下载并安装。Mac也可直接通过Homebrew安装，但通过此方法安装的Tor并没有图形界面。

```
https://www.torproject.org/download/
```

也可通过发送标题为`help`的邮件到`gettor@torproject.org`获取。

安装完成后需要寻找支持HTTPS代理或Socks代理的代理，注意不能是HTTP代理。可以通过公共代理节点，也可通过在本地挂翻墙软件实现，大部分翻墙软件都可以与Tor组合。打开翻墙软件后，从配置中查看代理类型、代理地址和端口，比如SOCKS5代理的`localhost:1081`。

注意，GoAgent是Google Agent Engine（GAE）翻墙工具的一种，似乎只提供HTTP代理，没有提供原生的HTTPS代理，因此不能用作Tor的代理节点。

打开Tor后点击配置，勾选`我所在的国家对Tor进行了审查`，并选择`选择内置网桥`。勾选`使用代理访问互联网`，填入上面查询的内容，然后连接即可。

#### 验证

可通过以下链接判断当前是否在使用Tor。

```
https://check.torproject.org/
```

#### 屏蔽节点

为避免蜜罐节点，可通过修改Tor的配置文件，规避特定地区的节点。打开Tor的配置文件torrc，添加以下内容。其中ExcludeNodes表示排除这些地区的节点，StrictNodes表示强制执行。

如果不设置strictnode 1，Tor首先也会规避ExcludeNodes列出的这些地区，但如果Tor找不到可用的线路，就会去尝试位于排除列表中的节点。如果设置了strictnode 1，即使Tor找不到可用的线路，也不会去尝试这些地区的节点。

```
ExcludeNodes  {cn},{hk},{mo}
StrictNodes  1
```

其余代码如下，可根据需求自行获取。

```
北朝鲜 / {kp}
伊朗 / {ir}
叙利亚 / {sy}
巴基斯坦 / {pk}
古巴 / {cu}
越南 / {vn}
```

#### 共享Tor通道

打开Tor的配置文件torrc，新增以下内容。

```
SocksListenAddress 0.0.0.0:9150
// SOCKSPort  0.0.0.0:9150
```

重启Tor后，Tor的监听端口将绑定到0.0.0.0，即任何地址（任何机器）都可以连接到Tor 的监听端口。可能需要修改防火墙配置，允许Tor监听端口的TCP连入。

#### 特殊说明

Tor可以打开特有的onion域名链接，导航如下。注意抵制诱惑。

```
https://thehiddenwiki.org/
```

## ZeroNet

也称零网，利用比特币加密和BT技术提供不受审查的网络与通信的BT平台。ZeroNet默认不匿名，用户可以通过内置的Tor功能进行匿名化。ZeroNet需要全局翻墙才能使用。

打开以下链接并下载软件包，双击打开即可。

```
https://zeronet.io/
```

### ZeroMe

打开后点击ZeroMe，点击右上角允许请求权限，进入请求认证证书界面，输入用户名后即可授权成功。点击`在用户数据库中搜索`可查看已注册用户并拉入黑名单，点击右上角的`0`图标可回到控制台首页，在设置中也可管理屏蔽用户。再次进入ZeroMe，选择已注册好的账户并下载相关组件，完成之后加入即可。

### 备份用户数据

若将Zeronet整个文件夹删除，重新载入则失去对当前账户的所有权且无法找回，因此需备份users.json文件。

重新载入时，打开ZeroNet让其生成data文件夹，复制已备份好的users.json文件放置在data文件夹。重启ZeroNet并打开ZeroMe，下载完数据库点击授权，再重启ZeroNet打开ZeroMe即可。

### 网址导航

若遇到site Blocked或disable proxy，将`www.zerogate.tk`换成127.0.0.1:43110即可。

```
// 零搜索
https://www.zerogate.tk/lingdu.bit

// 零123导航
https://www.zerogate.tk/0123.bit

// 海盗湾种子站
http://127.0.0.1:43110/1PLAYgDQboKojowD3kwdb3CtWmWaokXvfp/

// Kindle电子书
http://127.0.0.1:43110/1KHCBG6dmbKXTZNenfwhWZ5x3oDyYyHSD4

// 中文主题
http://127.0.0.1:43110/1NzWeweqJ32aRVdM5UzFnYCszuvG5xV3vS
```

# 附录

## 通过信用卡验证

使用VPS服务需要VISA或Mastercard的卡，不同VPS供应商的验证标准不同。

### 免费虚拟卡

#### 香港全球付预付卡（MasterCard）

可通过2018年GCP验证，不可通过2019年及以后的GCP验证。

```
https://www.globalcash.hk/v4/
```

#### 香港拍住赏预付卡（MasterCard）

需要香港号码，在App Store下载`拍住赏`即可。

#### Yandex Money（MasterCard）

提供免费万事达虚拟信用卡，一年有效期。GCP、DO、Vultr均无法通过验证。

```
https://money.yandex.ru/
```

#### Uquid（VISA）

未验证。

```
https://uquid.com/uquid-card
```

### 付费虚拟卡

#### EastPay

```
https://card.easypayx.com/card/cards
```

### 实体卡

#### 香港neat借记卡（MasterCard）

激活后若超过半年未使用，将从第七个月开始收取休眠费。

从App Store下载`neat`，把手机语言改成英语。打开APP并注册，添加收卡地址时会提示只能寄到商业地址，且用顺丰快递收取36到43人民币的邮费，但实际是可以寄到私人地址的，且通过Hong Kong post免费邮寄。

#### 中国银行长城跨境通单标储蓄卡（Visa/MasterCard）

可通过2019年GCP验证。

#### 爱汇国际旅支卡（MasterCard）

用微信打开以下链接，点击`我要旅支卡`，跳转后点击添加新卡即可。

```
https://www.ihui.com/alliance/share/43cc72a60e694fdeb775d9566c04e4fc.html
```

## 获取美国号码

获取美国号码后可以很方便地通过VPS的验证。

### 虚拟号码

免费号码可使用TextNow、TextPlus、HeyWire、Ring4、TalkU、TextMe、TextFree、TextNow、Talkatone、Dingtone、Pinger等。

### Google Voice

包括以上的虚拟号码基本已无法通过Google Voice验证。

#### 注册

##### 失效教程

<details>
<summary></summary>

全程挂美国全局代理，浏览器用无痕模式。通过以下付费接码网站接收Google Voice验证码，购买时类型选择Google Voice。

```
// verfirywithsms
https://verifywithsms.com/


// PVA Deals，购买Non VolP Numbers
https://pvadeals.com/product/non-voip/
```
</details>

#### 保号

如果超过六个月没有拨打或者接听电话，也没有发出或接收过短信，号码就会被回收。可利用IFTTT自动接收和发送短信以保号。

##### 自动发送短信

注册IFTTT账号后打开以下链接，分别设置时区和Google Voice号码。

```
https://ifttt.com/services/date_and_time/settings
https://ifttt.com/sms
```

打开以下链接以新建一个Applet。

```
https://ifttt.com/create/
```

点If This后的Add，并选择Date&Time。有多种触发条件，根据自己的实际需求选择。再点Then that后的Add，选择SMS，并自定义短信内容。创建完打开该Applet即可。

##### 自动回复短信

登录Google Voice，进入Settings—>Settings—>Forward messages to email，打开将短信转发到Gmail邮箱。完成后Google Voice收到的短信都会以邮件的形式发送到Gmail邮箱，邮件标题为`New text message from [发送者]`，发件人是后缀为`@txt.voice.google.com`的邮箱，前缀里包含短信发送方和接收方的号码。

仍打开上面的链接新建一个Applet，If this添加Gmail，并选择`[Inactive] New email in inbox from search`，内容填写`txt.voice.google.com`。然后Then that添加Gmail，并选择`Send an email`，按照以下内容配置，然后打开该Applet即可。

```
To address / 点Add ingredient，选FromAddress
Body / 短信内容
Attachment URL / 清空
```

## 脚本

### 性能测试

```
wget -qO- 86.re/bench.sh | bash
wget -qO- --no-check-certificate https://raw.githubusercontent.com/oooldking/script/master/superbench.sh | bash
wget https://raw.githubusercontent.com/oooldking/script/master/superspeed.sh && chmod +x superspeed.sh && ./superspeed.sh
```

### Socat

一个多功能的网络工具。

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/socat.sh && chmod +x socat.sh && bash socat.sh
```

### HaProxy

负载均衡，与Nginx类似。

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/haproxy.sh && chmod +x haproxy.sh && bash haproxy.sh
```

### iptables

端口转发。

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/iptables-pf.sh && chmod +x iptables-pf.sh && bash iptables-pf.sh
```

### SimpleHTTPServer

快速搭建HTTP服务。

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/pythonhttp.sh && chmod +x pythonhttp.sh && bash pythonhttp.sh
```

### V2Ray安装

复制以下代码到文本编辑器并另存为go.sh，放在工作目录即可使用。

```
#!/bin/bash

# This file is accessible as https://install.direct/go.sh
# Original source is located at github.com/v2ray/v2ray-core/release/install-release.sh

# If not specify, default meaning of return value:
# 0: Success
# 1: System error
# 2: Application error
# 3: Network error

CUR_VER=""
NEW_VER=""
ARCH=""
VDIS="64"
ZIPFILE="/tmp/v2ray/v2ray.zip"
V2RAY_RUNNING=0
VSRC_ROOT="/tmp/v2ray"
EXTRACT_ONLY=0
ERROR_IF_UPTODATE=0
DIST_SRC="github"

CMD_INSTALL=""
CMD_UPDATE=""
SOFTWARE_UPDATED=0

SYSTEMCTL_CMD=$(command -v systemctl 2>/dev/null)
SERVICE_CMD=$(command -v service 2>/dev/null)

CHECK=""
FORCE=""
HELP=""

#######color code########
RED="31m"      # Error message
GREEN="32m"    # Success message
YELLOW="33m"   # Warning message
BLUE="36m"     # Info message


#########################
while [[ $# > 0 ]];do
    key="$1"
    case $key in
        -p|--proxy)
        PROXY="-x ${2}"
        shift # past argument
        ;;
        -h|--help)
        HELP="1"
        ;;
        -f|--force)
        FORCE="1"
        ;;
        -c|--check)
        CHECK="1"
        ;;
        --remove)
        REMOVE="1"
        ;;
        --version)
        VERSION="$2"
        shift
        ;;
        --extract)
        VSRC_ROOT="$2"
        shift
        ;;
        --extractonly)
        EXTRACT_ONLY="1"
        ;;
        -l|--local)
        LOCAL="$2"
        LOCAL_INSTALL="1"
        shift
        ;;
        --source)
        DIST_SRC="$2"
        shift
        ;;
        --errifuptodate)
        ERROR_IF_UPTODATE="1"
        ;;
        *)
                # unknown option
        ;;
    esac
    shift # past argument or value
done

###############################
colorEcho(){
    COLOR=$1
    echo -e "\033[${COLOR}${@:2}\033[0m"
}

sysArch(){
    ARCH=$(uname -m)
    if [[ "$ARCH" == "i686" ]] || [[ "$ARCH" == "i386" ]]; then
        VDIS="32"
    elif [[ "$ARCH" == *"armv7"* ]] || [[ "$ARCH" == "armv6l" ]]; then
        VDIS="arm"
    elif [[ "$ARCH" == *"armv8"* ]] || [[ "$ARCH" == "aarch64" ]]; then
        VDIS="arm64"
    elif [[ "$ARCH" == *"mips64le"* ]]; then
        VDIS="mips64le"
    elif [[ "$ARCH" == *"mips64"* ]]; then
        VDIS="mips64"
    elif [[ "$ARCH" == *"mipsle"* ]]; then
        VDIS="mipsle"
    elif [[ "$ARCH" == *"mips"* ]]; then
        VDIS="mips"
    elif [[ "$ARCH" == *"s390x"* ]]; then
        VDIS="s390x"
    elif [[ "$ARCH" == "ppc64le" ]]; then
        VDIS="ppc64le"
    elif [[ "$ARCH" == "ppc64" ]]; then
        VDIS="ppc64"
    fi
    return 0
}

downloadV2Ray(){
    return 0
}

installSoftware(){
    COMPONENT=$1
    if [[ -n `command -v $COMPONENT` ]]; then
        return 0
    fi

    getPMT
    if [[ $? -eq 1 ]]; then
        colorEcho ${RED} "The system package manager tool isn't APT or YUM, please install ${COMPONENT} manually."
        return 1 
    fi
    if [[ $SOFTWARE_UPDATED -eq 0 ]]; then
        colorEcho ${BLUE} "Updating software repo"
        $CMD_UPDATE      
        SOFTWARE_UPDATED=1
    fi

    colorEcho ${BLUE} "Installing ${COMPONENT}"
    $CMD_INSTALL $COMPONENT
    if [[ $? -ne 0 ]]; then
        colorEcho ${RED} "Failed to install ${COMPONENT}. Please install it manually."
        return 1
    fi
    return 0
}

# return 1: not apt, yum, or zypper
getPMT(){
    if [[ -n `command -v apt-get` ]];then
        CMD_INSTALL="apt-get -y -qq install"
        CMD_UPDATE="apt-get -qq update"
    elif [[ -n `command -v yum` ]]; then
        CMD_INSTALL="yum -y -q install"
        CMD_UPDATE="yum -q makecache"
    elif [[ -n `command -v zypper` ]]; then
        CMD_INSTALL="zypper -y install"
        CMD_UPDATE="zypper ref"
    else
        return 1
    fi
    return 0
}

extract(){
    colorEcho ${BLUE}"Extracting V2Ray package to /tmp/v2ray."
    mkdir -p /tmp/v2ray
    unzip $1 -d ${VSRC_ROOT}
    if [[ $? -ne 0 ]]; then
        colorEcho ${RED} "Failed to extract V2Ray."
        return 2
    fi
    if [[ -d "/tmp/v2ray/v2ray-${NEW_VER}-linux-${VDIS}" ]]; then
      VSRC_ROOT="/tmp/v2ray/v2ray-${NEW_VER}-linux-${VDIS}"
    fi
    return 0
}


# 1: new V2Ray. 0: no. 2: not installed. 3: check failed. 4: don't check.
getVersion(){
    if [[ -n "$VERSION" ]]; then
        NEW_VER="$VERSION"
        if [[ ${NEW_VER} != v* ]]; then
          NEW_VER=v${NEW_VER}
        fi
        return 4
    else
        VER=`/usr/bin/v2ray/v2ray -version 2>/dev/null`
        RETVAL="$?"
        CUR_VER=`echo $VER | head -n 1 | cut -d " " -f2`
        if [[ ${CUR_VER} != v* ]]; then
            CUR_VER=v${CUR_VER}
        fi
        TAG_URL="https://api.github.com/repos/v2ray/v2ray-core/releases/latest"
        NEW_VER=`curl ${PROXY} -s ${TAG_URL} --connect-timeout 10| grep 'tag_name' | cut -d\" -f4`
        if [[ ${NEW_VER} != v* ]]; then
          NEW_VER=v${NEW_VER}
        fi
        if [[ $? -ne 0 ]] || [[ $NEW_VER == "" ]]; then
            colorEcho ${RED} "Failed to fetch release information. Please check your network or try again."
            return 3
        elif [[ $RETVAL -ne 0 ]];then
            return 2
        elif [[ `echo $NEW_VER | cut -d. -f-2` != `echo $CUR_VER | cut -d. -f-2` ]];then
            return 1
        fi
        return 0
    fi
}

stopV2ray(){
    colorEcho ${BLUE} "Shutting down V2Ray service."
    if [[ -n "${SYSTEMCTL_CMD}" ]] || [[ -f "/lib/systemd/system/v2ray.service" ]] || [[ -f "/etc/systemd/system/v2ray.service" ]]; then
        ${SYSTEMCTL_CMD} stop v2ray
    elif [[ -n "${SERVICE_CMD}" ]] || [[ -f "/etc/init.d/v2ray" ]]; then
        ${SERVICE_CMD} v2ray stop
    fi
    if [[ $? -ne 0 ]]; then
        colorEcho ${YELLOW} "Failed to shutdown V2Ray service."
        return 2
    fi
    return 0
}

startV2ray(){
    if [ -n "${SYSTEMCTL_CMD}" ] && [ -f "/lib/systemd/system/v2ray.service" ]; then
        ${SYSTEMCTL_CMD} start v2ray
    elif [ -n "${SYSTEMCTL_CMD}" ] && [ -f "/etc/systemd/system/v2ray.service" ]; then
        ${SYSTEMCTL_CMD} start v2ray
    elif [ -n "${SERVICE_CMD}" ] && [ -f "/etc/init.d/v2ray" ]; then
        ${SERVICE_CMD} v2ray start
    fi
    if [[ $? -ne 0 ]]; then
        colorEcho ${YELLOW} "Failed to start V2Ray service."
        return 2
    fi
    return 0
}

copyFile() {
    NAME=$1
    ERROR=`cp "${VSRC_ROOT}/${NAME}" "/usr/bin/v2ray/${NAME}" 2>&1`
    if [[ $? -ne 0 ]]; then
        colorEcho ${YELLOW} "${ERROR}"
        return 1
    fi
    return 0
}

makeExecutable() {
    chmod +x "/usr/bin/v2ray/$1"
}

installV2Ray(){
    # Install V2Ray binary to /usr/bin/v2ray
    mkdir -p /usr/bin/v2ray
    copyFile v2ray
    if [[ $? -ne 0 ]]; then
        colorEcho ${RED} "Failed to copy V2Ray binary and resources."
        return 1
    fi
    makeExecutable v2ray
    copyFile v2ctl && makeExecutable v2ctl
    copyFile geoip.dat
    copyFile geosite.dat

    # Install V2Ray server config to /etc/v2ray
    if [[ ! -f "/etc/v2ray/config.json" ]]; then
        mkdir -p /etc/v2ray
        mkdir -p /var/log/v2ray
        cp "${VSRC_ROOT}/vpoint_vmess_freedom.json" "/etc/v2ray/config.json"
        if [[ $? -ne 0 ]]; then
            colorEcho ${YELLOW} "Failed to create V2Ray configuration file. Please create it manually."
            return 1
        fi
        let PORT=$RANDOM+10000
        UUID=$(cat /proc/sys/kernel/random/uuid)

        sed -i "s/10086/${PORT}/g" "/etc/v2ray/config.json"
        sed -i "s/23ad6b10-8d1a-40f7-8ad0-e3e35cd38297/${UUID}/g" "/etc/v2ray/config.json"

        colorEcho ${BLUE} "PORT:${PORT}"
        colorEcho ${BLUE} "UUID:${UUID}"
    fi
    return 0
}


installInitScript(){
    if [[ -n "${SYSTEMCTL_CMD}" ]];then
        if [[ ! -f "/etc/systemd/system/v2ray.service" ]]; then
            if [[ ! -f "/lib/systemd/system/v2ray.service" ]]; then
                cp "${VSRC_ROOT}/systemd/v2ray.service" "/etc/systemd/system/"
                systemctl enable v2ray.service
            fi
        fi
        return
    elif [[ -n "${SERVICE_CMD}" ]] && [[ ! -f "/etc/init.d/v2ray" ]]; then
        installSoftware "daemon" || return $?
        cp "${VSRC_ROOT}/systemv/v2ray" "/etc/init.d/v2ray"
        chmod +x "/etc/init.d/v2ray"
        update-rc.d v2ray defaults
    fi
    return
}

Help(){
    echo "./install-release.sh [-h] [-c] [--remove] [-p proxy] [-f] [--version vx.y.z] [-l file]"
    echo "  -h, --help            Show help"
    echo "  -p, --proxy           To download through a proxy server, use -p socks5://127.0.0.1:1080 or -p http://127.0.0.1:3128 etc"
    echo "  -f, --force           Force install"
    echo "      --version         Install a particular version, use --version v3.15"
    echo "  -l, --local           Install from a local file"
    echo "      --remove          Remove installed V2Ray"
    echo "  -c, --check           Check for update"
    return 0
}

remove(){
    if [[ -n "${SYSTEMCTL_CMD}" ]] && [[ -f "/etc/systemd/system/v2ray.service" ]];then
        if pgrep "v2ray" > /dev/null ; then
            stopV2ray
        fi
        systemctl disable v2ray.service
        rm -rf "/usr/bin/v2ray" "/etc/systemd/system/v2ray.service"
        if [[ $? -ne 0 ]]; then
            colorEcho ${RED} "Failed to remove V2Ray."
            return 0
        else
            colorEcho ${GREEN} "Removed V2Ray successfully."
            colorEcho ${BLUE} "If necessary, please remove configuration file and log file manually."
            return 0
        fi
    elif [[ -n "${SYSTEMCTL_CMD}" ]] && [[ -f "/lib/systemd/system/v2ray.service" ]];then
        if pgrep "v2ray" > /dev/null ; then
            stopV2ray
        fi
        systemctl disable v2ray.service
        rm -rf "/usr/bin/v2ray" "/lib/systemd/system/v2ray.service"
        if [[ $? -ne 0 ]]; then
            colorEcho ${RED} "Failed to remove V2Ray."
            return 0
        else
            colorEcho ${GREEN} "Removed V2Ray successfully."
            colorEcho ${BLUE} "If necessary, please remove configuration file and log file manually."
            return 0
        fi
    elif [[ -n "${SERVICE_CMD}" ]] && [[ -f "/etc/init.d/v2ray" ]]; then
        if pgrep "v2ray" > /dev/null ; then
            stopV2ray
        fi
        rm -rf "/usr/bin/v2ray" "/etc/init.d/v2ray"
        if [[ $? -ne 0 ]]; then
            colorEcho ${RED} "Failed to remove V2Ray."
            return 0
        else
            colorEcho ${GREEN} "Removed V2Ray successfully."
            colorEcho ${BLUE} "If necessary, please remove configuration file and log file manually."
            return 0
        fi       
    else
        colorEcho ${YELLOW} "V2Ray not found."
        return 0
    fi
}

checkUpdate(){
    echo "Checking for update."
    VERSION=""
    getVersion
    RETVAL="$?"
    if [[ $RETVAL -eq 1 ]]; then
        colorEcho ${BLUE} "Found new version ${NEW_VER} for V2Ray.(Current version:$CUR_VER)"
    elif [[ $RETVAL -eq 0 ]]; then
        colorEcho ${BLUE} "No new version. Current version is ${NEW_VER}."
    elif [[ $RETVAL -eq 2 ]]; then
        colorEcho ${YELLOW} "No V2Ray installed."
        colorEcho ${BLUE} "The newest version for V2Ray is ${NEW_VER}."
    fi
    return 0
}

main(){
    #helping information
    [[ "$HELP" == "1" ]] && Help && return
    [[ "$CHECK" == "1" ]] && checkUpdate && return
    [[ "$REMOVE" == "1" ]] && remove && return

    sysArch
    # extract local file
    if [[ $LOCAL_INSTALL -eq 1 ]]; then
        colorEcho ${YELLOW} "Installing V2Ray via local file. Please make sure the file is a valid V2Ray package, as we are not able to determine that."
        NEW_VER=local
        installSoftware unzip || return $?
        rm -rf /tmp/v2ray
        extract $LOCAL || return $?
        #FILEVDIS=`ls /tmp/v2ray |grep v2ray-v |cut -d "-" -f4`
        #SYSTEM=`ls /tmp/v2ray |grep v2ray-v |cut -d "-" -f3`
        #if [[ ${SYSTEM} != "linux" ]]; then
        #    colorEcho ${RED} "The local V2Ray can not be installed in linux."
        #    return 1
        #elif [[ ${FILEVDIS} != ${VDIS} ]]; then
        #    colorEcho ${RED} "The local V2Ray can not be installed in ${ARCH} system."
        #    return 1
        #else
        #    NEW_VER=`ls /tmp/v2ray |grep v2ray-v |cut -d "-" -f2`
        #fi
    else
        # download via network and extract
        installSoftware "curl" || return $?
        getVersion
        RETVAL="$?"
        if [[ $RETVAL == 0 ]] && [[ "$FORCE" != "1" ]]; then
            colorEcho ${BLUE} "Latest version ${NEW_VER} is already installed."
            if [[ "${ERROR_IF_UPTODATE}" == "1" ]]; then
              return 10
            fi
            return
        elif [[ $RETVAL == 3 ]]; then
            return 3
        else
            colorEcho ${BLUE} "Installing V2Ray ${NEW_VER} on ${ARCH}"
            downloadV2Ray || return $?
            installSoftware unzip || return $?
            extract ${ZIPFILE} || return $?
        fi
    fi 

    if [[ "${EXTRACT_ONLY}" == "1" ]]; then
        colorEcho ${GREEN} "V2Ray extracted to ${VSRC_ROOT}, and exiting..."
        return 0
    fi

    if pgrep "v2ray" > /dev/null ; then
        V2RAY_RUNNING=1
        stopV2ray
    fi
    installV2Ray || return $?
    installInitScript || return $?
    if [[ ${V2RAY_RUNNING} -eq 1 ]];then
        colorEcho ${BLUE} "Restarting V2Ray service."
        startV2ray
    fi
    colorEcho ${GREEN} "V2Ray ${NEW_VER} is installed."
    rm -rf /tmp/v2ray
    return 0
}

main
```

### 系统重装

#### 原系统为Linux

##### Linux

```
#!/bin/bash
 
while [[ $# -ge 1 ]]; do
  case $1 in
    -v|--ver)
      shift
      VERtmp="$1"
      shift
      ;;
    -d|--debian|--ubuntu)
      shift
      vDEBtmp="$1"
      shift
      ;;
    -p|--password)
      shift
      WDtmp="$1"
      shift
      ;;
    -a|--auto)
      shift
      INStmp='auto'
      ;;
    -m|--manual)
      shift
      INStmp='manual'
      ;;
    -apt|--mirror)
      shift
      isMirror='1'
      tmpMirror="$1"
      shift
      ;;
    *)
      echo -ne " Usage:\n\tbash $0\t-d/--debian [7/\033[33m\033[04mwheezy\033[0m|8/jessie|9/stretch]\n\t\t\t\t-v/--ver [32/\033[33m\033[04mi386\033[0m|64/amd64]\n\t\t\t\t-apt/--mirror\n\t\t\t\t-a/--auto\n\t\t\t\t-m/--manual\n"
      exit 1;
      ;;
    esac
  done
 
[ $EUID -ne 0 ] && echo "Error:This script must be run as root!" && exit 1
[ -f /boot/grub/grub.cfg ] && GRUBOLD='0' && GRUBDIR='/boot/grub' && GRUBFILE='grub.cfg'
[ -z $GRUBDIR ] && [ -f /boot/grub2/grub.cfg ] && GRUBOLD='0' && GRUBDIR='/boot/grub2' && GRUBFILE='grub.cfg'
[ -z $GRUBDIR ] && [ -f /boot/grub/grub.conf ] && GRUBOLD='1' && GRUBDIR='/boot/grub' && GRUBFILE='grub.conf'
[ -z $GRUBDIR -o -z $GRUBFILE ] && echo "Error! Not Found grub path." && exit 1
 
[ -n $vDEBtmp ] && {
[ "$vDEBtmp" == '7' -o "$vDEBtmp" == 'wheezy' ] && linuxdists='debian' && vDEB='wheezy';
[ "$vDEBtmp" == '8' -o "$vDEBtmp" == 'jessie' ] && linuxdists='debian' && vDEB='jessie';
[ "$vDEBtmp" == '9' -o "$vDEBtmp" == 'stretch' ] && linuxdists='debian' && vDEB='stretch';
[ "$vDEBtmp" == 'precise' ] && linuxdists='ubuntu' && vDEB='precise';
[ "$vDEBtmp" == 'trusty' ] && linuxdists='ubuntu' && vDEB='trusty';
[ "$vDEBtmp" == 'wily' ] && linuxdists='ubuntu' && vDEB='wily';
[ "$vDEBtmp" == 'xenial' ] && linuxdists='ubuntu' && vDEB='xenial';
[ "$vDEBtmp" == 'yakkety' ] && linuxdists='ubuntu' && vDEB='yakkety';
[ "$vDEBtmp" == 'zesty' ] && linuxdists='ubuntu' && vDEB='zesty';
}
[ -n $vDEBtmp ] && {
[ "$VERtmp" == '32' -o "$VERtmp" == 'i386' ] && VER='i386';
[ "$VERtmp" == '64' -o "$VERtmp" == 'amd64' ] && VER='amd64';
}
 
[ -z $linuxdists ] && linuxdists='debian'
[ -n $isMirror ] && [ "$isMirror" == '1' ] && [ -n $tmpMirror ] && {
tmpDebianMirror="$(echo -n "$tmpMirror" |grep -Eo '.*\.(\w+)')"
echo -n "$tmpDebianMirror" |grep -q '://'
[ $? -eq '0' ] && {
DebianMirror="$(echo -n "$tmpDebianMirror" |awk -F'://' '{print $2}')"
} || {
DebianMirror="$(echo -n "$tmpDebianMirror")"
}
} || {
[[ $linuxdists == 'debian' ]] && DebianMirror='httpredir.debian.org'
[[ $linuxdists == 'ubuntu' ]] && DebianMirror='archive.ubuntu.com'
}
[ -z $DebianMirrorDirectory ] && [ -n $DebianMirror ] && [ -n $tmpMirror ] && {
DebianMirrorDirectory="$(echo -n "$tmpMirror" |awk -F''${DebianMirror}'' '{print $2}' |sed 's/\/$//g')"
}
[ "$DebianMirrorDirectory" == '/' ] && [ -n $DebianMirror ] && {
[[ $linuxdists == 'debian' ]] && DebianMirrorDirectory='/debian'
[[ $linuxdists == 'ubuntu' ]] && DebianMirrorDirectory='/ubuntu'
}
[ -z $DebianMirrorDirectory ] && [ -n $DebianMirror ] && {
[[ $linuxdists == 'debian' ]] && DebianMirrorDirectory='/debian'
[[ $linuxdists == 'ubuntu' ]] && DebianMirrorDirectory='/ubuntu'
}
 
[ -n $INStmp ] && {
[ "$INStmp" == 'auto' ] && inVNC='n'
[ "$INStmp" == 'manual' ] && inVNC='y'
}
[ -n $WDtmp ] && myPASSWORD="$WDtmp"
 
[ -z $vDEB ] && vDEB='wheezy';
[ -z $VER ] && VER='i386';
[ -z $myPASSWORD ] && myPASSWORD='Vicer'
 
clear && echo -e "\n\033[36m# Install\033[0m\n"
 
[ -z $inVNC ] && ASKVNC(){
inVNC='y';
echo -ne "\033[34mCan you login VNC?\033[0m\e[33m[\e[32my\e[33m/n]\e[0m "
read inVNCtmp
[[ -n "$inVNCtmp" ]] && inVNC=$inVNCtmp
[ "$inVNC" == 'y' -o "$inVNC" == 'Y' ] && inVNC='y'
[ "$inVNC" == 'n' -o "$inVNC" == 'N' ] && inVNC='n'
}
 
[ "$inVNC" == 'y' -o "$inVNC" == 'n' ] || ASKVNC;
 
[[ $linuxdists == 'debian' ]] && LinuxName='Debian'
[[ $linuxdists == 'ubuntu' ]] && LinuxName='Ubuntu'
[ "$inVNC" == 'y' ] && echo -e "\033[34mManual Mode\033[0m insatll \033[33m$LinuxName\033[0m [\033[33m$vDEB\033[0m] [\033[33m$VER\033[0m] in VNC. "
[ "$inVNC" == 'n' ] && echo -e "\033[34mAuto Mode\033[0m insatll \033[33m$LinuxName\033[0m [\033[33m$vDEB\033[0m] [\033[33m$VER\033[0m]. "
 
echo -e "\n[\033[33m$vDEB\033[0m] [\033[33m$VER\033[0m] Downloading..."
[ -z $DebianMirror ] && echo -ne "\033[31mError! \033[0mGet debian mirror fail! \n" && exit 1
[ -z $DebianMirrorDirectory ] && echo -ne "\033[31mError! \033[0mGet debian mirror directory fail! \n" && exit 1
wget --no-check-certificate -qO '/boot/initrd.gz' "http://$DebianMirror$DebianMirrorDirectory/dists/$vDEB/main/installer-$VER/current/images/netboot/$linuxdists-installer/$VER/initrd.gz"
[ $? -ne '0' ] && echo -ne "\033[31mError! \033[0mDownload 'initrd.gz' failed! \n" && exit 1
wget --no-check-certificate -qO '/boot/linux' "http://$DebianMirror$DebianMirrorDirectory/dists/$vDEB/main/installer-$VER/current/images/netboot/$linuxdists-installer/$VER/linux"
[ $? -ne '0' ] && echo -ne "\033[31mError! \033[0mDownload 'linux' failed! \n" && exit 1
 
DEFAULTNET="$(ip route show |grep -o 'default via [0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.*' |head -n1 |sed 's/proto.*\|onlink.*//g' |awk '{print $NF}')"
[ -n "$DEFAULTNET" ] && IPSUB="$(ip addr |grep ''${DEFAULTNET}'' |grep 'global' |grep 'brd' |head -n1 |grep -o '[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}/[0-9]\{1,2\}')"
IPv4="$(echo -n "$IPSUB" |cut -d'/' -f1)"
NETSUB="$(echo -n "$IPSUB" |grep -o '/[0-9]\{1,2\}')"
GATE="$(ip route show |grep -o 'default via [0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}' |head -n1 |grep -o '[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}')"
[ -n "$NETSUB" ] && MASK="$(echo -n '128.0.0.0/1,192.0.0.0/2,224.0.0.0/3,240.0.0.0/4,248.0.0.0/5,252.0.0.0/6,254.0.0.0/7,255.0.0.0/8,255.128.0.0/9,255.192.0.0/10,255.224.0.0/11,255.240.0.0/12,255.248.0.0/13,255.252.0.0/14,255.254.0.0/15,255.255.0.0/16,255.255.128.0/17,255.255.192.0/18,255.255.224.0/19,255.255.240.0/20,255.255.248.0/21,255.255.252.0/22,255.255.254.0/23,255.255.255.0/24,255.255.255.128/25,255.255.255.192/26,255.255.255.224/27,255.255.255.240/28,255.255.255.248/29,255.255.255.252/30,255.255.255.254/31,255.255.255.255/32' |grep -o '[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}'${NETSUB}'' |cut -d'/' -f1)"
 
[ -n "$GATE" ] && [ -n "$MASK" ] && [ -n "$IPv4" ] || {
echo "Not found `ip command`, It will use `route command`."
ipNum() {
  local IFS='.'
  read ip1 ip2 ip3 ip4 <<<"$1"
  echo $((ip1*(1<<24)+ip2*(1<<16)+ip3*(1<<8)+ip4))
}
 
SelectMax(){
ii=0
for IPITEM in `route -n |awk -v OUT=$1 '{print $OUT}' |grep '[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}'`
  do
    NumTMP="$(ipNum $IPITEM)"
    eval "arrayNum[$ii]='$NumTMP,$IPITEM'"
    ii=$[$ii+1]
  done
echo ${arrayNum[@]} |sed 's/\s/\n/g' |sort -n -k 1 -t ',' |tail -n1 |cut -d',' -f2
}
 
[[ -z $IPv4 ]] && IPv4="$(ifconfig |grep 'Bcast' |head -n1 |grep -o '[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}' |head -n1)"
[[ -z $GATE ]] && GATE="$(SelectMax 2)"
[[ -z $MASK ]] && MASK="$(SelectMax 3)"
 
[ -n "$GATE" ] && [ -n "$MASK" ] && [ -n "$IPv4" ] || {
echo "Error! Not configure network. "
exit 1
}
}
 
[ -f /etc/network/interfaces ] && {
[[ -z "$(sed -n '/iface.*inet static/p' /etc/network/interfaces)" ]] && AutoNet='1' || AutoNet='0'
[ -d /etc/network/interfaces.d ] && {
ICFGN="$(find /etc/network/interfaces.d -name '*.cfg' |wc -l)" || ICFGN='0'
[ "$ICFGN" -ne '0' ] && {
for NetCFG in `ls -1 /etc/network/interfaces.d/*.cfg`
 do 
  [[ -z "$(cat $NetCFG | sed -n '/iface.*inet static/p')" ]] && AutoNet='1' || AutoNet='0'
  [ "$AutoNet" -eq '0' ] && break
done
}
}
}
[ -d /etc/sysconfig/network-scripts ] && {
ICFGN="$(find /etc/sysconfig/network-scripts -name 'ifcfg-*' |grep -v 'lo'|wc -l)" || ICFGN='0'
[ "$ICFGN" -ne '0' ] && {
for NetCFG in `ls -1 /etc/sysconfig/network-scripts/ifcfg-* |grep -v 'lo$' |grep -v ':[0-9]\{1,\}'`
 do 
  [[ -n "$(cat $NetCFG | sed -n '/BOOTPROTO.*[dD][hH][cC][pP]/p')" ]] && AutoNet='1' || {
  AutoNet='0' && . $NetCFG
  [ -n $NETMASK ] && MASK="$NETMASK"
  [ -n $GATEWAY ] && GATE="$GATEWAY"
}
  [ "$AutoNet" -eq '0' ] && break
done
}
}
 
[ ! -f $GRUBDIR/$GRUBFILE ] && echo "Error! Not Found $GRUBFILE. " && exit 1
 
[ ! -f $GRUBDIR/$GRUBFILE.old ] && [ -f $GRUBDIR/$GRUBFILE.bak ] && mv -f $GRUBDIR/$GRUBFILE.bak $GRUBDIR/$GRUBFILE.old
mv -f $GRUBDIR/$GRUBFILE $GRUBDIR/$GRUBFILE.bak
[ -f $GRUBDIR/$GRUBFILE.old ] && cat $GRUBDIR/$GRUBFILE.old >$GRUBDIR/$GRUBFILE || cat $GRUBDIR/$GRUBFILE.bak >$GRUBDIR/$GRUBFILE
 
[ "$GRUBOLD" == '0' ] && {
CFG0="$(awk '/menuentry /{print NR}' $GRUBDIR/$GRUBFILE|head -n 1)"
CFG2="$(awk '/menuentry /{print NR}' $GRUBDIR/$GRUBFILE|head -n 2 |tail -n 1)"
CFG1=""
for CFGtmp in `awk '/}/{print NR}' $GRUBDIR/$GRUBFILE`
 do
  [ $CFGtmp -gt "$CFG0" -a $CFGtmp -lt "$CFG2" ] && CFG1="$CFGtmp";
 done
[ -z "$CFG1" ] && {
echo "Error! read $GRUBFILE. "
exit 1
}
sed -n "$CFG0,$CFG1"p $GRUBDIR/$GRUBFILE >/tmp/grub.new
[ -f /tmp/grub.new ] && [ "$(grep -c '{' /tmp/grub.new)" -eq "$(grep -c '}' /tmp/grub.new)" ] || {
echo -ne "\033[31mError! \033[0mNot configure $GRUBFILE. \n"
exit 1
}
 
sed -i "/menuentry.*/c\menuentry\ \'Install OS \[$vDEB\ $VER\]\'\ --class debian\ --class\ gnu-linux\ --class\ gnu\ --class\ os\ \{" /tmp/grub.new
[ "$(grep -c '{' /tmp/grub.new)" -eq "$(grep -c '}' /tmp/grub.new)" ] || {
echo "Error! configure append $GRUBFILE. "
exit 1
}
sed -i "/echo.*Loading/d" /tmp/grub.new
}
 
[ "$GRUBOLD" == '1' ] && {
CFG0="$(awk '/title /{print NR}' $GRUBDIR/$GRUBFILE|head -n 1)"
CFG1="$(awk '/title /{print NR}' $GRUBDIR/$GRUBFILE|head -n 2 |tail -n 1)"
[ -n $CFG0 ] && [ -z $CFG1 -o $CFG1 == $CFG0 ] && sed -n "$CFG0,$"p $GRUBDIR/$GRUBFILE >/tmp/grub.new
[ -n $CFG0 ] && [ -z $CFG1 -o $CFG1 != $CFG0 ] && sed -n "$CFG0,$CFG1"p $GRUBDIR/$GRUBFILE >/tmp/grub.new
[ ! -f /tmp/grub.new ] && echo "Error! configure append $GRUBFILE. " && exit 1
sed -i "/title.*/c\title\ \'Install OS \[$vDEB\ $VER\]\'" /tmp/grub.new
sed -i '/^#/d' /tmp/grub.new
}
 
[ -n "$(grep 'initrd.*/' /tmp/grub.new |awk '{print $2}' |tail -n 1 |grep '^/boot/')" ] && Type='InBoot' || Type='NoBoot'
 
LinuxKernel="$(grep 'linux.*/' /tmp/grub.new |awk '{print $1}' |head -n 1)"
[ -z $LinuxKernel ] && LinuxKernel="$(grep 'kernel.*/' /tmp/grub.new |awk '{print $1}' |head -n 1)"
LinuxIMG="$(grep 'initrd.*/' /tmp/grub.new |awk '{print $1}' |tail -n 1)"
 
[ "$Type" == 'InBoot' ] && {
sed -i "/$LinuxKernel.*\//c\\\t$LinuxKernel\\t\/boot\/linux auto=true hostname=$linuxdists domain= -- quiet" /tmp/grub.new
sed -i "/$LinuxIMG.*\//c\\\t$LinuxIMG\\t\/boot\/initrd.gz" /tmp/grub.new
}
 
[ "$Type" == 'NoBoot' ] && {
sed -i "/$LinuxKernel.*\//c\\\t$LinuxKernel\\t\/linux auto=true hostname=$linuxdists domain= -- quiet" /tmp/grub.new
sed -i "/$LinuxIMG.*\//c\\\t$LinuxIMG\\t\/initrd.gz" /tmp/grub.new
}
 
sed -i '$a\\n' /tmp/grub.new
 
[ "$inVNC" == 'n' ] && {
GRUBPATCH='0'
[ -f /etc/network/interfaces -o -d /etc/sysconfig/network-scripts ] && {
sed -i ''${CFG0}'i\\n' $GRUBDIR/$GRUBFILE
sed -i ''${CFG0}'r /tmp/grub.new' $GRUBDIR/$GRUBFILE
[ -z $AutoNet ] && echo "Error, Not found interfaces config." && exit 1
[ -f  $GRUBDIR/grubenv ] && sed -i 's/saved_entry/#saved_entry/g' $GRUBDIR/grubenv
[ -d /boot/tmp ] && rm -rf /boot/tmp
mkdir -p /boot/tmp/
cd /boot/tmp/
gzip -d < ../initrd.gz | cpio --extract --verbose --make-directories --no-absolute-filenames >>/dev/null 2>&1
cat >/boot/tmp/preseed.cfg<<EOF
d-i debian-installer/locale string en_US
d-i console-setup/layoutcode string us
 
d-i keyboard-configuration/xkb-keymap string us
 
d-i netcfg/choose_interface select auto
 
d-i netcfg/disable_autoconfig boolean true
d-i netcfg/dhcp_failed note
d-i netcfg/dhcp_options select Configure network manually
d-i netcfg/get_ipaddress string $IPv4
d-i netcfg/get_netmask string $MASK
d-i netcfg/get_gateway string $GATE
d-i netcfg/get_nameservers string 8.8.8.8
d-i netcfg/no_default_route boolean true
d-i netcfg/confirm_static boolean true
 
d-i mirror/country string manual
d-i mirror/http/hostname string $DebianMirror
d-i mirror/http/directory string $DebianMirrorDirectory
d-i mirror/http/proxy string
 
d-i passwd/root-login boolean ture
d-i passwd/make-user boolean false
d-i passwd/root-password password $myPASSWORD
d-i passwd/root-password-again password $myPASSWORD
d-i user-setup/allow-password-weak boolean true
d-i user-setup/encrypt-home boolean false
 
d-i clock-setup/utc boolean true
d-i time/zone string US/Eastern
d-i clock-setup/ntp boolean true
 
d-i partman/early_command string \
debconf-set partman-auto/disk "\$(list-devices disk |head -n1)"; \
debconf-set grub-installer/bootdev string "\$(list-devices disk |head -n1)"; \
umount /media || true;
d-i partman/mount_style select uuid
d-i partman-auto/init_automatically_partition select Guided - use entire disk
d-i partman-auto/method string regular
d-i partman-lvm/device_remove_lvm boolean true
d-i partman-md/device_remove_md boolean true
d-i partman-auto/choose_recipe select atomic
d-i partman-partitioning/confirm_write_new_label boolean true
d-i partman/choose_partition select finish
d-i partman-lvm/confirm boolean true
d-i partman-lvm/confirm_nooverwrite boolean true
d-i partman/confirm boolean true
d-i partman/confirm_nooverwrite boolean true
 
d-i debian-installer/allow_unauthenticated boolean true
 
tasksel tasksel/first multiselect minimal
d-i pkgsel/update-policy select none
d-i pkgsel/include string openssh-server
d-i pkgsel/upgrade select none
 
popularity-contest popularity-contest/participate boolean false
 
d-i grub-installer/only_debian boolean true
d-i grub-installer/bootdev string default
d-i finish-install/reboot_in_progress note
d-i debian-installer/exit/reboot boolean true
d-i preseed/late_command string \
sed -i 's/^.*PermitRootLogin.*/PermitRootLogin yes/g' /target/etc/ssh/sshd_config; \
sed -i 's/^.*PasswordAuthentication.*/PasswordAuthentication yes/g' /target/etc/ssh/sshd_config;
EOF
[ "$AutoNet" -eq '1' ] && {
sed -i '/netcfg\/disable_autoconfig/d' /boot/tmp/preseed.cfg
sed -i '/netcfg\/dhcp_options/d' /boot/tmp/preseed.cfg
sed -i '/netcfg\/get_.*/d' /boot/tmp/preseed.cfg
sed -i '/netcfg\/confirm_static/d' /boot/tmp/preseed.cfg
}
[ "$vDEB" == 'trusty' ] && GRUBPATCH='1'
[ "$vDEB" == 'wily' ] && GRUBPATCH='1'
[ "$GRUBPATCH" == '1' ] && {
sed -i 's/^d-i\ grub-installer\/bootdev\ string\ default//g' /boot/tmp/preseed.cfg
}
[ "$GRUBPATCH" == '0' ] && {
sed -i 's/debconf-set\ grub-installer\/bootdev.*\"\;//g' /boot/tmp/preseed.cfg
}
[ "$linuxdists" == 'debian' ] && {
sed -i '/user-setup\/allow-password-weak/d' /boot/tmp/preseed.cfg
sed -i '/user-setup\/encrypt-home/d' /boot/tmp/preseed.cfg
sed -i '/pkgsel\/update-policy/d' /boot/tmp/preseed.cfg
sed -i 's/umount\ \/media.*\;//g' /boot/tmp/preseed.cfg
}
rm -rf ../initrd.gz
find . | cpio -H newc --create --verbose | gzip -9 > ../initrd.gz
rm -rf /boot/tmp
}
}
 
[ "$inVNC" == 'y' ] && {
sed -i '$i\\n' $GRUBDIR/$GRUBFILE
sed -i '$r /tmp/grub.new' $GRUBDIR/$GRUBFILE
echo -e "\n\033[33m\033[04mIt will reboot! \nPlease look at VNC! \nSelect\033[0m\033[32m Install OS [$vDEB $VER] \033[33m\033[4mto install system.\033[04m\n\n\033[31m\033[04mThere is some information for you.\nDO NOT CLOSE THE WINDOW! \033[0m\n"
echo -e "\033[35mIPv4\t\tNETMASK\t\tGATEWAY\033[0m"
echo -e "\033[36m\033[04m$IPv4\033[0m\t\033[36m\033[04m$MASK\033[0m\t\033[36m\033[04m$GATE\033[0m\n\n"
 
read -n 1 -p "Press Enter to reboot..." INP
if [ "$INP" != '' ] ; then
echo -ne '\b \n'
echo "";
fi
}
 
chown root:root $GRUBDIR/$GRUBFILE
chmod 444 $GRUBDIR/$GRUBFILE
 
sleep 3 && reboot >/dev/null 2>&1
```

##### Windows

```
#!/bin/bash
 
while [[ $# -ge 1 ]]; do
  case $1 in
    -v|--ver)
      shift
      VERtmp="$1"
      shift
      ;;
    -d|--debian|--ubuntu)
      shift
      vDEBtmp="$1"
      shift
      ;;
    -dd|--ddwin)
      shift
      ddMode='1'
      URLtmp="$1"
      shift
      ;;
    -p|--password)
      shift
      WDtmp="$1"
      shift
      ;;
    -a|--auto)
      shift
      INStmp='auto'
      ;;
    -m|--manual)
      shift
      INStmp='manual'
      ;;
    -apt|--mirror)
      shift
      isMirror='1'
      tmpMirror="$1"
      shift
      ;;
    -ssl)
      shift
      tmpSSL="$1"
      shift
      ;;
    *)
      echo -ne " Usage:\n\tbash $0\t-d/--debian [7/\033[33m\033[04mwheezy\033[0m|8/jessie|9/stretch]\n\t\t\t\t-v/--ver [32/\033[33m\033[04mi386\033[0m|64/amd64]\n\t\t\t\t-apt/--mirror\n\t\t\t\t-dd/--ddwin\n\t\t\t\t-a/--auto\n\t\t\t\t-m/--manual\n"
      exit 1;
      ;;
    esac
  done
 
[[ $EUID -ne 0 ]] && echo "Error:This script must be run as root!" && exit 1
[[ -f /boot/grub/grub.cfg ]] && GRUBOLD='0' && GRUBDIR='/boot/grub' && GRUBFILE='grub.cfg'
[[ -z $GRUBDIR ]] && [[ -f /boot/grub2/grub.cfg ]] && GRUBOLD='0' && GRUBDIR='/boot/grub2' && GRUBFILE='grub.cfg'
[[ -z $GRUBDIR ]] && [[ -f /boot/grub/grub.conf ]] && GRUBOLD='1' && GRUBDIR='/boot/grub' && GRUBFILE='grub.conf'
[ -z $GRUBDIR -o -z $GRUBFILE ] && echo "Error! Not Found grub path." && exit 1
 
[[ -n $vDEBtmp ]] && {
[ "$vDEBtmp" == '7' -o "$vDEBtmp" == 'wheezy' ] && linuxdists='debian' && vDEB='wheezy';
[ "$vDEBtmp" == '8' -o "$vDEBtmp" == 'jessie' ] && linuxdists='debian' && vDEB='jessie';
[ "$vDEBtmp" == '9' -o "$vDEBtmp" == 'stretch' ] && linuxdists='debian' && vDEB='stretch';
[[ "$vDEBtmp" == 'precise' ]] && linuxdists='ubuntu' && vDEB='precise';
[[ "$vDEBtmp" == 'trusty' ]] && linuxdists='ubuntu' && vDEB='trusty';
[[ "$vDEBtmp" == 'wily' ]] && linuxdists='ubuntu' && vDEB='wily';
[[ "$vDEBtmp" == 'xenial' ]] && linuxdists='ubuntu' && vDEB='xenial';
[[ "$vDEBtmp" == 'yakkety' ]] && linuxdists='ubuntu' && vDEB='yakkety';
[[ "$vDEBtmp" == 'zesty' ]] && linuxdists='ubuntu' && vDEB='zesty';
}
[[ -n $vDEBtmp ]] && {
[ "$VERtmp" == '32' -o "$VERtmp" == 'i386' ] && VER='i386';
[ "$VERtmp" == '64' -o "$VERtmp" == 'amd64' ] && VER='amd64';
}
[[ -n $ddMode ]] && [[ "$ddMode" == '1' ]] && {
[[ -n $URLtmp ]] && {
linuxdists='debian';
vDEB='jessie';
VER='amd64';
INStmp='auto'
DDURL="$URLtmp"
[[ -n $tmpSSL ]] && CURL_SUPPORT="$tmpSSL"
[[ -z $CURL_SUPPORT ]] && CURL_SUPPORT='https://moeclub.org/get-curl_udeb_amd64'
} || {
echo 'Please input vaild URL! '
exit 1
}
} || {
ddMode='0';
}
 
[[ -z $linuxdists ]] && linuxdists='debian'
[[ -n $isMirror ]] && [[ "$isMirror" == '1' ]] && [[ -n $tmpMirror ]] && {
tmpDebianMirror="$(echo -n "$tmpMirror" |grep -Eo '.*\.(\w+)')"
echo -n "$tmpDebianMirror" |grep -q '://'
[[ $? -eq '0' ]] && {
DebianMirror="$(echo -n "$tmpDebianMirror" |awk -F'://' '{print $2}')"
} || {
DebianMirror="$(echo -n "$tmpDebianMirror")"
}
} || {
[[ $linuxdists == 'debian' ]] && DebianMirror='httpredir.debian.org'
[[ $linuxdists == 'ubuntu' ]] && DebianMirror='archive.ubuntu.com'
}
[[ -z $DebianMirrorDirectory ]] && [[ -n $DebianMirror ]] && [[ -n $tmpMirror ]] && {
DebianMirrorDirectory="$(echo -n "$tmpMirror" |awk -F''${DebianMirror}'' '{print $2}' |sed 's/\/$//g')"
}
[[ "$DebianMirrorDirectory" == '/' ]] && [[ -n $DebianMirror ]] && {
[[ $linuxdists == 'debian' ]] && DebianMirrorDirectory='/debian'
[[ $linuxdists == 'ubuntu' ]] && DebianMirrorDirectory='/ubuntu'
}
[[ -z $DebianMirrorDirectory ]] && [[ -n $DebianMirror ]] && {
[[ $linuxdists == 'debian' ]] && DebianMirrorDirectory='/debian'
[[ $linuxdists == 'ubuntu' ]] && DebianMirrorDirectory='/ubuntu'
}
 
[[ -n $INStmp ]] && {
[[ "$INStmp" == 'auto' ]] && inVNC='n'
[[ "$INStmp" == 'manual' ]] && inVNC='y'
}
[[ -n $WDtmp ]] && myPASSWORD="$WDtmp"
 
[[ -z $vDEB ]] && vDEB='wheezy';
[[ -z $VER ]] && VER='i386';
[[ -z $myPASSWORD ]] && myPASSWORD='Vicer'
 
clear && echo -e "\n\033[36m# Install\033[0m\n"
 
[[ -z $inVNC ]] && ASKVNC(){
inVNC='y';
[[ "$ddMode" == '0' ]] && {
echo -ne "\033[34mCan you login VNC?\033[0m\e[33m[\e[32my\e[33m/n]\e[0m "
read inVNCtmp
[[ -n "$inVNCtmp" ]] && inVNC=$inVNCtmp
}
[ "$inVNC" == 'y' -o "$inVNC" == 'Y' ] && inVNC='y'
[ "$inVNC" == 'n' -o "$inVNC" == 'N' ] && inVNC='n'
}
 
[ "$inVNC" == 'y' -o "$inVNC" == 'n' ] || ASKVNC;
 
[[ $linuxdists == 'debian' ]] && LinuxName='Debian'
[[ $linuxdists == 'ubuntu' ]] && LinuxName='Ubuntu'
[[ "$ddMode" == '0' ]] && { 
[[ "$inVNC" == 'y' ]] && echo -e "\033[34mManual Mode\033[0m insatll \033[33m$LinuxName\033[0m [\033[33m$vDEB\033[0m] [\033[33m$VER\033[0m] in VNC. "
[[ "$inVNC" == 'n' ]] && echo -e "\033[34mAuto Mode\033[0m insatll \033[33m$LinuxName\033[0m [\033[33m$vDEB\033[0m] [\033[33m$VER\033[0m]. "
}
[[ "$ddMode" == '1' ]] && {
echo -ne "\033[34mAuto Mode\033[0m insatll \033[33mWindows\033[0m\n[\033[33m$DDURL\033[0m]\n"
}
 
echo -e "\n[\033[33m$vDEB\033[0m] [\033[33m$VER\033[0m] Downloading..."
[[ -z $DebianMirror ]] && echo -ne "\033[31mError! \033[0mGet debian mirror fail! \n" && exit 1
[[ -z $DebianMirrorDirectory ]] && echo -ne "\033[31mError! \033[0mGet debian mirror directory fail! \n" && exit 1
wget --no-check-certificate -qO '/boot/initrd.gz' "http://$DebianMirror$DebianMirrorDirectory/dists/$vDEB/main/installer-$VER/current/images/netboot/$linuxdists-installer/$VER/initrd.gz"
[[ $? -ne '0' ]] && echo -ne "\033[31mError! \033[0mDownload 'initrd.gz' failed! \n" && exit 1
wget --no-check-certificate -qO '/boot/linux' "http://$DebianMirror$DebianMirrorDirectory/dists/$vDEB/main/installer-$VER/current/images/netboot/$linuxdists-installer/$VER/linux"
[[ $? -ne '0' ]] && echo -ne "\033[31mError! \033[0mDownload 'linux' failed! \n" && exit 1
 
DEFAULTNET="$(ip route show |grep -o 'default via [0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.*' |head -n1 |sed 's/proto.*\|onlink.*//g' |awk '{print $NF}')"
[[ -n "$DEFAULTNET" ]] && IPSUB="$(ip addr |grep ''${DEFAULTNET}'' |grep 'global' |grep 'brd' |head -n1 |grep -o '[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}/[0-9]\{1,2\}')"
IPv4="$(echo -n "$IPSUB" |cut -d'/' -f1)"
NETSUB="$(echo -n "$IPSUB" |grep -o '/[0-9]\{1,2\}')"
GATE="$(ip route show |grep -o 'default via [0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}' |head -n1 |grep -o '[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}')"
[[ -n "$NETSUB" ]] && MASK="$(echo -n '128.0.0.0/1,192.0.0.0/2,224.0.0.0/3,240.0.0.0/4,248.0.0.0/5,252.0.0.0/6,254.0.0.0/7,255.0.0.0/8,255.128.0.0/9,255.192.0.0/10,255.224.0.0/11,255.240.0.0/12,255.248.0.0/13,255.252.0.0/14,255.254.0.0/15,255.255.0.0/16,255.255.128.0/17,255.255.192.0/18,255.255.224.0/19,255.255.240.0/20,255.255.248.0/21,255.255.252.0/22,255.255.254.0/23,255.255.255.0/24,255.255.255.128/25,255.255.255.192/26,255.255.255.224/27,255.255.255.240/28,255.255.255.248/29,255.255.255.252/30,255.255.255.254/31,255.255.255.255/32' |grep -o '[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}'${NETSUB}'' |cut -d'/' -f1)"
 
[[ -n "$GATE" ]] && [[ -n "$MASK" ]] && [[ -n "$IPv4" ]] || {
echo "Not found `ip command`, It will use `route command`."
ipNum() {
  local IFS='.'
  read ip1 ip2 ip3 ip4 <<<"$1"
  echo $((ip1*(1<<24)+ip2*(1<<16)+ip3*(1<<8)+ip4))
}
 
SelectMax(){
ii=0
for IPITEM in `route -n |awk -v OUT=$1 '{print $OUT}' |grep '[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}'`
  do
    NumTMP="$(ipNum $IPITEM)"
    eval "arrayNum[$ii]='$NumTMP,$IPITEM'"
    ii=$[$ii+1]
  done
echo ${arrayNum[@]} |sed 's/\s/\n/g' |sort -n -k 1 -t ',' |tail -n1 |cut -d',' -f2
}
 
[[ -z $IPv4 ]] && IPv4="$(ifconfig |grep 'Bcast' |head -n1 |grep -o '[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}.[0-9]\{1,3\}' |head -n1)"
[[ -z $GATE ]] && GATE="$(SelectMax 2)"
[[ -z $MASK ]] && MASK="$(SelectMax 3)"
 
[[ -n "$GATE" ]] && [[ -n "$MASK" ]] && [[ -n "$IPv4" ]] || {
echo "Error! Not configure network. "
exit 1
}
}
 
[[ -f /etc/network/interfaces ]] && {
[[ -z "$(sed -n '/iface.*inet static/p' /etc/network/interfaces)" ]] && AutoNet='1' || AutoNet='0'
[[ -d /etc/network/interfaces.d ]] && {
ICFGN="$(find /etc/network/interfaces.d -name '*.cfg' |wc -l)" || ICFGN='0'
[[ "$ICFGN" -ne '0' ]] && {
for NetCFG in `ls -1 /etc/network/interfaces.d/*.cfg`
 do 
  [[ -z "$(cat $NetCFG | sed -n '/iface.*inet static/p')" ]] && AutoNet='1' || AutoNet='0'
  [[ "$AutoNet" -eq '0' ]] && break
done
}
}
}
[[ -d /etc/sysconfig/network-scripts ]] && {
ICFGN="$(find /etc/sysconfig/network-scripts -name 'ifcfg-*' |grep -v 'lo'|wc -l)" || ICFGN='0'
[[ "$ICFGN" -ne '0' ]] && {
for NetCFG in `ls -1 /etc/sysconfig/network-scripts/ifcfg-* |grep -v 'lo$' |grep -v ':[0-9]\{1,\}'`
 do 
  [[ -n "$(cat $NetCFG | sed -n '/BOOTPROTO.*[dD][hH][cC][pP]/p')" ]] && AutoNet='1' || {
  AutoNet='0' && . $NetCFG
  [[ -n $NETMASK ]] && MASK="$NETMASK"
  [[ -n $GATEWAY ]] && GATE="$GATEWAY"
}
  [[ "$AutoNet" -eq '0' ]] && break
done
}
}
 
[[ ! -f $GRUBDIR/$GRUBFILE ]] && echo "Error! Not Found $GRUBFILE. " && exit 1
 
[[ ! -f $GRUBDIR/$GRUBFILE.old ]] && [[ -f $GRUBDIR/$GRUBFILE.bak ]] && mv -f $GRUBDIR/$GRUBFILE.bak $GRUBDIR/$GRUBFILE.old
mv -f $GRUBDIR/$GRUBFILE $GRUBDIR/$GRUBFILE.bak
[[ -f $GRUBDIR/$GRUBFILE.old ]] && cat $GRUBDIR/$GRUBFILE.old >$GRUBDIR/$GRUBFILE || cat $GRUBDIR/$GRUBFILE.bak >$GRUBDIR/$GRUBFILE
 
[[ "$GRUBOLD" == '0' ]] && {
CFG0="$(awk '/menuentry /{print NR}' $GRUBDIR/$GRUBFILE|head -n 1)"
CFG2="$(awk '/menuentry /{print NR}' $GRUBDIR/$GRUBFILE|head -n 2 |tail -n 1)"
CFG1=""
for CFGtmp in `awk '/}/{print NR}' $GRUBDIR/$GRUBFILE`
 do
  [ $CFGtmp -gt "$CFG0" -a $CFGtmp -lt "$CFG2" ] && CFG1="$CFGtmp";
 done
[[ -z "$CFG1" ]] && {
echo "Error! read $GRUBFILE. "
exit 1
}
sed -n "$CFG0,$CFG1"p $GRUBDIR/$GRUBFILE >/tmp/grub.new
[[ -f /tmp/grub.new ]] && [[ "$(grep -c '{' /tmp/grub.new)" -eq "$(grep -c '}' /tmp/grub.new)" ]] || {
echo -ne "\033[31mError! \033[0mNot configure $GRUBFILE. \n"
exit 1
}
 
sed -i "/menuentry.*/c\menuentry\ \'Install OS \[$vDEB\ $VER\]\'\ --class debian\ --class\ gnu-linux\ --class\ gnu\ --class\ os\ \{" /tmp/grub.new
[[ "$(grep -c '{' /tmp/grub.new)" -eq "$(grep -c '}' /tmp/grub.new)" ]] || {
echo "Error! configure append $GRUBFILE. "
exit 1
}
sed -i "/echo.*Loading/d" /tmp/grub.new
}
 
[[ "$GRUBOLD" == '1' ]] && {
CFG0="$(awk '/title /{print NR}' $GRUBDIR/$GRUBFILE|head -n 1)"
CFG1="$(awk '/title /{print NR}' $GRUBDIR/$GRUBFILE|head -n 2 |tail -n 1)"
[[ -n $CFG0 ]] && [ -z $CFG1 -o $CFG1 == $CFG0 ] && sed -n "$CFG0,$"p $GRUBDIR/$GRUBFILE >/tmp/grub.new
[[ -n $CFG0 ]] && [ -z $CFG1 -o $CFG1 != $CFG0 ] && sed -n "$CFG0,$CFG1"p $GRUBDIR/$GRUBFILE >/tmp/grub.new
[[ ! -f /tmp/grub.new ]] && echo "Error! configure append $GRUBFILE. " && exit 1
sed -i "/title.*/c\title\ \'Install OS \[$vDEB\ $VER\]\'" /tmp/grub.new
sed -i '/^#/d' /tmp/grub.new
}
 
[[ -n "$(grep 'initrd.*/' /tmp/grub.new |awk '{print $2}' |tail -n 1 |grep '^/boot/')" ]] && Type='InBoot' || Type='NoBoot'
 
LinuxKernel="$(grep 'linux.*/' /tmp/grub.new |awk '{print $1}' |head -n 1)"
[[ -z $LinuxKernel ]] && LinuxKernel="$(grep 'kernel.*/' /tmp/grub.new |awk '{print $1}' |head -n 1)"
LinuxIMG="$(grep 'initrd.*/' /tmp/grub.new |awk '{print $1}' |tail -n 1)"
 
[[ "$Type" == 'InBoot' ]] && {
sed -i "/$LinuxKernel.*\//c\\\t$LinuxKernel\\t\/boot\/linux auto=true hostname=$linuxdists domain= -- quiet" /tmp/grub.new
sed -i "/$LinuxIMG.*\//c\\\t$LinuxIMG\\t\/boot\/initrd.gz" /tmp/grub.new
}
 
[[ "$Type" == 'NoBoot' ]] && {
sed -i "/$LinuxKernel.*\//c\\\t$LinuxKernel\\t\/linux auto=true hostname=$linuxdists domain= -- quiet" /tmp/grub.new
sed -i "/$LinuxIMG.*\//c\\\t$LinuxIMG\\t\/initrd.gz" /tmp/grub.new
}
 
sed -i '$a\\n' /tmp/grub.new
 
[[ "$inVNC" == 'n' ]] && {
GRUBPATCH='0'
[ -f /etc/network/interfaces -o -d /etc/sysconfig/network-scripts ] && {
sed -i ''${CFG0}'i\\n' $GRUBDIR/$GRUBFILE
sed -i ''${CFG0}'r /tmp/grub.new' $GRUBDIR/$GRUBFILE
[[ -z $AutoNet ]] && echo "Error, Not found interfaces config." && exit 1
[[ -f  $GRUBDIR/grubenv ]] && sed -i 's/saved_entry/#saved_entry/g' $GRUBDIR/grubenv
[[ -d /boot/tmp ]] && rm -rf /boot/tmp
mkdir -p /boot/tmp/
cd /boot/tmp/
gzip -d < ../initrd.gz | cpio --extract --verbose --make-directories --no-absolute-filenames >>/dev/null 2>&1
cat >/boot/tmp/preseed.cfg<<EOF
d-i debian-installer/locale string en_US
d-i console-setup/layoutcode string us
 
d-i keyboard-configuration/xkb-keymap string us
 
d-i netcfg/choose_interface select auto
 
d-i netcfg/disable_autoconfig boolean true
d-i netcfg/dhcp_failed note
d-i netcfg/dhcp_options select Configure network manually
d-i netcfg/get_ipaddress string $IPv4
d-i netcfg/get_netmask string $MASK
d-i netcfg/get_gateway string $GATE
d-i netcfg/get_nameservers string 8.8.8.8
d-i netcfg/no_default_route boolean true
d-i netcfg/confirm_static boolean true
 
d-i hw-detect/load_firmware boolean false
 
d-i mirror/country string manual
d-i mirror/http/hostname string $DebianMirror
d-i mirror/http/directory string $DebianMirrorDirectory
d-i mirror/http/proxy string
 
d-i passwd/root-login boolean ture
d-i passwd/make-user boolean false
d-i passwd/root-password password $myPASSWORD
d-i passwd/root-password-again password $myPASSWORD
d-i user-setup/allow-password-weak boolean true
d-i user-setup/encrypt-home boolean false
 
d-i clock-setup/utc boolean true
d-i time/zone string US/Eastern
d-i clock-setup/ntp boolean true
 
d-i preseed/early_command string anna-install libfuse2-udeb fuse-udeb ntfs-3g-udeb fuse-modules-3.16.0-4-amd64-di
d-i partman/early_command string \
debconf-set partman-auto/disk "\$(list-devices disk |head -n1)"; \
wget -qO- '$DDURL' |gunzip -dc |/bin/dd of=\$(list-devices disk |head -n1); \
mount.ntfs-3g \$(list-devices partition |head -n1) /mnt; \
cp -f '/net.bat' '/mnt/ProgramData/Microsoft/Windows/Start Menu/Programs/Startup/net.bat'; \
/sbin/reboot; \
debconf-set grub-installer/bootdev string "\$(list-devices disk |head -n1)"; \
umount /media || true; \
 
d-i partman/mount_style select uuid
d-i partman-auto/init_automatically_partition select Guided - use entire disk
d-i partman-auto/method string regular
d-i partman-lvm/device_remove_lvm boolean true
d-i partman-md/device_remove_md boolean true
d-i partman-auto/choose_recipe select atomic
d-i partman-partitioning/confirm_write_new_label boolean true
d-i partman/choose_partition select finish
d-i partman-lvm/confirm boolean true
d-i partman-lvm/confirm_nooverwrite boolean true
d-i partman/confirm boolean true
d-i partman/confirm_nooverwrite boolean true
 
d-i debian-installer/allow_unauthenticated boolean true
 
tasksel tasksel/first multiselect minimal
d-i pkgsel/update-policy select none
d-i pkgsel/include string openssh-server
d-i pkgsel/upgrade select none
 
popularity-contest popularity-contest/participate boolean false
 
d-i grub-installer/only_debian boolean true
d-i grub-installer/bootdev string default
d-i finish-install/reboot_in_progress note
d-i debian-installer/exit/reboot boolean true
d-i preseed/late_command string \
sed -i 's/^.*PermitRootLogin.*/PermitRootLogin yes/g' /target/etc/ssh/sshd_config; \
sed -i 's/^.*PasswordAuthentication.*/PasswordAuthentication yes/g' /target/etc/ssh/sshd_config;
EOF
[[ "$AutoNet" -eq '1' ]] && {
sed -i '/netcfg\/disable_autoconfig/d' /boot/tmp/preseed.cfg
sed -i '/netcfg\/dhcp_options/d' /boot/tmp/preseed.cfg
sed -i '/netcfg\/get_.*/d' /boot/tmp/preseed.cfg
sed -i '/netcfg\/confirm_static/d' /boot/tmp/preseed.cfg
}
[[ "$vDEB" == 'trusty' ]] && GRUBPATCH='1'
[[ "$vDEB" == 'wily' ]] && GRUBPATCH='1'
[[ "$GRUBPATCH" == '1' ]] && {
sed -i 's/^d-i\ grub-installer\/bootdev\ string\ default//g' /boot/tmp/preseed.cfg
}
[[ "$GRUBPATCH" == '0' ]] && {
sed -i 's/debconf-set\ grub-installer\/bootdev.*\"\;//g' /boot/tmp/preseed.cfg
}
[[ "$vDEB" == 'xenial' ]] && {
sed -i 's/^d-i\ clock-setup\/ntp\ boolean\ true/d-i\ clock-setup\/ntp\ boolean\ false/g' /boot/tmp/preseed.cfg
}
[[ "$linuxdists" == 'debian' ]] && {
sed -i '/user-setup\/allow-password-weak/d' /boot/tmp/preseed.cfg
sed -i '/user-setup\/encrypt-home/d' /boot/tmp/preseed.cfg
sed -i '/pkgsel\/update-policy/d' /boot/tmp/preseed.cfg
sed -i 's/umount\ \/media.*true\;\ //g' /boot/tmp/preseed.cfg
}
[[ "$ddMode" == '1' ]] && {
[[ "$AutoNet" -eq '1' ]] && echo -ne "@ECHO OFF\r\ncd\040\057d\040\042\045ProgramData\045\057Microsoft\057Windows\057Start\040Menu\057Programs\057Startup\042\r\ndel\040\057f\040\057q\040net\056bat\r\n\r\n" >'/boot/tmp/net.tmp';
[[ "$AutoNet" -eq '0' ]] && echo -ne "@ECHO OFF\r\ncd\056\076\045windir\045\GetAdmin\r\nif\040exist\040\045windir\045\GetAdmin\040\050del\040\057f\040\057q\040\042\045windir\045\GetAdmin\042\051\040else\040\050\r\necho\040CreateObject^\050\042Shell\056Application\042^\051\056ShellExecute\040\042\045~s0\042\054\040\042\045\052\042\054\040\042\042\054\040\042runas\042\054\040\061\040\076\076\040\042\045temp\045\Admin\056vbs\042\r\n\042\045temp\045\Admin\056vbs\042\r\ndel\040\057f\040\057q\040\042\045temp\045\Admin\056vbs\042\r\nexit\040\057b\040\062\051\r\nfor\040\057f\040\042tokens=\063\052\042\040\045\045i\040in\040\050\047netsh\040interface\040show\040interface\040^|more\040+3\040^|findstr\040\057R\040\042本地\056\052\040以太\056\052\040Local\056\052\040Ethernet\042\047\051\040do\040\050set\040EthName=\045\045j\051\r\nnetsh\040-c\040interface\040ip\040set\040address\040name=\042\045EthName\045\042\040source=static\040address=$IPv4\040mask=$MASK\040gateway=$GATE\r\nnetsh\040-c\040interface\040ip\040add\040dnsservers\040name=\042\045EthName\045\042 address=\070\056\070\056\070\056\070\040index=1\040validate=no\r\nnetsh\040-c\040interface\040ip\040add\040dnsservers\040name=\042\045EthName\045\042\040address=\070\056\070\056\064\056\064\040index=2\040validate=no\r\ncd\040\057d\040\042\045ProgramData\045\057Microsoft\057Windows\057Start\040Menu\057Programs\057Startup\042\r\ndel\040\057f\040\057q\040net\056bat\r\n\r\n" >'/boot/tmp/net.tmp';
iconv -f 'UTF-8' -t 'GBK' '/boot/tmp/net.tmp' -o '/boot/tmp/net.bat'
rm -rf '/boot/tmp/net.tmp'
echo "$DDURL" |grep -q '^https://'
[[ $? -eq '0' ]] && {
echo -ne '\nAdd curl support...\n'
[[ -n $CURL_SUPPORT ]] && {
wget --no-check-certificate -qO- "$CURL_SUPPORT" |tar -x
[[ ! -f  /boot/tmp/usr/bin/curl ]] && echo 'Error! CURL_SUPPORT.' && exit 1;
sed -i 's/wget\ -qO-/\/usr\/bin\/curl -ksSL/g' /boot/tmp/preseed.cfg
[[ $? -eq '0' ]] && echo 'Success! \n\n'
} || {
echo -ne 'Not curl support package! \n\n'
exit 1
}
}
}
[[ "$ddMode" == '0' ]] && {
sed -i '/anna-install/d' /boot/tmp/preseed.cfg
sed -i 's/wget.*\/sbin\/reboot\;\ //g' /boot/tmp/preseed.cfg
}
rm -rf ../initrd.gz
find . | cpio -H newc --create --verbose | gzip -9 > ../initrd.gz
rm -rf /boot/tmp
}
}
 
[[ "$inVNC" == 'y' ]] && {
sed -i '$i\\n' $GRUBDIR/$GRUBFILE
sed -i '$r /tmp/grub.new' $GRUBDIR/$GRUBFILE
echo -e "\n\033[33m\033[04mIt will reboot! \nPlease look at VNC! \nSelect\033[0m\033[32m Install OS [$vDEB $VER] \033[33m\033[4mto install system.\033[04m\n\n\033[31m\033[04mThere is some information for you.\nDO NOT CLOSE THE WINDOW! \033[0m\n"
echo -e "\033[35mIPv4\t\tNETMASK\t\tGATEWAY\033[0m"
echo -e "\033[36m\033[04m$IPv4\033[0m\t\033[36m\033[04m$MASK\033[0m\t\033[36m\033[04m$GATE\033[0m\n\n"
 
read -n 1 -p "Press Enter to reboot..." INP
if [[ "$INP" != '' ]] ; then
echo -ne '\b \n'
echo "";
fi
}
 
chown root:root $GRUBDIR/$GRUBFILE
chmod 444 $GRUBDIR/$GRUBFILE
 
sleep 3 && reboot >/dev/null 2>&1
```

#### 原系统为Windows

##### Linux

```
@ECHO OFF&PUSHD %~DP0 &TITLE Win32Loader
setlocal enabledelayedexpansion
::Author MoeClub.org
color 87
cd.>%windir%\GetAdmin
if exist %windir%\GetAdmin (del /f /q "%windir%\GetAdmin") else (
echo CreateObject^("Shell.Application"^).ShellExecute "%~s0", "%*", "", "runas", 1 >> "%temp%\Admin.vbs"
"%temp%\Admin.vbs"
del /s /q "%temp%\Admin.vbs"
exit /b 2)
cls
 
echo * Init Win32Loader.
set URL=https://moeclub.org/attachment/WindowsSoftware

set download=0
set try_download=1
 
:Init
mkdir "%SystemDrive%\win32-loader" >NUL 2>NUL
if exist "%SystemDrive%\Windows\System32\WindowsPowerShell" (
set use_ps=1
) else (
set use_ps=0
)
 
if %use_ps% equ 1 (
goto InitIt
) else (
goto InitFail
)
 
:InitIt
set try_download=0
powershell.exe -command "& {$client = new-object System.Net.WebClient; $client.DownloadFile('!URL!/g2ldr/g2ldr','%SystemDrive%\g2ldr')}" >NUL 2>NUL
powershell.exe -command "& {$client = new-object System.Net.WebClient; $client.DownloadFile('!URL!/g2ldr/g2ldr.mbr','%SystemDrive%\g2ldr.mbr')}" >NUL 2>NUL
powershell.exe -command "& {$client = new-object System.Net.WebClient; $client.DownloadFile('!URL!/g2ldr/grub.cfg','%SystemDrive%\win32-loader\grub.cfg')}" >NUL 2>NUL
goto InitDone
 
:InitFail
echo Not found powershell, please download them by yourself.
echo '%SystemDrive%\g2ldr'
echo '%SystemDrive%\g2ldr.mbr'
echo '%SystemDrive%\win32-loader\grub.cfg'
echo Press [ENTER] when you finished.
pause >NUL 2>NUL
goto InitDone
 
:InitDone
if !try_download! equ 0 (
set InitOption=InitFail
) else (
set InitOption=Init
)
if not exist "%SystemDrive%\g2ldr" goto !InitOption!
if not exist "%SystemDrive%\g2ldr.mbr" goto !InitOption!
if not exist "%SystemDrive%\win32-loader\grub.cfg" goto !InitOption!
 
:Image
echo.
echo * Please select initrd mode.
echo     [1] Online download
echo     [2] Local file
choice /n /c 12 /m Select:
if errorlevel 2 goto LocalMode
if errorlevel 1 goto OnlineMode
goto Image
 
:OnlineMode
echo.
echo * Please select source.
echo     [1] by MoeClub (DHCP or VNC Support)
echo     [2] by yourself
choice /n /c 12 /m Select:
if errorlevel 2 goto Yourself
if errorlevel 1 goto MoeClub
goto OnlineMode
:Yourself
echo.
echo if 'initrd.img' URL is 'https://moeclub.org/onedrive/IMAGE/Loader/DebianJessie/initrd.img', Please input 'https://moeclub.org/onedrive/IMAGE/Loader/DebianJessie'.
set /p IMG_URL=URL :
if defined IMG_URL (
goto Download
) else (
goto MoeClub
)
:MoeClub
set IMG_URL=https://moeclub.org/onedrive/IMAGE/Loader/DebianJessie
goto Download
:Download
if %use_ps% equ 1 (
echo.
echo Downloading 'initrd.img'...
powershell.exe -command "& {$client = new-object System.Net.WebClient; $client.DownloadFile('!IMG_URL!/initrd.img','%SystemDrive%\win32-loader\initrd.img')}" >NUL 2>NUL
echo Downloading 'vmlinuz'...
powershell.exe -command "& {$client = new-object System.Net.WebClient; $client.DownloadFile('!IMG_URL!/vmlinuz','%SystemDrive%\win32-loader\vmlinuz')}" >NUL 2>NUL
set download=1
) else (
echo Not support online download, auto change Local initrd.
goto LocalMode
)
 
:LocalMode
if !download! equ 0 (
echo.
echo Please put 'initrd.img' and 'vmlinuz' to '%SystemDrive%\win32-loader' .
echo Press [ENTER] when you finished.
pause >NUL 2>NUL
)
 
:Done0
set download=0
if exist "%SystemDrive%\win32-loader\initrd.img" (
goto Done1
) else (
echo Not found '%SystemDrive%\win32-loader\initrd.img' .
goto LocalMode
)
 
:Done1
set download=0
if exist "%SystemDrive%\win32-loader\vmlinuz" (
goto Done
) else (
echo Not found '%SystemDrive%\win32-loader\vmlinuz' .
goto LocalMode
)
 
:Done
echo.
echo Press [ENTER] to reboot...
pause >NUL 2>NUL
if not exist "%SystemDrive%\g2ldr" echo Not found '%SystemDrive%\g2ldr' . && exit 1
if not exist "%SystemDrive%\g2ldr.mbr" echo Not found '%SystemDrive%\g2ldr.mbr' . && exit 1
if not exist "%SystemDrive%\win32-loader\grub.cfg" echo Not found '%SystemDrive%\win32-loader\grub.cfg' . && exit 1
if not exist "%SystemDrive%\win32-loader\initrd.img" echo Not found '%SystemDrive%\win32-loader\initrd.img' . && exit 1
if not exist "%SystemDrive%\win32-loader\vmlinuz" echo Not found '%SystemDrive%\win32-loader\vmlinuz' . && exit 1
set id={01234567-89ab-cdef-0123-456789abcdef}
bcdedit /create %id% /d "Debian GUN/Linux" /application bootsector >NUL 2>NUL
bcdedit /set %id% device partition=%SystemDrive% >NUL 2>NUL
bcdedit /set %id% path \g2ldr.mbr >NUL 2>NUL
bcdedit /displayorder %id% /addlast >NUL 2>NUL
bcdedit /bootsequence %id% /addfirst >NUL 2>NUL
shutdown -r -t 0
```

### HTTP/2检测

在支持loadTimes的chrome浏览器下执行。

```
(function(){
    if(window.chrome && typeof chrome.loadTimes === 'function') {
        var loadTimes = window.chrome.loadTimes();
        var spdy = loadTimes.wasFetchedViaSpdy;
        var info = loadTimes.npnNegotiatedProtocol || loadTimes.connectionInfo;
        // 就以 「h2」作为判断标识
        if(spdy && /^h2/i.test(info)) {
            return console.info('本站点使用了HTTP/2');
        }
    }
    console.warn('本站点没有使用HTTP/2');
})();
```

## 生成UUID

可在终端输入以下命令。

```
cat /proc/sys/kernel/random/uuid
```

也可通过以下网站。

```
https://www.uuidgenerator.net/
```

## 生成指定前缀信用卡卡号

运行以下命令即可。

```
wget https://raw.githubusercontent.com/malaohu/ruyo-shell/master/credit_card_number.py
python credit_card_number.py
```

运行示例如下。

```
# 生成前缀为'123456'，1个，卡号长度16位
fakecard = credit_card_number('123456')
# 生成前缀为'123456'，1个，卡号长度16位
fakecard = credit_card_number('123456',1,16)
# 生成前缀为'123'或'234'的信用卡卡号，共15个，卡号长度16位
fakecard = credit_card_number(['123','234'], 15,16)
```

脚本如下。

```
from random import Random
import copy
 
def completed_number(prefix, length):
    """
    'prefix' is the start of the CC number as a string, any number of digits.
    'length' is the length of the CC number to generate. Typically 13 or 16
    """
    generator = Random()
    generator.seed()    # Seed from current time
    ccnumber = prefix
    # generate digits
    while len(ccnumber) < (length - 1):
        digit = str(generator.choice(range(0, 10)))
        ccnumber.append(digit)
  # Calculate sum
    sum = 0
    pos = 0
    reversedCCnumber = []
    reversedCCnumber.extend(ccnumber)
    reversedCCnumber.reverse()
    while pos < length - 1:
        odd = int(reversedCCnumber[pos]) * 2
        if odd > 9:
            odd -= 9
        sum += odd
        if pos != (length - 2):
            sum += int(reversedCCnumber[pos + 1])
        pos += 2
  # Calculate check digit
    checkdigit = ((sum // 10 + 1) * 10 - sum) % 10
    ccnumber.append(str(checkdigit))
    return ''.join(ccnumber)
 
def credit_card_number(prefixList, howMany=1, length=16):
    generator = Random()
    generator.seed()    # Seed from current time
    if type(prefixList)==str:
        prefixList=[[i for i in prefixList]]
    if type(prefixList[0])==str:
        prefixList=[[i for i in List] for List in prefixList]
    result = []
    while len(result) < howMany:
        ccnumber = copy.copy(generator.choice(prefixList))
        result.append(completed_number(ccnumber, length))
    return result
```

# 参考教程

## trojan-gfw/trojan

```
https://github.com/trojan-gfw/trojan
```

## Shadowsocks官网

```
https://shadowsocks.org/en/index.html
```

## V2Ray官网

```
https://www.v2ray.com/
https://github.com/v2fly/v2ray-core
https://www.v2fly.org/
https://guide.v2fly.org/
https://toutyrater.github.io/
```

## Proxifier使用教程

```
https://blog.csdn.net/u013066730/article/details/88788191
```

## V2Ray+TLS+DNSSEC+WS+Nginx+BBRPlus 搭建教程【2020年最新】

```
https://yuchuanfeng.github.io/posts/v2ray+tls+ws+nginx/
```

## 德国Euserv免费VPS申请+开通V2RAY+安装宝塔图文教程

```
https://www.shopee6.com/web/web-tutorial/euserv-order-v2ray-bt.html
https://lala.im/6781.html
https://1rmb.net/t/31.html
```

## 使用宝塔快速安装WordPress教程

```
https://www.wpmee.com/5976.html
```

## Okteto上部署v2ray

```
https://704sjf.coding-pages.com/post/hello-gridea/
```

## 自制一个真正属于自己的v2ray订阅地址（自己动手创建v2ray订阅地址教程）

```
https://aishangyou.tube/?p=6885
```

## Goorm Shell Auto Deploy

```
https://github.com/guleonseon/goorm-auto
```

## okteto容器破解搭建宝塔shell教程

```
https://fbk.ink/946.html
https://www.hostloc.com/forum.php?mod=viewthread&tid=667527
```

## 在Compute Engine上设置适用于Linux的Chrome远程桌面

```
https://cloud.google.com/solutions/chrome-desktop-remote-on-compute-engine?hl=zh-cn#installing_chrome_remote_desktop_on_the_vm_instance
```

## 10分钟快速掌握Docker必备基础知识

```
https://juejin.im/post/6844903918372143112
https://juejin.im/post/6844903938030845966
```

## wget/curl large file from google drive

```
https://stackoverflow.com/questions/25010369/wget-curl-large-file-from-google-drive/39225039#39225039
```

## 如何卸载阿里云&腾讯云官方的监控软件服务

```
https://51.ruyo.net/5369.html
```

## oooldking/script

```
https://github.com/oooldking/script
```

## 教程01：如何通过V2Ray搭配使用洋葱tor浏览器？

```
https://test.enuotime.com/guide01
```

## 别以为有vpn我找不到你（网警抓人全过程）

```
http://www.91ri.org/3599.html
```

## 浅谈HTTP劫持、DNS污染的影响及解决办法（仅个人理解）

```
https://doubibackup.com/6t3mypbm-5.html
```

## 关于目前 GFW(墙) 的封锁方式TCP封锁(阻断)猜想

```
https://doubibackup.com/wkcjzpyd-2.html
```

## SS二级（前置）代理设置实现中转加速观看BBC iPlayer

```
https://www.itengli.com/ss-relay/
```

## macOS - ClashX 使用教程

```
https://wiki.kache.moe/2019/12/11/macOS-ClashX/
```

## ClashX教程 | macOS上好看又好用的科学上网工具

```
https://merlinblog.xyz/wiki/ClashX.html
```

## 如何对Linux服务器的端口限速？

```
https://51.ruyo.net/2877.html
```

## 免费Docker容器来挂机Vagex赚美刀

```
https://51.ruyo.net/3290.html
```

## 国内申请Google Adsense账号以及完成首笔收款

```
https://51.ruyo.net/12240.html
```

## 2020Google镜像大全，谷歌镜像网址

```
https://www.uedbox.com/post/54776/
```

## 香港实体预付万事达借记卡neat注册申请指南

```
https://www.vpsdawanjia.com/1817.html
```

## 注册Google Voice方案，成功率较高

```
https://github.com/masonme/GoogleVoice
```

## 使用IFTTT让Google Voice自动回复短信来保号

```
https://www.vpsdawanjia.com/1452.html
```

## Accesser

```
https://urenko.github.io/Accesser/
```

## “如何翻墙”系列：关于 Tor 的常见问题解答

```
https://program-think.blogspot.com/2013/11/tor-faq.html
```

## w3-goto-world/reademe.md

```
https://hoochanlon.github.io/fq-book/#/browse/zeronet
https://github.com/hoochanlon/w3-goto-world/blob/master/%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91%E3%80%81%E6%9A%97%E7%BD%91%E3%80%81%E9%9B%B6%E7%BD%91/%E9%9B%B6%E7%BD%91%E4%B8%8E%E6%9A%97%E7%BD%91/reademe.md
```

## SSH-Tunnel

```
https://hoochanlon.github.io/fq-book/#/proxy/SSH-Tunnel
```

## Clash For Windows 小贴士：允许局域网 @320资源私享家

```
https://www.hottg.com/www_320nle_com/133/zh-TW.html
```

## Surge Mac 3 无限试用方案

```
https://blog.cat73.org/20190528/2019052801.surge3-crack/
https://gist.github.com/whyliam/a27bae053207dcb4c46bb5c9cf8ef274
```

## Alvin9999/new-pac Wiki

```
https://github.com/Alvin9999/new-pac/wiki
```

## shuuzhoou/doubi: 一个逗比写的各种逗比脚本~

```
https://github.com/shuuzhoou/doubi
```

## configuration · Dreamacro/clash Wiki

```
https://github.com/Dreamacro/clash/wiki/configuration
```

## Clash基本配置记录

```
http://blog.joylau.cn/2020/05/01/Clash-Config/
```

## Clash proxy-provider 搭配 subconverter 使用小记

```
https://10101.io/2020/02/12/use-clash-proxy-provider-with-subconverter
```

## V2Ray 白话文教程

```
https://toutyrater.github.io/
https://toutyrater.github.io/advanced/vps_relay.html
https://toutyrater.github.io/app/netflix.html
https://toutyrater.github.io/advanced/outboundproxy.html
```

## 流媒体解锁服务，解锁不能看Netflix的服务器

```
https://www.mainstriker.com/archives/1207
```

## Getting Started with Software-Defined Networking and Creating a VPN with ZeroTier One

```
https://www.digitalocean.com/community/tutorials/getting-started-software-defined-networking-creating-vpn-zerotier-one?utm_medium=social&utm_source=twitter&utm_campaign=zerotier_tut&utm_content=no_image
```

## 启用 ZeroTier 的 VPN 功能，通过 VPN 转发客户端流量

```
https://github.com/aturl/awesome-anti-gfw/blob/master/ZeroTier/ZeroTier's_VPN.md
```

## 使用 VMWare 安装 macOS 虚拟机使用 Surge 作为代理网关

```
https://blog.skk.moe/post/macos-vmware-surge-gateway/
```

## 解锁netflix网飞代理观看方式原理及多种方法

```
https://www.mebi.me/1035.html
```

## Introduce - Clash

```
https://lancellc.gitbook.io/clash/
```
