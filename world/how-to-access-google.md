# Google正确食用指南

## 相关知识

### 运行模式

| 类型 | 说明 |
| :--- | :--- |
| 全局直连 | 所有的流量会被直接转发，不会经过任何处理 |
| 自动分流 | 通过设定的规则动态控制相关的请求后续如何处理 |
| 全局代理 | 所有的流量都会经过代理转发 |

### 访问方法

访问网络通过`数据包的传递`完成。网站实际上是一个服务器，用户向服务器发送数据包，服务器返回相应的数据包，就完成了访问网站的流程。

每台服务器都有一个`IP地址`，比如百度的IP地址是14.215.177.39，Google的IP地址经常变动。为了便于记忆，用域名把每个IP地址一一对应，这种对应则需要`DNS`。向DNS提供域名，DNS返回正确的IP地址，即可访问到正确的网站。高墙则建在用户和网站之间。可通过以下方式完成与外网的连接。

#### SSH隧道

有一台支持SSH的墙外服务器后，在客户端执行如下命令即可打通隧道。

```text
ssh -D 7001 username@remote-host
```

上述命令中-D表示动态绑定，7001表示本地SOCKS代理的侦听端口，username@remote-host为登录远程服务器的用户名和主机。

#### VPN

现基本无法用于翻墙。比较常见的VPN隧道协议有PPTP VPN、L2TP VPN、OpenVPN、SSH代理等。

**PPTP VPN**

PPTP协议是点对点隧道协议，其将TCP控制包与数据包分开，适合在没有墙限制的网络中使用。

**L2TP VPN**

L2TP能以隧道方式使PPP包通过各种网络协议，但没有任何加密措施，更多是和IPSec协议结合使用提供隧道验证。支持PPTP的设备基本都支持L2TP，需要选择L2TP/IPSec PSK方式且设置预共享密钥PSK

L2TP使用UDP协议，一般可穿透防火墙，适合在有防火墙限制的局域网用户，如公司、网吧、学校场合等使用。PPTP和L2TP二个连接类型在性能上差别不大。

**OpenVPN**

OpenVPN是一个基于SSL加密的纯应用层VPN协议，是SSL VPN的一种，支持UDP与TCP两种方式。通常UDP的效率会比较高，速度也相对较快。其运行在纯应用层，避免了PPTP和L2TP在某些NAT设备后面不被支持的情况，并且可以绕过一些网络的封锁。基本上能上网的地方就能用OpenVPN。

OpenVPN客户端软件可以很方便地配合路由表，实现不同线路（如国内和国外）的路由选择，实现一部分IP走VPN，另一部分IP走原网络。

OpenVPN目前已被墙用流量特征识别等技术手段侦测并受到严重封杀。

**SSH隧道协议**

SSH原本用于UNIX类系统的远程登录和管理。由于SSH可以通过客户端软件，在本地做一个SOCKS代理进行加密转发，因此也被用于网络代理。配合浏览器插件和自动列表，可实现对不同网址有选择性地代理。

**协议比较**

```text
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

#### 代理

**正向代理**

正向代理一般都是C/S架构的，它通过客户端，将你要访问某站的请求打包，然后发给代理服务器，而代理服务器负责将你的请求发送给目标站。类型分为HTTP代理、FTP代理、SSL代理、Socks代理等。正向代理又会根据高度匿名与否，分为高度匿名代理、普通匿名代理、透明代理等。

**Socks代理**

Socks代理通过TCP连接把目标主机和客户端连接在一起，并转发所有的流量，而不关心协议。该代理能在Session层之上的任何端口或协议下运行。Socks4只支持TCP连接，而Socks5在其基础上增加了安全认证以及对UDP协议的支持，即Socks5支持密码认证以及转发UDP流量。

Socks代理在任何情况下都不会中断server与client之间的数据，当墙不允许访问外网时，可以通过Socks代理来上网。

Socks协议非明文，但在Socks代理服务器上可以还原出TCP和UDP的原始流量。

大多数的浏览器都支持Socks代理。浏览器上网的时候需要与目标主机建立TCP连接，这个时候浏览器就会告诉Socks代理，它想与目标主机进行通讯，然后Socks代理就会转发浏览器的数据，并向目标主机发出请求，再把返回的数据转发回来。

**HTTP代理**

HTTP代理跟Socks的原理类似，用处也基本相同，都是让处于防火墙下的主机与外界建立连接。

与Socks代理不同的是，HTTP代理可以中断连接（即在中间截断数据流），因为HTTP代理是以HTTP请求为基础的，而这些请求大多以明文形式存在，所以HTTP代理可以在客户端和下游服务器中间窃听，修改数据。且HTTP不支持转发UDP。

由于HTTP代理只能处理HTTP请求，所以HTTP代理服务器就可以根据你提交的数据来把资源缓存下来，提升访问的速度。很多ISP采用HTTP代理，因为用户都需要访问80端口，而80端口可由ISP进行控制。

HTTP协议属于应用层，而SOCKS协议属于传输层。传输层在网络层之下，这使得Socks代理的作用更为广泛。

**反向代理**

假设大型网站服务器有一台，而后端负责计算的服务器有N台。真正的服务器在这N台服务器，它们通过某些机制同步内容。前端的服务器只负责收发数据，在前端接收到请求之后，就会根据后台某服务器的空闲资源完成请求的反向代理，从而达到了负载均衡的目的。

**透明代理**

此处的透明并不是正向代理的分支，而是路由器上的各种代理，这种代理相对电脑来说是透明的。在路由器上使用透明代理，一般是使用Socks5代理或VPN。

#### 专门协议

包括`v2ray`、`shadowsocks（SS）`、`shadowsocksR（SSR）`、`wireguard`、`brook`、`outline`等，这些协议隐蔽性相对较高，加密方式相对完善。现SSR遭封禁严重，v2ray和SS是主流协议。

这些协议都是通过连接到一台墙未封禁的国外服务器，让该服务器访问被墙封锁的网站后返回相应数据，从而达到翻墙的目的，这些服务器一般称为VPS。`VPS（Virtual Private Server，虚拟专用服务器）`可以理解为安装到电脑上的虚拟机，这些虚拟机相互独立。

**Shaadowsocks**

Shadowsocks有服务端和客户端。客户端监听1080端口，并将数据转发到Socks服务器上。这种转发在会话层，是加密的，所以墙过滤的难度很大。服务端收到数据后将数据发送到目标请求，收到反馈后再传给本地客户端的1080端口，完成TCP连接。

**自建VPS**

自己购买VPS并搭建协议。

**机场**

`机场`指提供节点服务的供应商。`节点`可以理解为服务器，机场会通过`Subscribe`提供等级不同的节点，形式是一个`URL`。

### 安全须知

不要使用360系和百度系的产品，其会自动上报IP地址，很容易使所用服务器遭到封禁。不要用国内浏览器，推荐用`Google Chrome`或`Microsoft Edge`。浏览器可打开`不跟踪`请求，且可将系统语言改为服务器所在地语言会进一步提升安全性。同时尽量减少让国内网站走代理的情况出现，会增加被墙几率。

### 墙封锁技术

#### 网站封锁列表

```text
https://github.com/gfwlist/gfwlist
```

#### 发帖与回溯流程

| 情况 | 浏览流程 | 回溯流程 | 暴露概率 |
| :--- | :--- | :--- | :--- |
| 不用代理 | 发帖人-ISP-服务器托管商-服务器 | 网监-服务器IP-发帖人IP-发帖人ISP-档案 | 100% |
| 一层私有VPN | 发帖人-ISP-VPN-服务器托管商-服务器 | 网监-服务器IP-VPN的IP-路由日志-链接者IP-链接者ISP-办网档案 | 100% |
| 一层共用VPN | 发帖人-ISP-VPN-服务器托管商-服务器 | 网监-服务器IP-VPN的IP-入侵VPN服务器-日志-链接者IP-链接者ISP-办网档案 | 80% |
| 两层私有VPN | 发帖人-ISP-VPN1-VPN2-服务器托管商-服务器 | 网监-服务器IP-VPN2的IP-入侵VPN2服务器-日志查VPN1-路由日志-链接者IP-链接者ISP-办网档案 | 40% |
| 两层共用VPN | 发帖人-ISP-VPN1-VPN2-服务器托管商-服务器 | 网监-服务器IP-VPN2的IP-入侵VPN2服务器-日志查VPN1-日志-链接者IP-链接者ISP-办网档案 | 25% |
| N层私有/公用VPN，发帖人有QQ | 发帖人-ISP-VPN1-VPN2-VPN3-VPN\*n服务器托管商-服务器 | 网监-服务器IP-访问网站-发帖人常用用户名-谷歌（百度）一下-查到QQ-找腾讯查近期登录IP | 100% |
| N层私有/公用VPN，发帖人无QQ | 发帖人-ISP-VPN1-VPN2-VPN3-VPN\*n服务器托管商-服务器 | 网监-服务器IP-访问网站-发帖人常用用户名-谷歌（百度）一下-查到其他论坛注册的ID-找管理员查注册（登录）IP | 100% |
| N层私有/公用VPN，发帖人无其它信息 | 发帖人-ISP-VPN1-VPN2-VPN3-VPN\*n服务器托管商-服务器 | 网监-服务器IP-访问网站-无常用用户名-根据发帖内容（如北京市海淀区XX小区）-监控整个小区宽带-分析-找到IP | 80% |

#### IP封锁

**原理**

墙通过将IP拉入黑名单以完成封锁。Goagent、XX-NET、Hosts方式通过IP访问谷歌或者其他被屏蔽的网站。现在大量谷歌IP被拉黑，而这些方法依然在扫描隐藏的谷歌IP，待IP消耗完毕后，这些方法则会失效。

2018年初开始，墙的封锁方式从全部协议封锁改为了TCP封锁（阻断），主要特征为Ping IP正常连通（ICMP协议），而TCPing IP连接超时无回应（TCP协议）。因此可通过Ping IP和TCPing IP确定服务器IP是否被墙。

服务器IP被TCP封锁（阻断）后，依然可以正常的向服务器发送数据，但服务器返回数据时，墙发现IP在黑名单中，于是就会阻断、拦截，SSR上表现为超时或空连。

目前的代理软件基本都在使用TCP协议进行传输。TCP协议要传输数据先要进行握手，而当墙对海外代理服务器回程TCP阻断的时候，就会导致代理客户端与服务端无法完成握手，自然也无法使用代理。

墙通过分析流量特征判断该链接为代理的可能性，当可能性达到不同的程度时，墙做出不同的处理。其逻辑举例如下（10%/50%/80%等仅用作举例）：

检测到一个链接是代理的可能性低于10%，那么就无视；高于10%，则将IP的相关信息（及收集的证据、特征等）记录到数据库中，当下次再发现这个链接时，继续去收集证据判断是否为代理，若代理几率降低则无视，否则进一步关注；高于50%，可能单独封一个端口（代理端口），若换个端口后继续做代理，则进一步关注，到了80%，所有端口都被封禁。

**解决方法**

转换协议可以复活被墙的IP。转换为ICMP速度太慢，故转换为UDP较好，但国内运营商往往会对UDP QOS限速。能把TCP转为UDP的软件有KCPTun、Dragonite、V2ray（自带KCP协议）、Goflyway（自带KCP协议）。

也可使用CDN中转代理，而该方式需要WebSocket，故可用Goflyway的WS+CDN模式或V2ray的WS+CDN模式。同时需要通过伪装以保证CDN IP的安全性，在使用Goflyway时往往搭配HTTP伪装，而使用V2ray时往往搭配HTTP/HTTPS伪装。HTTP/HTTPS伪装有真正的网页，即浏览器打开域名会看到正常的网页，而代理客户端访问建立代理链接。

也可等待墙解封IP。墙考虑到海外大量的IDC和国内购买海外服务器正常使用者 的体验和情绪，会定期解封IP。每个做代理被墙的海外服务器IP，墙都会根据不同情况设定一个解封时间， 如果解封时间内该海外服务器IP没有向国内发送数据或者尝试建立代理连接，那么墙就会解封该IP。被解封的海外服务器IP，短时间内再用于代理用途，往往很快就又会封禁。

因此可以关闭或删除服务器上面的代理软件，然后闲置该服务器，不要Ping/TCPing这个服务器，一段时间后应当会解封。墙还可能定期对被墙IP的服务器主动探测，看看代理软件是否还在运行，所以必须要关闭代理软件。

**封锁几率**

在日本、美国被墙几率高。

使用SSR代理时可搭配服务端的redirect参数做真实网站，或就用原版混淆插件（plain）。ShadowsocksR服务端伪装成正常网站流量的教程如下。

```text
https://doubibackup.com/hi10k-7p-5.html
```

#### DNS污染

**原理**

DNS污染是指一些刻意制造或无意中制造出来的域名服务器分组，把域名指往不正确的IP地址。

一般来说，网站在互联网上一般都有可信赖的域名服务器，但为减免网络上的交通，一般的域名都会把外间的域名服务器数据暂存起来，待下次有其他机器要求解析域名时，可以立即提供服务。一旦有相关网域的局域域名服务器的缓存受到污染，就会把网域内的电脑导引往错误的服务器或服务器的网址。

假设A为用户端，B为DNS服务器，C为A到B链路中一个节点的网络设备（路由器、交换机、网关等）。现A需访问Google，故A向B通过UDP方式发送查询请求。这个数据在前往B时要经过C。DNS端口为53，C对此进行特定端口监视扫描，对UDP明文传输的DNS查询请求进行特征和关键词匹配分析，从而立刻返回一个错误的解析结果。作为链路上的一个节点，C必定比B更快的返回结果到A，而目前的DNS解析机制策略是只认第一，因此C所返回的查询结果就被A当作了最终结果，由此获得了错误的IP。

干扰也可以通过网络服务提供商提供的DNS服务器进行DNS欺骗，当使用此DNS服务器的网络用户访问此特定网站时，DNS服务便给出虚假的IP地址，导致访问网站失败，甚至返回ISP运营商提供的出错页面和广告页面。

可通过`nslookup`命令查看域名被解析出的IP地址。如果IP与域名服务器明显不同，如`8.7.198.45`，则可证明受到了DNS污染。

全球一共有13组根域名服务器，目前中国大陆有F、I这2个根域DNS镜像，但现在均已因为多次DNS污染外国网络，而被断开与国际互联网的连接。

**解决方法**

对于站长，换域名或者IP即可。

对于用户，可使用翻墙软件。也可在hosts添加受到污染的DNS地址，或使用DNSCrypt软件。

若使用Firefox，则可打开远程DNS解析功能。在地址栏中输入`about:config`， 找到`network.proxy.socks_remote_dns`，改成true即可。

#### SSL连接阻断

墙会阻断特定网站的SSL加密连接，方法是通过伪装成对方向连接两端的计算机发送RST包（RESET）干扰两者间正常的TCP连接，进而打断与特定IP地址之间的SSL（HTTPS，443端口）握手（如Gmail、Google文件、Google网上论坛等的SSL加密连接），从而导致SSL连接失败。

#### HTTP劫持

**原理**

HTTP传输协议是明文的，若网站服务器在海外，访问网站时就要通过国际宽带出口。在通过时，若被墙扫描到违规关键词，墙则会阻断TCP连接，主要错误为链接已重置、该网站已永久移动到其他地址等。

**解决方法**

HTTP劫持可通过加SSL证书解决，网站全部内容都会被加密。但由于HTTPS在建立加密连接的时候需要一次握手，而这次握手是明文的，所以如果域名被重点关注，即使加上了SSL证书，也会在首次握手的时候由于关键词匹配而阻断TCP连接。

另一解决方法为域名备案并使用国内服务器，则不会通过墙。

#### 封锁方式的选择

采用IP封锁时，所有的骨干路由器和国际出口的路由器都添加上和Google服务有关的IP黑名单，带有相关IP地址的数据包直接在半路就被丢包，这会给路由器带来很大负担且成本非常高。这种方法针对的多为非常重要的网站，对于普通网站，采用DNS污染即可。

#### 对于Google的特殊操作

墙对Google部分服务器的IP地址实施某些端口的自动封锁，按时段对`www.google.com`和`mail.google.com`的几十个IP地址的443端口实施自动封锁，具体是每10或15分钟可以连通，接着断开，10或15分钟后再连通，再断开，如此循环，令中国大陆用户和Google主机之间的连接出现间歇性中断，使其各项服务出现问题，由此显得问题出在Google自身。

### 网页

#### 论坛

```text
https://limbopro.xyz/
https://fanqiangdang.com/
https://www.ssrshare.com/
https://fangeqiang.com/
https://www.flyzy2005.com/
https://iyideng.cloud/
https://52bp.org/index.html
https://vpncn.blogspot.com/
https://doubibackup.com/
```

#### 免费VPS

```text
https://freeserver.us/
https://www.youneed.win/
```

#### 待整理

```text
https://program-think.blogspot.com/2010/04/howto-cover-your-tracks-0.html
https://program-think.blogspot.com/2009/09/break-through-gfw-with-tor.html
https://program-think.medium.com/%E5%A6%82%E4%BD%95%E7%BF%BB%E5%A2%99-%E7%B3%BB%E5%88%97-%E5%85%B3%E4%BA%8E-tor-%E7%9A%84%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E8%A7%A3%E7%AD%94-daa1115ac300
https://blog.csdn.net/Angle_Cal/article/details/78249612s

// Privoxy教程
https://www.cnblogs.com/hongdada/p/10787924.html
```

```text
Surge for Mac
破解：
https://blog.cat73.org/20190528/2019052801.surge3-crack/
https://gist.github.com/whyliam/a27bae053207dcb4c46bb5c9cf8ef274
```

```text
https://wiki.kache.moe/2019/12/11/macOS-ClashX/
https://merlinblog.xyz/wiki/ClashX.html

ClashX Pro:
https://github.com/yichengchen/clashX/releases/tag/1.31.1
https://install.appcenter.ms/users/clashx/apps/clashx-pro/distribution_groups/public
https://github.com/V2RaySSR/Tools/blob/master/clash.yaml

远程配置：配置-托管配置-管理
配置文件（可填写服务器信息）：
https://github.com/V2RaySSR/Tools/blob/master/clash.yaml
https://www.hijk.pw/clash_template.yaml 
https://lancellc.gitbook.io/clash/clash-config-file/an-example-configuration-file

也可https://bianyuan.xyz/生成
然后ClashX内配置-打开本地配置文件夹，选择配置好的配置文件并使用

点击 ClashX 状态栏图标，将「出站模式」选为「规则判断」，在「Proxy」或「Global」策略组中可以选择自己喜欢的线路，然后点击「设置为系统代理」即可开始使用。
注：「Proxy」策略组是用于访问国际互联网的默认策略，一般情况下，所有国际网络的访问都通过该策略组中选择的节点进行连接。

点击状态栏上小猫咪图标-》控制台，进入ClashX主界面
在主界面选择代理规则：

首先在主界面的PROXY里选择使用的节点：默认是“自动选择快速节点”，也可以指定使用某个节点。

其次在“Final”选择默认规则：DIRECT表示直连，PROXY走代理。个人建议使用直连，如果网站打不开再选择代理。
在主界面点击“设置”，设置启用系统代理和选择代理模式：代理模式有全局、基于规则路由和直连，(可以认为)对应其他客户端的全局模式、PAC模式和禁用系统代理。绝大多数情况建议使用规则模式。

https://blog.csdn.net/yyws2039725/article/details/90675347

生成终端代理命令：
只能在当前终端执行一次，退出后就需要重新设置




策略组的特性：
可以包含节点或其他策略组
具有多种不同的策略类型
服务于规则
不同策略组的作用：
Global (或Proxy，视配置文件具体情况而定)：主要规则代理分组，需手动选定一个你要使用的节点作为默认的节点。无其它修改的话，所有国际网络的访问都通过该策略组中选择的节点进行连接。
Netflix：指Netflix流媒体的分组，如选择香港节点，则显示港区内容。其它地区的同理。
Spotify：指Spotify流媒体的分组，仅用于加速访问。Spotify执行锁区政策，根据账号注册时所在地进行内容展示，更换节点并不能显示其它地区的版权内容。
YouTube：指YouTube流媒体的分组，使用不同的线路将展示不同地区的特色内容。
Telegram：指Telegram（电报）的分组，国内手机号注册的用户选择新加坡线路也许可以加速（注意：是也许。因为+86号段注册的电报用户数据都在新加坡数据中心）。
China：访问中国大陆网站所用的策略。大陆用户请选择 DIRECT （直接连接）；海外用户请选择回国线路。
Auto：软件每隔一段时间会自动进行测试并排序，自动评选出延迟最低的节点，当其它策略组里选择了 auto 策略，那么效果就是该策略组将每隔一段时间自动切换到延迟最低的线路。
（如果你有登陆脸书、IG等社交媒体，不建议使用该策略，以免由于IP变动造成风控，甚至封号。）
fallback：可用性策略。与故障转移类似，按照节点顺序选择第一个可用节点。
```

```text
// 订阅转换：
Sub-store教程：
https://github.com/Peng-YM/Sub-Store
https://www.notion.so/Sub-Store-6259586994d34c11a4ced5c406264b46
https://github.com/Peng-YM/Sub-Store/tree/master/config

-添加
1. Loon
打开 Loon 并点击下方 配置 栏，点击 插件
+，URL：
https://raw.githubusercontent.com/Peng-YM/Sub-Store/master/config/Loon.plugin
确保 PROXY 栏右侧区域不出现任何文字，点击 保存 即可
或手动配置：
[MITM]
hostname=sub.store

[Script]
http-request https?:\/\/sub\.store script-path=https://raw.githubusercontent.com/Peng-YM/Sub-Store/master/backend/sub-store.js, requires-body=true, timeout=120, tag=Sub-Store

2. Surge
目前iOS商店版本的bug未修复，暂时无法使用。TF用户直接使用模块。
模块订阅，外部资源刷新
https://raw.githubusercontent.com/Peng-YM/Sub-Store/master/config/Surge.sgmodule

3. QX
QX暂时需要通过backend方式使用，添加如下配置。注意，HTTP backend开关需要打开！

[http_backend]
https://raw.githubusercontent.com/Peng-YM/Sub-Store/master/backend/sub-store.js, tag=Sub-Store, path=/, enabled=true

-访问
Loon & Surge
浏览器打开以下网址，可添加到主屏幕：
https://sub-store.vercel.app
QuanX
通过JSBox版本：从JSBox的Erots商店获取，一些有用的脚本：
https://github.com/LiuGuoGY/JSBox-addins
https://xteko.com/redir?name=Light%20Store&url=https%3A%2F%2Fraw.githubusercontent.com%2FZiGmaX809%2FJsBoxLib%2Fmaster%2FLight_Store%2FLight%2520Store.box
在JSBox打开脚本，点击设置，查看环境是不是quanx，不是的话就要切回quanx


使用：
订阅，+号，填写订阅名称和地址，设置好后点进去可以看到节点
可以多个订阅组合在一起，合并成一个不订阅，订阅-文件夹符号即可

本地解析：
在要修改的订阅三个点-编辑，
常用选项：过滤节点
节点操作：添加各个过滤模式，同一类型过滤模式可以添加多个，会按照顺序执行
保留模式下关键词过滤只能单个匹配，需要添加多个过滤器实现多关键词
过滤模式则可匹配多个词
关键词重命名可以一次操作多个关键词
脚本操作可以远程脚本或者本地脚本，脚本库：
https://github.com/Peng-YM/Sub-Store/tree/master/scripts


// 其余同类型作品：
Surgio：
https://surgio.js.org/
https://github.com/surgioproject/surgio

subconverter：
https://github.com/tindy2013/subconverter

ConfigConverter:
https://github.com/ImSingee/ConfigConverter

// 订阅类型转换
https://bianyuan.xyz/
```

## 现成工具

### 谷歌镜像网站

```text
自建：
https://github.com/aploium/zmirror
```

```text
http://google.seek68.cn/
https://x.glgoo.top/scholar/
https://ac.scmor.com/
https://v2ray.party/
http://scholar.hedasudi.com/
https://ac.scmor.com/
https://google2.jiongjun.cc/
```

库如下。

```text
https://www.uedbox.com/post/54776/
```

### 电脑端

#### Chrome插件

**谷歌访问助手破解版**

为Chrome插件，解压后拖到`扩展程序`即可。

```text
https://www.lanzous.com/ial9x8b
```

**其余插件**

```text
http://googlehelper.net/
http://www.ggfwzs.com/
https://setupvpn.com/download/
https://tunnello.com/
```

#### 软件

**无界浏览**

```text
// Windows
https://www.lanzous.com/i4eyu4b

// Mac
https://www.lanzous.com/i4eyt1c
```

**JJQQKK**

现只能免费使用三天。

```text
https://github.com/jjqqkk/chromium
```

**翻墙工具包**

```text
https://github.com/Alvin9999/new-pac/wiki
```

**VPN Plus/Hotspot Shield**

可在`美区`的Mac App Store找到。注意，尽管第一款软件在中区Mac App Store有上架，但下载后无法使用。

**OpenVPN**

一般不使用。

```text
https://www.vpngate.net/en/howto_l2tp.aspx
https://www.vpnbook.com/freevpn
https://docs.getfoxyproxy.org/index.php/Main_Page
https://www.freeopenvpn.org/en/cf/usa.php
https://www.sparklabs.com/viscosity/
```

**其余电脑端软件**

```text
https://chn.windscribe.com/download
https://protonvpn.com/download
https://psiphon.ca/
https://privatix.com/
https://tuxler.com/
http://ipunblock.com/
```

**TunSafe**

使用WireGuard协议的高性能且安全的VPN客户端。

```text
https://tunsafe.com/
https://hoochanlon.github.io/fq-book/#/vpn/classical-vpn
```

#### SSH隧道

**使用**

```text
https://hoochanlon.github.io/fq-book/#/proxy/SSH-Tunnel
```

**客户端**

```text
https://www.bitvise.com/ssh-client-download
```

**公用账号**

```text
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

#### WireGuard

```text
https://www.wireguard.com/
https://hoochanlon.github.io/fq-book/#/vpn/wireguard
```

### 手机端

#### Betternet/VPN 360

可在美区的App Store找到。

#### VPN Plus

iOS版可在美区的App Store找到。安卓版下载地址如下。

```text
（Android）https://www.lanzous.com/i4ez8wd
```

#### 老王VPN

可在Google Play下载。

```text
https://www.lanzous.com/i4ezbla
```

#### 蚂蚁加速器

```text
https://b.lausera.com/c-1/a-agWjQ/
```

#### 其余Android软件

```text
https://play.google.com/store/apps/details?id=free.vpn.unblock.proxy.turbovpn&hl=zh_CN
https://play.google.com/store/apps/details?id=net.upx.proxy.browser&hl=zh_CN
https://play.google.com/store/apps/details?id=com.findtheway&hl=zh_CN
https://play.google.com/store/apps/details?id=com.fast.free.unblock.secure.vpn&hl=zh_CN
https://play.google.com/store/apps/details?id=free.vpn.unblock.proxy.vpnpro&hl=zh_CN
https://play.google.com/store/apps/details?id=com.tunsafe.app&hl=zh_CN
https://play.google.com/store/apps/details?id=com.opera.max.global&hl=zh_CN
https://play.google.com/store/apps/details?id=com.github.shadowsocks&hl=zh_CN
https://play.google.com/store/apps/details?id=com.v2ray.ang&hl=zh_CN
https://play.google.com/store/apps/details?id=vpn.look.www.sharevpn&hl=zh_CN
https://play.google.com/store/apps/details?id=easyvpn.free.vpn.unblock.proxy&hl=zh_CN
https://play.google.com/store/apps/details?id=in.teleplus&hl=zh
```

### 代理

该方法适合所有的系统，包括Linux和较旧的Windows系统，如XP。

#### 寻找代理节点

**通过网站**

在Google搜索`免费代理节点`即可。

```text
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

**通过工具**

下载`ProxyScrape`，打开后选择`Check proxies`，填写所要求的代理服务器的`ping值`，ping值越大，表明延迟越大。一般来说，公用的服务器ping值都会在100ms以上，因此可取400-500。填写检查`代理服务器的数量`，选择`代理服务器的协议类型`，选择`Check from ProxyScrape.com`，工具就会开始筛选代理，并把可用的代理用绿色表示出来。

```text
https://proxyscrape.com/proxy-checker
```

#### 配置网络代理

**设置软件代理**

以网易云音乐为例，点击`设置`-`工具`，在代理选项卡下有三个选项，若选择使用IE代理设置，则需要提前设置好IE的代理。如果选择`自定义代理`，则在下拉菜单中选择代理类型，并输入服务器与端口（比如说所得结果为`1.2.3.4:5`，则服务器地址为`1.2.3.4`，端口为`5`），保存即可。

**设置IE代理**

打开IE，点击右上方`设置`-`Internet选项`，选择`连接`-`局域网设置`，勾选`为LAN使用代理服务器`，填好地址和端口，并勾选`对于本机地址不使用代理服务器`，保存并退出。

**设置全局代理**

打开系统的`设置`-`网络和Internet`-`代理`，选择`使用代理服务器`，填好地址和端口，并保存。

**设置浏览器代理**

以Chrome为例，打开`Chrome网上应用店`，搜索`SwitchyOmega`并安装（需要外网环境）。

下载完成后点击插件的`选项`，进入配置页面。点击情景模式的`proxy`，填写代理协议、代理服务器和端口。点击`auto switch`，切换规则只留`proxy`，默认情景模式设为`直接连接`，在导入在线规则列表下，点击`添加规则列表`并填入下面网址，点击`立即更新情景模式`，保存后退出。此时可以禁用系统设置中的网络代理。

点击插件，选择`auto switch`或`proxy`，即可访问外网。其中proxy相当于`全局模式`，auto switch相当于`PAC模式`。

```text
https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt
```

### 应急使用

#### 友明互联

0.01元/天。

```text
http://idc.ymhlw.cn/index.php/buy/index/
```

#### 乐云

2元/月。

```text
https://www.renzhijia.com/buy/index/7/
```

#### 公用节点

```text
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
```

## 反代网站

### 现成网站

```text
http://webproxy.to/
https://weboas.is/
https://www.croxyproxy.com/
```

### 自行搭建

通过cloudflare的worker项目，可以建立一个简单的代理项目。打开第一个网页，完成注册后登录。然后打开第二个网页并点击`Start building`，子域名可以任意填写，计划选择免费，并创建worker。

```text
https://dash.cloudflare.com/
https://workers.cloudflare.com/
```

进入worker页面，更改三级域名，删除原始脚本。然后打开以下网页，复制代码到worker的脚本处，点击下方`保存并部署`，然后进行预览。使用时直接输入域名对应的网址即可。

```text
https://github.com/yangmyc/jsproxy/blob/master/cf-worker/index.js
```

其余同类型仓库如下。

```text
https://github.com/netptop/siteproxy
```

## 自建服务器

### Google Cloud Platform

#### 申请

在Google搜索GCP即可进入Google Cloud Platform的官网。GCP是谷歌提供的服务器，新用户注册可免费获取300美元的赠金，在12个月内使用。

注册GCP的关键是通过银行卡认证。注册完成后点击左上角的菜单，选择`结算`，可以看到剩余赠金和剩余试用日期。信用卡验证相关操作可查看附录。

#### 搭建

**创建防火墙规则**

点击菜单中的`VPC网络`-`防火墙规则`，进入防火墙规则页面后，点击`创建防火墙规则`，进入防火墙规则创建界面。

名称改为`entrance`（其他名称也可以），优先级设为`1`，流量方向为`入站`，对匹配项执行的操作选择`允许`，目标选择`网络中的所有实例`，来源IP地址范围填写`0.0.0.0/0`，协议和端口选`全部允许`，点击`创建`即可。

同理，再创建一个防火墙实例，名称改为`exit`，流量方向为`出站`，其余与上面的一致，保存。

**创建VM实例**

点击菜单中的`Compute Engine`-`VM实例`，点击`创建`，进入VM实例创建页面。

名称任意，区域建议选择`asia-east2（香港）`或`asia-east1（台湾）`，地区任意，机器类型选择`微型`，防火墙勾选`允许HTTP流量`、`允许HTTPS流量`。

下面的标签页切换到`网络`一栏，在`网络标记`一栏填入刚才创建好的两个防火墙规则的名字，点击最下方的`创建`，完成VM实例创建。

创建完成后会出来一个内部IP和一个外部IP，打开命令提示符（Win+R后cmd回车），输入`ping 外部IP`（比如`ping 35.XXX.XXX.XXX`），查看ping值。

如果ping值过大，则再创建一个新的实例，直到得到一台ping值小的服务器。注意不要把原来的实例删除，不然会分配到同一台服务器。正常服务器ping值在100ms以内。

**搭建v2ray协议**

**运行一键脚本**

点击VM实例上的`SSH`按钮，等待命令行窗口出现。在命令行中输入以下命令以获取管理员权限，并执行v2ray一键安装脚本。

```text
sudo -i
bash <(curl -s -L https://git.io/v2ray.sh)
```

**协议配置**

具体协议配置如下。

**TCP协议**

在一键脚本配置协议时选择默认的`TCP`协议即可。TCP协议速度快，但有被封端口的风险。

**WS+TLS协议**

该协议安全性高，但需要域名且速度不及TCP协议。

进入以下网站并注册一个cloudflare账号，然后把刚才注册的域名加进去，添加时选择免费套餐即可。

```text
https://www.cloudflare.com/zh-cn/
```

向域名的`DNS`添加一个A记录，Name为二级域名，比如`hk.nionguan.ga`，Value为服务器的IP地址，注册时`Proxy status`点成`DNS Only`。

然后到购买域名网页的后台，更改域名的Nameserver为cloudflare中指定的网址。更改完成后返回cloudflare，点击`Done, Check...`并等待生效。

**多用户配置**

请在完成TCP协议和WS+TLS协议的配置后进行以下操作。

在服务器输入以下命令以编辑v2ray的配置文件。

```text
vim /etc/v2ray/config.json
```

找到inbound段的代码并进行如下修改，注意每个用户的UUID应当一致。

```text
"inbounds": [{
  "port": 8089, // 高稳定用户
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
  "port": 8288, // 普通用户
  "protocol": "vmess",
  "settings": {
    "clients": [{
      "id": "71880ee2-4d15-47da-87b2-xxxxxxxxxx",
      "alterId": 64
    }]
  }
}]
```

修改完成后重启v2ray服务即可生效。

```text
sudo systemctl restart v2ray
```

**配置分享**

得到v2ray配置信息后复制，并输入`v2ray ssqr`生成二维码，便于以后使用。

#### 使用

**快照**

GCP提供的快照功能能够备份磁盘内容，当内容丢失后可以用于恢复。新建VM实例时也可通过快照建立。

点击Compute Engine下的快照，建立即可。建立完成后，直接点击快照详情，即可用该快照建立实例，其磁盘内容完全一致。

**镜像**

GCP可通过Docker Hub上的镜像部署VM实例。在部署示例时勾选`将镜像应用到该实例`，并复制该实例的完整地址即可，如`hub.docker.com/_/wordpress`。

### GoormIDE

GoormIDE可以永久免费使用一个含1GB内存、10GB存储空间的VPS，但不能保持常开。

#### 申请

打开以下链接并完成注册即可。

```text
https://ide.goorm.io/
```

#### 搭建

进入控制台后点击`New Container`。地区最好选择韩国，类型选择Private，选择新建。新建完成后点击Run，然后点击Terminal进行连接。

**Socks5连接协议**

输入以下命令安装。

```text
bash <(curl -s -L https://raw.githubusercontent.com/guleonseon/goorm-auto/master/install.sh)
```

回到VPS设置页，在Port Forwarding处填写`1080`并点击完成。在客户端添加以下配置即可。

```text
类型 / socks5
地址 / VPS的IP地址
端口 / 原端口（不是1080）
```

**vmess连接协议**

输入以下命令以通过一键脚本安装，安装完成后记下配置，不要关闭窗口。

```text
bash <(curl -s -L https://git.io/v2ray.sh)
```

回到VPS设置页，在Port Forwarding处填写刚才记下的端口号并点击完成，然后记下后面系统分配的新的端口号。在添加连接时，需要用新端口号替换旧的端口号，其余配置与脚本提供的一致。

返回终端页，运行以下命令以保持v2ray运行。使用过程中需一直停留在该标签页上，每次重新开启服务器时，需查看配置是否被改变。

```text
/usr/bin/v2ray/v2ray -config /etc/v2ray/config.json
```

### Heroku

#### 申请

打开以下链接并注册，注意需要翻墙环境。

```text
https://dashboard.heroku.com/
```

#### 搭建

**v2ray**

打开以下链接部署v2ray，注意数据中心即为翻墙服务器地址。

```text
https://dashboard.heroku.com/new?template=https%3A%2F%2Fgithub.com%2Fbclswl0827%2Fv2ray-heroku
```

完成部署后在后台查看刚才新建的项目，点击`Reveal Config Vars`并复制UUID，在`Domains`下复制域名。打开客户端并进行配置，其中地址为刚才的域名，端口为443，用户ID为刚才的UUID，alterID为64，传输协议为ws，打开TLS并允许不安全连接即可。

**Shadowsocks**

步骤基本同上，仓库如下。

```text
https://github.com/onplus/shadowsocks-heroku
```

客户端配置中，地址为app域名，端口为80，混淆为websocket，路径为`/`。

### EUserv

#### 申请

打开以下链接以注册。注意注册时地址选择中国，地址尽量真实，不需要翻墙环境。审核需要24-48小时，会核对注册时所用的IP地址和选择的地区是否匹配。若注册不通过则会被删除账号。

```text
https://www.euserv.com/en/virtual-private-server/root-vserver/v2/vs2-free.php
```

#### 搭建

**重装系统**

在后台点击`VSERVER`，看到服务器申请状态成功，即可点击`Select`进入服务器详情。进行系统安装时系统选择`CentOS 7`，然后点击左侧的`Serverdata`查看服务器信息，若Default-Password出现，则系统安装成功。

**SSH连接**

由于该服务器只提供IPV6地址，而普通路由器不具备解析IPV6地址的能力，因此需要通过其它方式进行连接。

**通过手机热点**

手机热点自带IPV6地址。将电脑连接手机热点后，输入以下命令以确认服务器是否能够连接。

```text
// Windows
ping -6 [IPV6地址]

// Mac
ping6 [IPV6地址]
```

然后通过正常的SSH连接即可。

也可在手机端直接完成。安装`1.1.1.1`这一APP并连接，然后通过SSH软件连接即可。

**通过网站**

打开以下网站并注册，然后依次新建组织、项目、资产。

```text
https://console.heyterm.com/
```

其中新建资产时相关配置如下。

```text
IP地址 / IPV6地址或服务器域名
用户名 / root
密码 / Default-Password的内容
服务器节点 / 荷兰或德国
```

新建完成后点击终端按钮即可完成连接。

**IPV4配置**

输入以下命令以更改DNS解析，使其可访问IPV4网络。

```text
echo -e "nameserver 2001:67c:2b0::4\nnameserver 2001:67c:2b0::6" > /etc/resolv.conf
```

可通过以下命令查看是否设置成功。

```text
vim /etc/resolv.conf
```

然后通过以下命令修改文件，将`enable=1`改为`enable=0`后按照vim的方法保存（按ESC后输入:wq并回车）。

```text
vi /etc/yum/pluginconf.d/fastestmirror.conf
```

然后执行以下命令以进行升级并安装带有IPV6的宝塔。

```text
yum update
curl -sSO http://download.bt.cn/install/new_install.sh && bash new_install.sh
```

安装完成后输入`bt`进入管理界面，执行8修改端口为8080，执行5和6修改用户名和密码。退出管理脚本后执行以下命令以取消登录面板安全保护。

```text
rm -f /www/server/panel/data/admin_path.pl
```

为访问宝塔面板，需要进行域名解析。进入Cloudflare的后台，在`DNS`页选择一个域名后添加一条记录，内容如下。

```text
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

**宝塔配置**

完成添加后通过以下链接访问宝塔面板，输入改好的用户名和密码即可登录。

```text

```

登录后安装推荐的LNMP，安装完成后点击左侧的`软件商店`，分类选择`一键部署`，选择`博客`类型下的`WordPress`。域名填写登录宝塔面板的域名，注意不包括端口号，然后提交。

复制弹出窗口中的数据库账号资料，然后点击访问站点下的链接进行部署，其中相关信息需要填写刚才复制下来的内容。点击安装并等待完成即可。

部署完成后回到宝塔面板，点击左侧的`网站`，可以看到刚才部署的WordPress博客。点击`设置`，在`SSL`下选择`Let's Encrypt`，勾选域名并安装SSL证书。完成后可访问博客，查看能否正常运行。

**安装v2ray**

在完成网站搭建后即可开始安装v2ray。通过以下命令使用一键脚本安装即可。

```text
bash <(curl -s -L https://git.io/v2ray.sh)
```

安装完成后利用宝塔面板的文件功能打开etc/v2ray/config.json，用以下配置替换。

```text
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

其中clients-id可在服务器用以下命令生成。

```text
cat /proc/sys/kernel/random/uuid
```

完成后回到宝塔面板，点击`网站`-`设置`-`配置文件`，在最后一个`}`前加入以下代码。

```text
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

然后输入以下命令使v2ray和nginx开机自启，并重启v2ray使配置生效。

```text
systemctl enable v2ray nginx
systemctl restart v2ray
```

然后需要在宝塔打开端口。点击`安全`，添加上述所使用的端口为放行端口，此处为10000。添加完成后服务端配置即完成，客户端配置如下。

```text
地址 / 访问WordPress的地址
端口 / 443
用户ID / 配置文件的clients-id
额外ID / 64
传输协议 / ws
路径 / 配置文件的streamSettings-path
TLS / 打开，允许不安全
```

注意，安装完成后千万不要运行bbrplus加速脚本，否则将导致服务器无法连接。

### Okteto（失效）

#### 注册

打开以下网页并用Github登录。

```text
https://cloud.okteto.com
```

需在本机安装Okteto工具以完成部署。对于MacOS，在终端运行以下命令。

```text
brew install okteto
```

对于Windows，下载以下两个文件并放到`C:\Windows\System32`。

```text
https://downloads.okteto.com/cli/okteto.exe
https://storage.googleapis.com/kubernetes-release/release/v1.18.0/bin/windows/amd64/kubectl.exe
```

其它平台可查看以下链接。

```text
https://okteto.com/docs/getting-started/index.html
https://kubernetes.io/docs/tasks/tools/install-kubectl
```

macOS下进入终端，Windows下管理员身份运行cmd，执行以下命令。如果无法登录，则更换系统环境。

```text
okteto login
okteto namespace
```

#### 搭建v2ray

新建文本文件，名称为`v2ray.yml`，内容如下。

```text
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

```text
kubectl apply -f [yml文件路径]
```

在客户端添加以下配置即可。

```text
地址 / Endpoints的地址
端口 / 443
用户ID / f3c9cb27-746f-4e41-acf2-820bd3002676
传输协议 / ws
路径 / /fuckgfw_letscrossgfw
TLS / 打开，允许不安全
额外ID / 100
```

#### 关于Docker的说明

上述yml文件可用于安装大部分镜像。镜像（Docker）是只读的，可在hub docker上找到，相当于打包好的系统，部署到服务器后即可直接使用。部署到服务器即为容器（Container）。

将文件中的`v2-app`改为另一个名称，`pch18/baota:clear`更换为其他容器，`port: 8888`改为容器内暴露端口，其中暴露端口可看相关镜像的说明。

保存后执行以下命令部署即可。

```text
kubectl apply -f [yml文件路径]
```

仓库和示例如下。

```text
https://github.com/pch18-docker/baota
```

**搭建宝塔**

完成后通过所给的网站即可访问。用户名为`username`，密码为`password`。

```text
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

**搭建Google镜像网站**

完成后通过所给的网站即可访问。

```text
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

**搭建Linux**

完成后通过所给的网站即可访问，默认以root身份登录。密码为`vncpassword`。

```text
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

```text
consol/centos-xfce-vnc
consol/ubuntu-xfce-vnc
consol/centos-icewm-vnc
consol/ubuntu-icewm-vnc
```

#### 已失效教程

打开以下仓库并fork到自己的Github上。

```text
https://github.com/byxiaopeng/okteto-reboot
```

在自己Fork的项目下点击`Settings`-`Secrets`，建立以下Secret。

```text
名称 / APITOKEN
内容 / 在Okteto控制台点击Settings，复制API Token到此

名称 / NAMESPACE
内容 / 控制台左上角的标注
```

回到控制台，点击`Deploy`，选择`Deploy from Git Repository`，仓库地址为刚才Fork的仓库，进行部署。

部署完成后，在客户端进行以下配置即可。

```text
地址 / Endpoints的地址
端口 / 443
用户ID / ad806487-2d26-4636-98b6-ab85cc8521f7
传输协议 / ws
路径 / /ws
TLS / 打开，允许不安全
```

### Kubesail（失效）

现在需要自行提供服务器。

#### 申请

打开以下链接并完成注册。

```text
https://kubesail.com
```

#### 搭建

打开以下链接，Fork一份到自己的Github。然后在Kubesail后台点击`Repos`，确认已经连接到自己的Github账号。

```text
https://github.com/bclswl0827/v2ray-openshift
```

打开以下链接，Fork一份后点击左侧的`Templates`，进入刚才fork的项目，点击`Settings`，将`Container Image`的路径更改为`[Github用户名]/v2ray-openshift`。点击`Launch Template`开始部署。点击`Status`，看到v2ray在运行时则部署成功。

若部署失败，则点击左侧的`Resources`，将`Apps`下项目的`Settings`-`Container Image`更改回`bclswl0827/v2ray-openshift`，保存。

```text
https://kubesail.com/template/bclswl0827/v2ray
```

部署成功后需要将项目暴露在域名下。点击`Domains`，添加一个已经在CloudFlare部署好的网址，注意不要保留二级域名和其它路径。系统将会要求添加一个TXT记录完成验证，在CloudFlare后台添加即可，名称填`@`或者`root`。完成验证后按照提示添加一个CNAME记录，名称填`@`或者`root`，内容填系统所给出的网址，类型选择`DNS Only`，保存。

完成网址部署后返回刚才的项目，点击`Network`-`Ingress`，`Domain`选择刚才新建的域名，`subdomain`留空，保存。

至此已将项目暴露到网页上，在客户端添加相关配置即可，具体如下。

```text
地址 / 绑定的域名，注意没有二级域名和其它路径
端口 / 443
用户ID / 在项目中点击Edit YAML，在clients-id下
AlterID / 64
传输方式 / ws
路径 / /ws
TLS / 打开，且允许不安全
```

### IBM Cloud（失效）

免费30天，免费期后会自动注销账号。

#### 注册

打开以下链接进行注册即可。

```text
https://cloud.ibm.com/
```

#### 搭建

进入后台并点击`创建资源`，创建一个Cloud Foundray下的公共应用程序。区域选择达拉斯，套餐选择256MB，配置资源选择Go，名称填ibmyes，完成创建。

创建完成后进入项目并点击右上角的终端符号以进入终端，注意切换地区为Dallas。然后运行以下脚本进行安装即可。安装完成后用分享链接即可导入配置到客户端。

```text
wget --no-check-certificate -O install.sh https://raw.githubusercontent.com/CCChieh/IBMYes/master/install.sh && chmod +x install.sh  && ./install.sh
```

由于IBM Cloud十天不操作就会关机，所以需要十天重启一次。在IBM Cloud终端输入以下命令以获取相关参数。

```text
ibmcloud login
ibmcloud resource groups
```

其中Region中显示的地区需转换为区域编号，即`REGION_NUM`，对应关系如下。而第二个命令执行后ID一栏为资源组id，即`RESOURSE_ID`。

```text
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

```text
https://github.com/CCChieh/IBMYes
```

在自己Fork的项目下点击`Settings`-`Secrets`，建立以下四个Secret。

```text
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

## 客户端配置

### 使用

#### iOS端说明

未越狱iOS端没有官方的v2ray客户端。常用的第三方客户端有Shadowrocket、Quantumult和Kitsunebi等，均需在美区App Store下载。

**Kitsunebi**

支持v2ray（WS/TCP）、Shadowsocks。

打开Kitsunebi，选择下方的`服务器`，点击右上方`+`号并选择`扫二维码`，扫描刚才生成的二维码，即可导入服务器配置。然后把操作模式改到`Rule`，传出代理选择刚才添加的服务器。完成设置后，在状态页开启VPN开关即可。

**Shadowrocket**

支持v2ray（WS/TCP）、Shadowsocks、Trojan。

**Quantumult**

支持v2ray（WS/TCP）、Shadowsocks。

#### v2ray

**Windows（高于XP）**

**v2rayN**

下载v2rayN-Core.zip，解压并打开`v2rayN.exe`。点击服务器，选择`扫描屏幕上的二维码`，客户端会自动扫描刚才生成的二维码并添加配置信息。如果无法生成二维码，则点击`添加Vmess服务器`，手动输入刚才保存的配置信息。右键点击新增的服务器，选择`设为活动服务器`。然后在v2ray的托盘图标点击右键，勾选`启用http代理`，并在http代理模式中选择`PAC模式`，即可上网。

```text
https://github.com/2dust/v2rayN/releases/
```

**Windows XP**

**安装**

从以下链接下载Alvin9999包含v2ray作为翻墙工具的Chrome翻墙浏览器包，此处为AllNew全新版。

```text
https://github.com/Alvin9999/new-pac/wiki/%E4%BD%8E%E5%86%85%E6%A0%B8%E7%89%88
```

提取压缩包中的v2ray文件夹，并修改里面的config.json为如下内容。

```text
{
  "log": {
    "access": "",
    "error": "",
    "loglevel": "warning"
  },
  "inbound": {
    "port": 1080,
    "listen": "127.0.0.1",

// procotol为v2ray所映射代理的协议，默认为socks（socks5），由于XP原生不支持socks5，故改为http
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

精简文件夹，只保留doc文件夹、config.json、geoip.dat、geosite.dat、vpoint\_socks\_vmess.json、vpoint\_vmess\_freedom.json、v2ctl.exe、v2ctl.exe.sig、v2ray.exe、v2ray.exe.sig、w2vray.exe、w2ray.exe.sig。此时双击v2ray.exe即跳出v2ray窗口，示意v2ray已经运行。若运行wv2ray.exe，则不显示窗口，在后台静默运行。

**使用**

同理，这时v2ray已经把服务器映射到本地代理127.0.0.1:1080，注意类型为`http`。XP中可以配置IE代理，类型选择`http`，服务器地址为`127.0.0.1`，端口为`1080`。也可在Chrome浏览器中使用SwitchOmega配置。

若需要实现全局翻墙，则需要用到翻墙工具Proxifier。

安装并打开软件，点击`Profile`菜单下的`Proxy Servers`，添加代理，配置同上。然后点击`Proxification Rules`修改翻墙规则，其中v2ray.exe、wv2ray.exe和翻墙服务器的地址（34.xx.xx.xx）需要使用direct模式，否则将会出现循环代理的情况。配置完成后点击`Name Resolution`，勾选`Resolve hostnames through proxy`以防止DNS污染，保存即可。使用时打开v2ray.exe或wv2ray.exe，并打开Proxifier的开关即可。

为启动方便，可通过以下bat程序一键打开和关闭全局翻墙。打开记事本，复制代码并保存为相应文件后执行即可。

```text
// 保存为startglobal.bat
start wv2ray.exe
stop Proxifier.exe

// 保存为stopglobal.bat
taskkill /f /im Proxifier.exe
taskkill /f /im wv2ray.exe
```

由于Proxifier支持socks5协议，而SwitchOmega在XP下并不支持，因此若v2ray选用socks5协议，只能实现全局翻墙而不能实现单一浏览器翻墙。

除了v2ray包，Alvin9999的AllNew包中goflyway代理工具也可用。双击打开后可见本地代理，在浏览器中填写相应地址即可。

**Mac**

**v2rayX**

与Windows客户端的配置类似。下载zip包并解压，将`V2RayX.app`复制到应用程序文件夹后打开，点击菜单中的`Configure`，输入刚才保存的服务器配置信息。然后在`Servers`中选择刚才新建的服务器，确保v2ray code处于`load`状态，并勾选`PAC Mode`，配置完成。

```text
https://github.com/insisttech/v2rayX-copy/releases
```

**v2rayU**

```text
https://github.com/yanue/V2rayU/releases/
```

**Android**

**v2rayNG**

下载`app-universal-release.apk`，安装到手机并打开。点击上方`+`号并选择`从二维码导入配置`，扫描前面生成的二维码，这样服务器配置就添加进来了。同理，如果二维码无法生成，则点击`手动输入`。

然后点击左上角的菜单，选择设置，在路由模式中选择`绕过中国大陆`。回到主页面，点击右下角的启动按钮，允许连接。

```text
https://github.com/2dust/v2rayNG/releases
```

**Linux**

Linux没有图形客户端，故须按照以下配置。

下载`v2ray-linux-64.zip`，解压后放于`/tmp/v2ray`，并改名为`v2ray.zip`。

```text
https://github.com/v2ray/v2ray-core/releases
```

打开终端并输入命令，下载好的脚本会放于终端工作路径（默认为主文件夹，即`/home/用户名`）。若go.sh无法下载，可使用文末附有的代码。

```text
wget https://install.direct/go.sh
```

用文本编辑器打开下载好的go.sh，找到`downloadV2Ray()`函数，删除函数内所有代码并`return 0`，如下所示。

```text
downloadV2Ray(){
    return 0
}
```

修改完成后在终端输入命令，等待v2ray安装完成。

```text
./go.sh
```

安装完成后输入以下命令以打开文件管理器。

```text
sudo nautilus
```

转到`/etc/v2ray/config.json`，打开并将内容用下面的代码覆盖。

```text
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

保存后在终端`Ctrl+C`并输入下面的命令，使v2ray开机自启，并启动v2ray查看状态。如果显示绿色，证明v2ray已成功运行。

```text
sudo systemctl enable v2ray
sudo systemctl start v2ray
sudo systemctl status v2ray
```

这时v2ray已经把服务器映射到本地代理127.0.0.1:1080，按照配置系统代理的方式即可使用，类型选择`Socks5`，服务器地址为`127.0.0.1`，端口为`1080`。配置系统代理后就可以配置浏览器代理，以后使用时，可以只打开浏览器代理，而关闭系统代理。

#### Shadowsocks

**Windows**

**shadowsocks-windows**

```text
https://github.com/shadowsocks/shadowsocks-windows/releases/
```

**Mac**

**ShadowsocksX-NG**

```text
https://github.com/shadowsocks/ShadowsocksX-NG/releases/
```

**Android**

**shadowsocks-android**

```text
https://github.com/shadowsocks/shadowsocks-android/releases/
```

**iOS**

**Shadowsocks**

越狱设备可安装Shadowsocks插件。自带Bigboss源有该插件。

**Linux**

```text
https://github.com/shadowsocks/shadowsocks-qt5/releases/
```

#### Trojan

**Windows**

**v2rayN**

与v2ray的Windows客户端相同。

**Android**

**igniter**

```text
https://github.com/trojan-gfw/igniter/releases/
```

#### 全平台

**Trojan-Qt5**

```text
https://github.com/charlieethan/Trojan-Qt5/releases
```

**Qv2ray**

打开以下网站下载Qv2ray软件以及SSR/Trojan插件。

```text
https://github.com/Qv2ray/Qv2ray/releases
https://qv2ray.github.io/
```

打开Qv2ray，点击`插件`，选择`打开本地插件目录`，将下载的插件复制到此目录，重启应用即可加载插件。然后打开以下网站，下载v2ray核心。在Qv2ray的安装目录下新建一个空文件夹，名称可以任意取，然后将解压后的v2ray核心文件拖入其中。

```text
https://github.com/v2ray/v2ray-core/releases
```

打开Qv2ray，点击`首选项`-`内核设置`，将`核心可执行文件路径`设置刚才所新建的文件夹下的v2ray.exe目录（Mac为v2ray文件），`V2ray资源目录`设置为刚才所新建的文件夹下的目录，然后点击`核心验证`确认配置完成。

### 二级代理

二级代理即为在数据从服务端传回客户端时并不直接传回，而是通过另一个服务器中转。

#### Shadowsocks

准备好客户端及其副本。打开客户端并连接中转节点，然后打开另一个客户端，将会提示端口被占用，因为默认端口均为1080。因此修改本客户端的本地端口为1081或其它值，然后设置该客户端连接服务器节点，代理模式选择全局模式即可。

## 翻墙进阶

### 订阅链接

为防止配置更改导致的失效问题，可通过制作订阅的方式。更新订阅即可更新服务器配置，无需逐个修改。

#### 制作

由于大部分v2ray客户端的分享链接采用v2rayN标准，因此需先在v2rayN上完成各服务器的配置。选中所有需要配置成订阅的服务器，右键选择`批量导出分享URL至剪贴板`，然后打开以下网站，将刚才复制的内容编码成BASE64。

```text
https://tool.oschina.net/encrypt?type=3
```

编码完成后回到Github并新建文件，文件名任意，后缀为txt。将编码后的内容复制到文本区域，保存后点击`Commit changes`，成功后点击`Raw`，复制得到的网址，即为所得订阅链接。本链接可用于大部分客户端。

对于Quantumult，由于其使用自有标准，因此无法使用以上链接。可通过以下网站生成适用于Quantumult的订阅，其中`订阅链接`填写从v2rayN得到的URL，远程配置选择`No-Urltest`，并勾选`输出为Node List`，得到链接。可直接用该链接作为订阅URL，也可在浏览器打开该链接下载文件得到文件内容。

```text
https://sud.bianhuabai.xyz/
```

也可手动完成本操作。在已经配置好服务器的Quantumult上获取服务器的分享链接，删除`vmess://`前缀后放到上述BASE64网站进行解码，解码内容应如下所示。

```text
[名称] = vmess, [地址], [端口], [加密方式], [UUID], [选项]
```

对于vmess节点，在后面加上`, group=[名称]`，例子如下所示。注意，对于vmess节点，每一个订阅需要有唯一的group名称，否则在订阅更新时会被覆盖。若为ss节点，则无需进行操作。

```text
example = vmess, 1.2.3.4, 1234, aes-128-cfb, "12345678-8765-4321-1234-123456789abc", over-tls=false, certificate=1, group=TCP
```

重新编码为BASE64并添加`vmess://`前缀，得到一个可用的链接。其余服务器同理，从而得到一系列可用的链接。将这些链接进行一次BASE64编码即可。如果手动编辑后的连接不可用，则尝试在后面加一个空格，再编码为BASE64并添加`vmess://`前缀。

注意，通过上述一键生成配置网站所生成的文件已经经过处理，但每一个订阅都使用同一个group名称，从而会导致服务器的覆盖。因此需要手动解码，解码一次后得到服务器列表，去掉`vmess://`前缀后解码得到服务器信息，更改group后再次编码即可。

#### 发布

**放到Github上**

登录Github并新建一个Repository，类型为Public，需勾选`Initialize this repository with a README`，完成创建。删除README以减少被搜索出来的几率。

在仓库内新建一个文本文档并编辑，将上面制作好的订阅内容复制进去后保存。然后点击`Raw`，跳转到的链接即为订阅地址。

**放到自有服务器上**

用SSH连接到服务器，新建一个php文档，内容如下。

```text
<?php
$str = file_get_contents("./res.txt", "r") or die("Unable to open file!"); echo base64_encode($str);
?>
```

新建名称为`res.txt`的文本文件，将上面制作好的订阅内容复制进去即可。php文档的地址即为订阅地址。

#### 使用

如果配置无法更新，则需先连接到翻墙环境。

**Quantumult**

点击`设置`-`订阅`，选择右上角的`+`，添加服务器配置，填写订阅URL即可。

**Shadowrocket**

点击右上角`+`号，类型选择Subscribe即可。

**Kitsunebi**

点击服务器，选择右上角的`+`，点击`订阅`，填写订阅URL即可。

#### 格式

以下为经base64解码后的格式。

**Shadowsocks**

```text
ss://method:password@server:port
```

**ShadowsocksR**

```text
ssr://ip:port:protocol:method:blending:password/?remarks=othertext
```

其余形式如下。其中obfsparam、protoparam、group、remarks可选。

```text
159.65.1.189:5252:auth_sha1_v4:rc4-md5:http_simple:NTJzc3IubmV0/?obfsparam=&protoparam=&group=d3d3LnNzcnNoYXJlLmNvbQ&remarks=RE1fTm9kZQ
```

**v2ray**

```text
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

### 服务器加速

#### BBR Plus

在服务器输入以下命令以安装脚本。注意需要先安装BBR Plus内核，安装过程中弹框需选择`NO`。

```text
wget --no-check-certificate -O tcp.sh https://github.com/cx9208/Linux-NetSpeed/raw/master/tcp.sh && chmod +x tcp.sh && ./tcp.sh
```

安装完成后再运行一次脚本，安装BBR Plus服务即可。

```text
./tcp.sh
```

### CDN加速

登录CloudFlare，新建一个worker。用下面代码覆盖原有代码，把url.hostname替换成想要加速的v2ray节点域名（不要添加前面的http和后面的路径）。

```text
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

### Vmware虚拟机使用代理

虚拟机使用宿主机代理，不用设置额外VMware的转发，只需添加代理的地址与端口。手机也能使用虚拟机所配置的本机代理服务器，不需要同宿主机设置专属的VMware转发。

除默认配置的仅主机模式外，宿主机使用VPN会影响全局网络，虚拟机可以直接访问互联网。在NAT模式中，虚拟机通过宿主机器所在的公网网络来访问互联网（目前在墙内），Vmware使用代理软件转发端口监听任意地址，主机在代理中配置同一公网内的局域网IP与端口，完成网络之间的互联共享。在NAT模式中不考虑使用VPN或代理的情况下，IP地址是完全一致的。

虚拟机采用的是非全局性的独立网络，也因此在虚拟机使用VPN并不能使宿主机也能够访问互联网，但能够进行相关配置。在Vmware中开启翻墙软件，安装Privoxy并配置相关参数为`0.0.0.0:8118`，将所有HTTP流量再转发至本机代理，然后在系统设置的代理中设置以上参数。打开命令行并通过`ipconfig`查看虚拟机的IP地址。在Vmware软件中设置端口映射，然后在主机的系统设置的代理中填入Vmware的IP地址和端口即可。

## 服务器进阶

### SSH连接

SSH可用于连接到服务器。

#### 连接方法

**浏览器连接**

GCP默认方式。点击服务器的`SSH`按钮即自动建立与服务器的SSH连接。

**命令行连接**

在终端输入以下命令即可。

```text
ssh -p 22 root@localhost
```

**手机APP连接**

推荐使用HyperApp。

#### 文件传输

可通过scp命令向连接到的服务器传输文件，命令格式如下。

```text
// 上传文件到服务器
scp [本地文件路径] remote_username@remote_ip:[服务器文件路径]
// 上传文件夹到服务器
scp -r [本地文件夹路径] remote_username@remote_ip:[服务器文件夹路径]
```

下载文件或文件夹，则将后面参数对调即可。

#### 服务器配置

一般服务器会提供SSH连接的入口，包括地址、密码等，端口默认为22。但有部分服务器需要手动配置。

**Google Cloud Platform**

通过浏览器进入服务器后台，输入以下命令以切换到root并编辑SSH配置文件。

```text
sudo -i
vi /etc/ssh/sshd_config
```

在配置文件中修改PermitRootLogin和PasswordAuthentication为yes，如下。

```text
# Authentication:
PermitRootLogin yes //默认为no，需要开启root用户访问改为yes

# Change to no to disable tunnelled clear text passwords
PasswordAuthentication yes //默认为no，改为yes开启密码登陆
```

保存后输入以下命令以给root用户设置密码。

```text
passwd root
```

设置完成后输入以下命令重启SSH服务即可。

```text
/etc/init.d/ssh restart
```

### 运行代码

#### 配置服务器运行环境

以运行Python代码为例。连接到服务器并取得root权限，运行以下命令以安装Python 3.7和pip。

```text
apt-get install python3.7
apt-get install pip
```

通过pip安装代码运行所需要的库，如下。

```text
pip install [库名]
```

进入到用户目录并新建文件夹，作为代码存放的位置。

```text
cd /home/[用户名]
mkdir [文件夹名]
```

#### 配置本地运行环境

打开Pycharm Professional，新建工程后选择File-Settings，进入project interpreter，点击add，选择SSH interpreter。输入服务器信息后进行连接，连接完成后配置编译器路径为`/usr/bin/python3`。

保存后选择Tool-Deployment-Configuration，点击刚才新建的服务器，在Mappings标签页选择路径为在服务器端新建好的文件夹。保存后选择Upload即可。

#### 运行代码

**通过Pycharm**

运行前选择服务器端的编译器，运行即可。

**通过服务器自身**

在需要运行的Python代码最前面加入以下代码。

```text
#!/usr/bin/python3
```

进入服务器命令行，切换到代码目录后通过以下命令使代码后台运行并将输出存到文件。此时可以关闭该SSH连接。

```text
nohup python3 -u [test.py] > [test.log] 2>&1 &
```

需要结束运行时，执行以下代码即可。

```text
ps -aux | grep python3
kill [PID]
```

### 网盘文件下载

#### Google Drive

**通过wget**

从Google Drive分享文件后获取其链接，示例如下。其中`1dt4SEYtvK_7LcFNaxKsldGCDd1SMbBlAFILEID`即为`FILEID`。

```text
https://drive.google.com/file/d/1dt4SEYtvK_7LcFNaxKsldGCDd1SMbBlA/view?usp=sharing
```

对于小文件采用以下命令，其中`FILENAME`自己命名即可。

```text
wget --no-check-certificate ‘https://docs.google.com/uc?export=download&id=FILEID’ -O FILENAME
```

对于大文件采用以下命令。

```text
wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=FILEID' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=FILEID" -O FILENAME && rm -rf /tmp/cookies.txt
```

**通过gdown脚本**

下载以下脚本。

```text
https://raw.githubusercontent.com/circulosmeos/gdown.pl/with-resume/gdown.pl
```

在终端切换到脚本所在目录，运行以下命令即可。

```text
./gdown.pl [文件链接] [重命名的文件名]
```

**通过gdown**

在终端输入以下命令即可。

```text
pip install gdown
gdown https://drive.google.com/uc?id=[文件ID]
```

### 创建图形界面系统

#### 在Google Cloud Shell中

进入GCP并点击右上方的终端按钮，进入Cloud Shell。输入以下命令以部署并查看状态。

```text
sudo -i
docker run -itd -p 6080:6080 -e PASSWORD=1234 chenjr0719/ubuntu-unity-novnc
docker ps
```

点击Cloud Shell右上角的预览，更改端口为6080，更改并预览即可。

#### 在VM实例中

新建VM实例并通过SSH连接，然后输入以下命令。

```text
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

```text
https://remotedesktop.google.com/headless
```

复制含DISPLAY的代码到VM实例中并运行，然后打开以下链接以访问远程桌面。

```text
https://remotedesktop.google.com/access/
```

### 安装matlab

通过以上过程安装图形界面，然后在图形界面中进入官网下载安装包即可。

安装完成后，通过SSH连接服务器并运行`matlab`即可调用matlab。

### 端口限速

教程如下。

```text
https://51.ruyo.net/2877.html
```

### 挂机赚钱

#### Vagex

教程如下。

```text
https://51.ruyo.net/3290.html
```

#### eBesucher

```text
https://www.ebesucher.com/
```

教程如下。

```text
https://51.ruyo.net/5383.html
```

#### Google Adsense

```text
https://www.google.com.tw/intl/zh-CN_cn/adsense/start/?utm_campaign=redirect-301
```

教程如下。

```text
https://51.ruyo.net/12240.html
```

## 常见问题

### 提示curl: command not found

服务器未安装curl导致。通过以下命令安装。

```text
// ubuntu/debian系统
apt-get update -y && apt-get install curl -y

// centOS系统
yum update -y && yum install curl -y
```

## 附录

### 通过信用卡验证

使用VPS服务需要VISA或Mastercard的卡，不同VPS的验证标准不同。

#### 免费虚拟卡

**香港全球付预付卡（MasterCard）**

可通过GCP（2018年）验证。

```text
https://www.globalcash.hk/v4/
```

**香港拍住赏预付卡（MasterCard）**

未验证。

需要香港号码。在iOS下载`拍住赏`即可。

**Yandex Money（MasterCard）**

未验证。

```text
https://money.yandex.ru/
https://51.ruyo.net/3338.html
```

**Uquid（VISA）**

未验证。

```text
https://uquid.com/uquid-card
https://51.ruyo.net/4976.html
```

#### 付费虚拟卡

**EastPay**

```text
https://card.easypayx.com/card/cards
```

#### 实体卡

**香港neat借记卡（MasterCard）**

```text
https://www.vpsdawanjia.com/1817.html
```

**中国银行长城跨境通单标储蓄卡（Visa/MasterCard）**

可通过GCP（2019年）验证。

**爱汇国际旅支卡（MasterCard）**

```text
https://www.vpsdawanjia.com/566.html
```

### 获取美国号码

获取美国号码后可以很方便地通过VPS的验证。

#### 虚拟号码

免费号码可使用TextNow。

#### Google Voice

TextPlus、HeyWire、Ring4、TalkU、TextMe、TextFree、TextNow、Talkatone、Dingtone、Pinger的虚拟号码已无法通过Google Voice验证。最新方法如下。

```text
https://github.com/masonme/GoogleVoice
```

保号教程如下。

```text
https://www.vpsdawanjia.com/1452.html
https://51.ruyo.net/3660.html
```

### 机场测速

```text
https://www.duyaoss.com/
```

### IP检查

#### IP连通性

可通过以下网站测试IP的连通性。

```text
https://www.vps234.com/ipchecker/
http://ping.pe/
https://torch.njs.app/
https://www.ipaddress.com/
https://www.ip-adress.com/
https://whoer.net/zh
https://tuna.moe/help/dns/
```

如果IP全通，但翻墙连不上，则一般为端口被封，更换端口即可。

#### 原生IP和广播IP

原生IP指由当地ISP运营商提供的本地IP。而广播IP是IP分配机构指派在某个地区使用的IP。

可通过以下网站查看IP地址的归属地。归属地与服务器所在地一致的为原生IP。

```text
https://bgp.he.net/
```

### 获取翻墙的方式

#### 利用搜索引擎

使用github/qwant/telegram搜索相关翻墙软件的关键词。

顺带使用similarsitesearch查询相似站点。

#### 通过发送Email

有些提供爱国软件的公司/组织会开设一个邮箱，用于自动回复科学上网工具以及页面镜像地址，如发送标题为`help`给`get@psiphon3.com`。

#### 通过P2P下载

连上某个eMule服务器后即可搜索Tor等相关工具。

```text
https://www.emule-project.net/home/perl/general.cgi?l=42
```

### 解决DNS污染

打开以下链接并下载源码。

```text
https://github.com/URenko/Accesser
```

解压后打开以下链接下载dnscrypt-proxy，放到解压好的文件夹的dnscrypt目录下。

```text
https://github.com/jedisct1/dnscrypt-proxy/releases
```

打开终端并运行以下命令。

```text
brew install openssl
pip3 install tld dnspython tornado
env ARCHFLAGS="-arch x86_64" LDFLAGS="-L/usr/local/opt/openssl/lib" CFLAGS="-I/usr/local/opt/openssl/include" pip install cryptography
pip3 install pyopenssl
```

对于Mac，打开网络偏好设置，选择当前连接的网络并点击`高级`，选择代理-自动代理配置，填写以下URL后保存。

```text
http://127.0.0.1:7654/pac/
```

终端切换到源码目录并运行以下命令，在弹出的窗口点击导入证书，下载后双击安装。在弹出的`钥匙串访问`对话框中国双击刚才安装的证书，在`信任`下选择始终信任。

保持网页打开，即可访问受到DNS污染的网站。

### Tor

```text

```

Tor是经过层层加密的浏览器。Tor从使用者本机直到出口节点的传输是强加密的，其他人无法偷窥到真实网络流量，除非Tor软件本身出现严重安全漏洞或者碰到的出口节点是蜜罐，即故意引诱用户攻击的目标。但若此时ISP和墙在监控流量，可以判断出在使用Tor。

流量混淆把Tor流量伪装成其它的上网流量，让监控者看不出在用Tor。meek是常用的流量混淆插件。

Tor主要用于隐匿上网身份，也可用于伪装别国网民、隐匿公网IP等。

#### 配置

打开以下链接下载并安装。Mac也可直接通过Homebrew安装，但通过此方法安装的Tor并没有图形界面。

```text
https://www.torproject.org/download/
```

也可通过发送标题为`help`的邮件到`gettor@torproject.org`获取。

安装完成后挂翻墙软件，然后从配置中查看代理类型、代理地址和端口，比如SOCKS5代理的`localhost:1081`。打开后点击配置，勾选`我所在的国家对Tor进行了审查`，并选择`选择内置网桥`。勾选`使用代理访问互联网`，填入上面查询的内容，然后连接即可。

#### 使用

Tor可以打开特有的onion域名链接，导航如下。注意抵制诱惑。

```text
https://thehiddenwiki.org/
```

#### 其余类似浏览器

```text
https://www.puffin.com/
https://softstribe.com/app/pc/download-install-net-upx-proxy-browser-windows-mac-os
https://getcocoon.com/support/download
https://www.webfreer.com/how.php
https://proxybrowser.xyz/
```

### ZeroNet

零网，托管了很多热门网站。

```text
https://zeronet.io/
https://hoochanlon.github.io/fq-book/#/browse/zeronet
https://github.com/hoochanlon/w3-goto-world/blob/master/%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91%E3%80%81%E6%9A%97%E7%BD%91%E3%80%81%E9%9B%B6%E7%BD%91/%E9%9B%B6%E7%BD%91%E4%B8%8E%E6%9A%97%E7%BD%91/reademe.md
```

## 存档

### 翻墙协议

#### go-shadowsocks2

```text
https://github.com/shadowsocks/go-shadowsocks2
```

#### shadowsocks-libev自动安装脚本

```text
https://github.com/lrinQVQ/script
https://github.com/shadowsocks/shadowsocks-libev
```

### 工具

#### ProxySU

```text
https://github.com/proxysu/windows
```

#### V2ray.Fun

基于Web的V2ray控制面板，需要使用纯净的VPS系统安装。

```text
wget -N --no-check-certificate https://raw.githubusercontent.com/FunctionClub/V2ray.Fun/master/install.sh && bash install.sh
```

#### Finalspeed

```text
https://github.com/ucoker/finalspeed
```

#### zmirror-onekey

部署Google镜像。

```text
https://github.com/aploium/zmirror-onekey
```

#### BarbaTunnel

点对点非独立的隧道。连接OpenVPN的操作并不成功。

```text
https://github.com/BarbaTunnelCoder/BarbaTunnel/
```

#### Cloak

Cloak不是独立的代理程序。而是通过将代理工具的流量伪装为普通的Web浏览流量来工作。

```text
https://github.com/cbeuw/Cloak
```

#### privoxy

将sock协议转换为http/https协议。

```text
https://www.privoxy.org/
```

### 插件

#### openwrt-fanqiang

OpenWrt的Shadowsocks插件。

```text
https://github.com/softwaredownload/openwrt-fanqiang
https://fanqiang.software-download.name/
```

#### v2ray-plugin

基于v2ray的SIP003插件。

```text
https://github.com/shadowsocks/v2ray-plugin
```

#### GoQuiet

shadowsocks的混淆插件。

```text
https://github.com/cbeuw/GoQuiet/wiki/GoQuiet
```

#### simple-tls

用于Shadowsocks（或其他TCP）连接的简单TLS包装器。

```text
https://github.com/IrineSistiana/simple-tls
```

#### obfuscated-openssh-patches

向OpenSSH添加握手混淆的补丁。

```text
https://github.com/zinglau/obfuscated-openssh-patches
```

### 脚本

#### 性能测试

输入以下命令即可。

```text
wget -qO- 86.re/bench.sh | bash
wget -qO- --no-check-certificate https://raw.githubusercontent.com/oooldking/script/master/superbench.sh | bash
wget https://raw.githubusercontent.com/oooldking/script/master/superspeed.sh && chmod +x superspeed.sh && ./superspeed.sh
```

#### v2ray配置脚本

复制以下代码到文本编辑器并另存为go.sh，放在工作目录即可使用。

```text
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

## 参考教程

### trojan-gfw/trojan

```text
https://github.com/trojan-gfw/trojan
```

### Shadowsocks官网

```text
https://shadowsocks.org/en/index.html
```

### v2ray官网

```text
https://www.v2ray.com/
https://github.com/v2fly/v2ray-core
https://www.v2ray.com/chapter_02/protocols/socks.html
```

### Proxifier使用教程

```text
https://blog.csdn.net/u013066730/article/details/88788191
```

### V2Ray+TLS+DNSSEC+WS+Nginx+BBRPlus 搭建教程【2020年最新】

```text
https://yuchuanfeng.github.io/posts/v2ray+tls+ws+nginx/
```

### 德国Euserv免费VPS申请+开通V2RAY+安装宝塔图文教程

```text
https://www.shopee6.com/web/web-tutorial/euserv-order-v2ray-bt.html
https://lala.im/6781.html
https://1rmb.net/t/31.html
```

### 使用宝塔快速安装WordPress教程

```text
https://www.wpmee.com/5976.html
```

### Okteto上部署v2ray

```text
https://704sjf.coding-pages.com/post/hello-gridea/
```

### 自制一个真正属于自己的v2ray订阅地址（自己动手创建v2ray订阅地址教程）

```text
https://aishangyou.tube/?p=6885
```

### Goorm Shell Auto Deploy

```text
https://github.com/guleonseon/goorm-auto
```

### okteto容器破解搭建宝塔shell教程

```text
https://fbk.ink/946.html
https://www.hostloc.com/forum.php?mod=viewthread&tid=667527
```

### 在Compute Engine上设置适用于Linux的Chrome远程桌面

```text
https://cloud.google.com/solutions/chrome-desktop-remote-on-compute-engine?hl=zh-cn#installing_chrome_remote_desktop_on_the_vm_instance
```

### 10分钟快速掌握Docker必备基础知识

```text
https://juejin.im/post/6844903918372143112
https://juejin.im/post/6844903938030845966
```

### wget/curl large file from google drive

```text
https://stackoverflow.com/questions/25010369/wget-curl-large-file-from-google-drive/39225039#39225039
```

### 如何卸载阿里云&腾讯云官方的监控软件服务

```text
https://51.ruyo.net/5369.html
```

### oooldking/script

```text
https://github.com/oooldking/script
```

### 教程01：如何通过V2Ray搭配使用洋葱tor浏览器？

```text
https://test.enuotime.com/guide01
```

### 别以为有vpn我找不到你（网警抓人全过程）

```text
http://www.91ri.org/3599.html
```

### 浅谈HTTP劫持、DNS污染的影响及解决办法（仅个人理解）

```text
https://doubibackup.com/6t3mypbm-5.html
```

### 关于目前 GFW\(墙\) 的封锁方式TCP封锁\(阻断\)猜想

```text
https://doubibackup.com/wkcjzpyd-2.html
```

### SS二级（前置）代理设置实现中转加速观看BBC iPlayer

```text
https://www.itengli.com/ss-relay/
```

