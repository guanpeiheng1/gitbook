```
Notion教程：
https://zhuanlan.zhihu.com/p/49263306
https://sspai.com/post/57464
```

# 相关知识

## 云应用部署

通过存放到Github等代码平台的镜像仓库，可以在云服务商部署应用。仓库应当有docker，以供服务器创建容器。

## IaaS/SaaS/BaaS

IaaS指提供系统（可以自己选）或者储存空间之类的硬件，软件要自己手动装。

SaaS只能使用开发好的软件（卖软件本身）。

BaaS一般类似于非关系数据库，但各家不通用，有时还有一些其它东西。

# 资料

## 大厂泄露资料

### Hacking Team

```
http://ht.transparencytoolkit.org/
```

### 网易

```
https://mega.nz/#F!ewNxmbyT!toMSJLz44mpmQOO6W7j3Kg
magnet:?xt=urn:btih:066c588a6fe43ce29a07c96e4063357eb18433ab
```

### 乌云公开漏洞

```
https://github.com/hanc00l/wooyun_public
```

## 3DMax高清贴图

```
链接 / https://share.weiyun.com/5TX2Fvk
密码 / lo8n23
```

## 简历

```
https://www.polebrief.com/edit

链接 / https://share.weiyun.com/5qHQlwJ
密码 / bs5tpa
```

## 网站

### Wifi破解

```
http://www.52bug.cn/hkjs/2427.html
```

### 内容合集

待整理。

```
http://kyon945.ys168.com/
https://github.com/ramitsurana/awesome-kubernetes
https://github.com/akullpp/awesome-java
https://github.com/awesome-selfhosted/awesome-selfhosted
https://github.com/ivbeg/awesome-status-pages
https://github.com/bhnddowinf/vuejs-learn
https://github.com/josephmisiti/awesome-machine-learning
```

### 论坛和博客

```
https://program-think.blogspot.com/
https://shanyue.tech/
https://g.widora.cn/
https://www.cnblogs.com/
https://masuit.com/
https://www.freehao123.com/
https://dev.to/
https://www.mf8.biz/
https://www.acexy.cn/
https://masuit.com/
https://salsa.debian.org/public
https://www.nutgeek.com/
https://pdf-lib.org/
https://blog.yazawaniko.com/
https://coderschool.cn/
https://www.techspot.com/
https://www.itengli.com/
https://www.chengxuzhilu.com/
```

# 学生专属项目

## Office 365 E5

### 教育版

```
https://www.microsoft.com/en-us/education/products/office?tab=students?tab=students
https://xyxywan.gitee.io/web/
https://od.obagg.com/
https://get.porta1.net/?__cf_chl_jschl_tk__=83c928ebbf99663237ea88805bc924e141699fc3-1605690660-0-AdnezinmvFMzpyB50OfoJCxwTtvM6LK4QCGvz5H-IApWWBURmgaDE8F0ls64UAmhogL2rFz9I9j43Pubz4pIwnxSBYE4x3LUVtmGSmxJey29d6JEKzRWYJGMb0aQKL9j8w0DROKY369MQ0x-ocjbi7gEo68wkc_SyIUuGf-1-I6fFbaWhNCIUod9O_08ANMH79Ws4y9fYn2_f9Xa6AocqNf_vFJYh54JbbrlrE6IhEcLLG0xcBJHzCiNutc417DtiVBbF-Nc93a966QfzJA0LccQIqVLez6kr-OFGE0JGeES_Ilbq1jjy52hyjn7sqxvtA#intro
```

### 开发者账号

打开以下链接开通Office开发者账号，国家选择美国，可以所有选项都勾选。然后点击`SET UP E5 SUBSCRIPTION`，按照流程填写即可。

```
https://developer.microsoft.com/en-us/office/profile/
```

### 自动续订

注册完成后打开以下链接，点击`Azure Active Directory`，进入后选择应用注册-新注册，账户类型选择`任何组织目录(任何 Azure AD 目录 – 多租户)中的帐户和个人 Microsoft 帐户(例如，Skype、Xbox)`，URI填写`http://localhost:53682/`，然后点击注册。

```
https://portal.azure.com/
```

注册完成后记录`应用程序(客户端)ID`，然后点击左侧菜单API权限-添加权限-Microsoft Graph-委托的权限，勾选以下权限。

```
Files.Read.All
Files.ReadWrite.All
Sites.Read.All
Sites.ReadWrite.All
User.Read.All
User.ReadWrite.All
Directory.Read.All
Directory.ReadWrite.All
Mail.Read
Mail.ReadWrite
MailboxSettings.Read
MailboxSettings.ReadWrite
```

完成后点击左侧证书和密码-新客户端密码，截止期限随意，添加后记录`值`以备用。

在本机安装rclone，对于Mac可通过brew安装，其余平台可参考以下链接。

```
https://rclone.org/install/
```

然后输入以下命令，在弹出的窗口完成登录授权。

```
rclone authorize "onedrive" "[应用程序(客户端)ID]" "[应用程序密码]"
```

回到终端，复制`"refresh_token"`的内容，不包含双引号。

打开以下项目并fork一份，然后修改1.txt，用刚才的`"refresh_token"`的内容覆盖。

```
https://github.com/wangziyingwen/AutoApiSecret
```

然后点击Settings-Secrets，添加以下Secret。

```
名称 / CONFIG_ID
值 / id=r'[应用程序(客户端)ID]'

名称 / CONFIG_KEY
值 / secret=r'[应用程序密码]'
```

添加后激活该Action即可。

## Semaphore

```
https://docs.semaphoreci.com/account-management/discounts/
```

## Unreal Engine

教育版免费。

```
https://www.unrealengine.com/zh-CN/education
```

## loom

```
https://support.loom.com/hc/en-us/articles/360006579637-Loom-Pro-Free-for-Students-and-Teachers/?ref=codegena.com
```

## Astah UML

```
https://astah.net/products/free-student-license/
```

## Tableau

```
https://www.tableau.com/academic/students
```

## StudentUniverse

面向30岁以下学生的旅游预订网站，提供学生航班折扣。中国大陆仅支持国航。

```
https://www.studentuniverse.com/
```

## Github Education Pack

```
https://education.github.com/
```

## Microsoft Azure

免费12个月试用。

```
https://www.xobk.cn/JSJC/156.html
https://azure.microsoft.com/zh-cn/
https://docs.microsoft.com/zh-cn/azure/education-hub/azure-dev-tools-teaching/program-faq
```

### 充分使用Azure学生订阅免费额度

教程如下。

```
https://www.jianshu.com/p/9bdd35dc2341
```

### 反代Google

教程如下。

```
https://51.ruyo.net/4880.html
```

### 定时任务

教程如下。

```
https://51.ruyo.net/6499.html
```

### 利用SendGrid免费计划发送邮件服务

教程如下。

```
https://51.ruyo.net/15179.html
```

### Imagine激活Azure

以及免费膜法上网教程。

```
https://51.ruyo.net/3141.html
```

## AWS

```
https://aws.amazon.com/cn/education/awseducate/
https://www.awseducate.com/Registration#APP_TYPE
```

## Autodesk

```
https://www.autodesk.com.cn/education/home
```

## G Suite for Education

```
https://edu.google.com/products/gsuite-for-education/
```

## LINGO Educational Research License

```
https://www.lindo.com/index.php?option=com_content&view=article&id=120&Itemid=45
```

## Office in Education

```
https://products.office.com/en-us/student?tab=students
```

## Pantheon

```
https://pantheon.io/edu
```

## Axure

```
https://www.axure.com/edu
```

## Tableau

```
https://www.tableau.com/zh-cn/academic
```

## Minecraft

```
https://education.minecraft.net/get-started
```

## OriginPro Student Version

```
https://www.originlab.com/OriginProLearning.aspx
```

## Notion

```
https://www.notion.so/student
```

## Shapr3D

```
https://www.shapr3d.com/education
```

## Intel® Software Development Tools

```
https://software.intel.com/content/www/us/en/develop/articles/qualify-for-free-software.html
```

## Bootstrap Studio

```
https://bootstrapstudio.io/pages/student-license
```

## MATLAB

```
https://ww2.mathworks.cn/products/matlab/student.html
```

## 1Password

```
https://www.studentappcentre.com/App/1Password
```

## invision

```
https://www.invisionapp.com/education
```

## Basecamp

```
https://basecamp.com/discounts
```

## YNAB

个人理财软件。

```
https://www.youneedabudget.com/college/
```

## Adobe

最多4折。

```
https://www.adobe.com/creativecloud/buy/students.html
```

## Squarespace

五折优惠。

```
https://www.squarespace.com/students/
```

## Azendoo

```
https://blog.azendoo.com/azendoo-for-education-special-offers/
```

## Figma

```
https://www.figma.com/education/
```

## Tower

```
https://www.git-tower.com/students/mac
```

## Lucidchart

流程图绘制。

```
https://www.lucidchart.com/pages/usecase/education
```

## Sketch Education

```
https://www.sketch.com/store/edu/
```

## Asana

学生生活安排。

```
https://asana.com/students?language=en
```

## Dashlane

为期一年的Dashlane高级版免费使用权。

```
https://www.dashlane.com/students
```

# Telegram Bot

## 接管WeChat信息

教程如下。

```
https://51.ruyo.net/8054.html
```

## Telegram Bot网站RSS订阅机器人部署

教程如下。

```
https://51.ruyo.net/13304.html
```

仓库地址如下。

```
https://github.com/iovxw/rssbot
```

# Docker

## 合集

```
https://github.com/mritd/dockerfile
```

## 签到

```
https://github.com/nzw9314/qiandao
```

# DNS

## 普通DNS

| 供应商        | 主DNS          | 备用DNS         | 备注                                           |
| ------------- | -------------- | --------------- | ---------------------------------------------- |
| Google        | 8.8.8.8        | 4.4.4.4         | https://developers.google.com/speed/public-dns |
| OpenDNS       | 208.67.222.222 | 208.67.220.220  | https://www.opendns.com/                       |
| Level3        | 209.244.0.3    | 209.244.0.4     |                                                |
| DNS Advantage | 156.154.70.1   | 156.154.71.1    |                                                |
| Verizon       | 4.2.2.1        | 4.2.2.1         |                                                |
| SmartViper    | 208.76.50.50   | 208.76.51.51    | https://www.markosweb.com/free-dns/            |
| AIXYZ DNS     | 123.206.21.48  | 115.159.146.99  |                                                |
| HelloDNS      | 123.56.46.123  | 182.254.185.148 |                                                |

## 带安全功能的DNS

| 供应商             | 主DNS           | 备用DNS         | 备注 |
| ------------------ | --------------- | --------------- | ---- |
| Norton ConnectSafe | 198.153.192.40  | 198.153.194.40  |      |
|                    | 198.153.192.50  | 198.153.194.50  |      |
|                    | 198.153.192.60  | 198.153.194.60  |      |
| Comodo Secure DNS  | 8.26.56.26      | 8.20.247.20     |      |
| Securly            | 184.169.143.224 | 184.169.161.155 |      |
| ScrubIT            | 67.138.54.100   | 207.225.209.66  |      |

## DNS供应商

### 阿里云公共DNS

```
https://www.alidns.com/setup/
```

### Public DNS

```
https://www.publicdns.xyz/public/puntcat.html
```

### Yandex.DNS

```
https://dns.yandex.com/
```

### Freenom World

```
http://www.freenom.world/zh/index.html
```

### 1.1.1.1

```
https://1.1.1.1/
```

### Quad9

```
https://quad9.net/
```

### OpenNIC

```
https://www.opennic.org/
```

### 京东智联云

```
https://www.jdcloud.com/cn/products/jd-cloud-dns
```

### 华为云

```
https://www.huaweicloud.com/product/dns.html
```

### DNS.COM

```
https://www.dns.com/
```

### dnsever

```
https://www.dnsever.com/
```

### Zoneedit

```
https://www.zoneedit.com/
```

### DNS EXIT

免费动态DNS，托管DNS服务。

```
https://www.dnsexit.com/
```

### GeoScaling DNS2

提供具有独特功能的免费（每月最多100万个DNS请求）托管的DNS服务。

```
https://www.geoscaling.com/
```

### DNS.LA

```
https://www.dns.la/
```

### 八戒DNS

```
https://www.bajiedns.com/
```

### DNS盾

```
https://www.dnsdun.com/
```

### xip.io

```
http://xip.io/
```

### dynv6

```
https://dynv6.com/
```

### SPDYN

```
https://www.spdyn.de/
```

### FreeDNS

```
https://freedns.afraid.org/
```

### DYNDNSS

```
https://dyndnss.net/eng/
```

### ClouDNS

```
https://asia.cloudns.net/
```

### DNSPOD

```
https://www.dnspod.cn/
https://www.dnspod.com/
```

优惠券如下。

```
https://www.dnspod.cn/promo/coupon
https://cloud.tencent.com/act/pro/DNSPodDomainsCarnival?fromSource=gwzcw.1293314.1293314.1293314&cps_key=98e3b13dc4a1659435daf62289161ee5
```

### Duck DNS

```
https://www.duckdns.org/
```

### FreeDNS

```
https://freedns.afraid.org/
```

### LuaDNS

```
https://www.luadns.com/
```

### Hurricane Electric Free DNS Management

```
https://dns.he.net/
```

### Dynu

```
https://www.dynu.com/
```

### no.ip

```
https://www.noip.com/
```

### ZoneWatcher

```
https://zonewatcher.com/
```

### Zoneedit

```
https://www.zoneedit.com/free-dns/
```

### Lilore

```
https://zilore.com/en/dns
```

### GratisDNS

```
https://web.gratisdns.dk/domaener/dns/
```

### Selectel

```
https://old.selectel.ru/en/services/additional/dns/
```

### PointDNS

```
https://pointhq.com/developer
```

### NS1

```
https://ns1.com/
```

### DomainTools

```
https://www.domaintools.com/
```

## DNS服务程序

### Overture

```
https://github.com/shawn1m/overture
```

### glider

```
https://github.com/nadoo/glider
```

### dnsmasq-china-list

```
https://github.com/felixonmars/dnsmasq-china-list
```

### ChinaDNS

```
https://github.com/aa65535/ChinaDNS
```

# SSL证书

## 国内SSL证书

### 又拍云

```
https://www.upyun.com/products/ssl
```

### 腾讯云

```
https://buy.cloud.tencent.com/ssl?fromSource=ssl
```

### 阿里云

```
https://common-buy.aliyun.com/?spm=5176.7968328.231195.3.DAY7Db&commodityCode=cas#/buy
https://developer.aliyun.com/adc/student/?source=5176.11533457&userCode=qhrfl7xv
```

### 沃通

```
https://buy.wosign.com/free/#ssl
```

### allinssl

```
https://allinssl.com/zh/
```

## 国外SSL证书

### freeSSL

```
https://freessl.cn/
https://freessl.org/
```

### Let's Encrypt

```
https://certbot.eff.org/
https://github.com/certbot/certbot
```

教程如下。

```
https://51.ruyo.net/8077.html
https://51.ruyo.net/3163.html
```

### KeyChest

```
https://keychest.net/
```

### GlobalSign

```
https://www.globalsign.com/en/managed-ssl/ssl-open-source
```

### ZeroSSL

```
https://zerossl.com/
```

# CDN

## 国内免费CDN

### 360网站卫士CDN前端公共库

```
http://libs.useso.com/
```

### 七牛静态资源CDN服务

```
https://www.staticfile.org/
```

### 又拍云常用JS库CDN服务

```
http://jscdn.upai.com/
```

### 极客族公共加速服务

```
https://cdn.geekzu.org/cached.html
```

### 奇虎360前端静态资源库

```
https://cdn.baomitu.com/
```

### 百度静态资源公共库

```
http://cdn.code.baidu.com/
```

### 新浪云计算公共库

```
http://lib.sinaapp.com/
```

### CDNBee

```
https://cdnbee.com/
```

### BootCDN

```
http://www.bootcdn.cn/
```

### 七牛云

```
https://developer.qiniu.com/af/kb/1574/free-credit-information
```

### Fikker

自建CDN。

```
https://www.fikker.com/
```

安装教程如下。

```
步骤一：下载/解压
Fikker 为绿色安装包，推荐直接下载/解压在 Windows 桌面上。
这样子做的好处是，其它空白分区(例如D/E盘等)都可以设置成硬盘缓存目录了。

步骤二：关闭 IIS 服务和 Windows 防火墙
a、关闭 IIS 服务，避免与 Fikker 的 80/443 端口冲突。★★★
b、关闭 Windows 防火墙，避免与 Fikker 的 80/443/6780 端口冲突。★★★

步骤三：启动 Fikker 服务
a、分别运行【注册服务】 和 【运行服务】 即可。<截图示例>
b、登录 Fikker 管理后台：http://your-fikker-ip:6780/，管理员的初始密码：123456。

步骤四：关于 Fikker 配置
a、修改管理员/监控员的初始密码。
b、通过 Fikker 管理后台配置 【系统配置】，限制 Fikker 可用内存。
c、通过 Fikker 管理后台配置 【主机管理】，添加网站域名与对应的源站IP。

系统优化：
在安装目录 tools 下面，提供有优化 Windows 网络性能的脚本，放大系统 TCP 端口数/提升 TCP 连接并发数。注：重启机器生效。

其它说明：
a、注销服务程序，在安装目录下面，运行【注销服务】即可。
b、重启服务程序，在安装目录下面，分别运行【停止服务】和【运行服务】即可。
```

### FreeCDN

```
http://su.zhiduopc.com/
```

### 加速乐

```
https://defense.yunaq.com/jsl/
```

### YUNDUN

```
https://www.yundun.com/
```

### 字节跳动静态资源公共库

```
http://cdn.bytedance.com/
```

### JSHub

```
https://jshub.com/
```

### 腾讯网静态资源公共库

```
https://libs.qq.com/
```

### BootCDN

```
https://www.bootcdn.cn/
```

### JSHub

```
https://jshub.com/
```

### 又拍云JS库

```
http://jscdn.upai.com/
```

### Staticfile CDN

```
https://staticfile.org/
```

### loli

```
https://cdnjs.loli.net/
```

### UCLOUD GlobalSSH

需要实名。

```
https://docs.ucloud.cn/pathx/globalssh
```

### unpkg CDN

```
https://www.v2ex.com/t/521411
```

### 百度云加速

```
https://su.baidu.com/
```

### 知道创宇云安全

```
https://defense.yunaq.com/
```

### CDNFINE

```
https://www.cdnfine.com/
```

### 美尔联

```
http://cdn.meierlian.net/
```

### 又拍云

常用JavaScript库CDN服务。

```
http://jscdn.upai.com/
```

### 前端静态资源库CDN

首个支持HTTP/2的CDN服务。

```
https://cdn.baomitu.com/
```

## 国外免费CDN

### Microsoft ASP.net CDN

```
http://www.asp.net/ajaxlibrary/cdn.ashx
```

### j-Query

```
https://jquery.com/
http://code.jquery.com/
```

### Google Hosted Libraries

```
https://developers.google.com/speed/libraries/#d3.js
```

### Fast.io

用于图像，视频和文档的CDN。

```
https://fast.io/
```

### jare

```
http://www.jare.io/
```

### FreeDNS

```
https://www.1984.is/product/freedns/
```

### cdnjs

```
https://cdnjs.com/
http://cdnjs.net/
```

### Microsoft Ajax内容分发网络

```
https://docs.microsoft.com/zh-cn/aspnet/ajax/cdn/overview
```

### Google Hosted Libraries

```
https://developers.google.com/speed/libraries/
```

### bootstrapCDN

```
https://www.bootstrapcdn.com/
```

### JSdelivr

```
https://www.jsdelivr.com/
```

搭建为脚本/图片等静态文件加速的全球CDN的教程如下。

```
https://51.ruyo.net/15149.html
```

### embed.ly

```
https://embed.ly/
```

### image 4 io

```
https://image4.io/
```

### Sirv

图像CDN。

```
https://sirv.com/
```

### Travis CI

```
https://travis-ci.org/
```

### tiiny.host

```
https://tiiny.host/
```

### Z.A.R.V.I.S.

```
https://zarvis.ai/
```

### CODING

```
https://coding.net/
```

### gitee

```
https://gitee.com/
```

### NotABug

```
https://notabug.org/
```

### Perforce

```
https://www.perforce.com/products/helix-teamhub
```

### OpenJS Foundation

```
https://openjsf.org/
```

### deployhq

直接从存储库构建和部署代码，管理网站的持续部署。免费试用10天。

```
https://www.deployhq.com/
```

### HOUND

```
https://houndci.com/
```

### COVERALLS

```
https://coveralls.io/
```

### CodeFactor

Git的自动代码审查。

```
https://www.codefactor.io/
```

### codecov

改善代码审查工作流程和质量。

```
https://codecov.io/
```

### Deepcode

实时语义代码分析。

```
https://www.deepcode.ai/
```

### CodeScene

```
https://codescene.io/
```

### CODE CLIMATE

```
https://codeclimate.com/
```

### codeac

将基础架构分析为代码。

```
https://www.codeac.io/infrastructure-as-code.html?ref=free-for-dev
```

### codacy

自动检查提交和拉取请求的代码。

```
https://www.codacy.com/
```

### cacher

```
https://www.cacher.io/
```

### Dependabot

```
https://dependabot.com/
```

### CodeNotary

```
https://codenotary.io/
```

### FoxPass

实现服务器和网络访问的自动化。

```
https://www.foxpass.com/
```

### 百度效率云

```
https://xiaolvyun.baidu.com/
```

### Opendev

```
https://opendev.org/
```

### para

```
https://paraio.com/
```

# 云

## 国内免费云

### 盛大云

```
http://www.grandcloud.cn/
```

### 小米云服务

```
https://i.mi.com/
```

### 看云

```
https://www.kancloud.cn/
```

### 亿方云

```
https://www.fangcloud.com/
```

### 优刻得

```
https://www.ucloud.cn/site/product/ufile.html
```

部分教程如下。

```
https://51.ruyo.net/14676.html
https://51.ruyo.net/8383.html
```

### 酷播云

免费版空间5G/年，流量12G/月。

```
http://www.cuplayer.com/price/
```

### 灵雀云

可申请免费试用。

```
https://www.alauda.cn/
```

### DaoCloud

```
https://www.daocloud.io/
```

搭建SSR教程如下，只能用于国外翻回国内。

```
https://51.ruyo.net/3720.html
```

### Bmob

```
https://www.bmob.cn/
```

### 又拍云

```
https://www.upyun.com/
```

### 思爱普SAP

```
https://www.sap.cn/products/cloud-platform/pricing.html
```

### 时速云

```
https://www.tenxcloud.com/
```

### 魔泊云

```
https://www.mopaas.com/
```

### 伙伴云

```
https://www.huoban.com/
```

### 简道云

```
https://www.jiandaoyun.com/
```

### 氚云

```
https://h3yun.com/
```

### 明道云

```
https://www.mingdao.com/
```

## 国外免费云

### OpenStack

```
https://www.openstack.org/
```

### Rancher

```
https://rancher.com/
```

### cloudAMQP

```
https://www.cloudamqp.com/
```

### Cloudinary

```
https://cloudinary.com/pricing
```

### Firebase

```
https://firebase.google.cn/pricing
```

教程如下。

```
https://zhuanlan.zhihu.com/p/95334890
https://www.zhihu.com/question/34124789/answer/72495188
```

### Virtual VM

暂时缺货。

```
http://www.virtualvm.com/#!/Home
```

### Hikiku

暂时缺货。

```
https://hikiku.fr/cart.php?gid=1
```

### Ikoula

新用户送100美金。

```
https://www.ikoula.cn/zh/cloudikoulaone
```

### Digital Ocean

新用户送100美金。

```
https://try.digitalocean.com/virtual-private-servers/?refcode=1ac212f0057b
```

### 阿里云

免费12个月试用。

```
https://www.alibabacloud.com/zh/campaign/free-trial?utm_content=se_1005982105&gclid=Cj0KCQjwpNr4BRDYARIsAADIx9wDyfkU0SVGIme51lYoe5OAck_D7ARwEC5M3extalTLq4xaRg2hocYaAsVoEALw_wcB
```

### Amazon Web Services

免费12个月试用，750个小时。

```
https://aws.amazon.com/cn/about-aws/select-regions/?sc_channel=PS&sc_campaign=acquisition_CN&sc_publisher=baidu&sc_medium=bz&sc_content=pc&sc_detail=HL&sc_category=pc&sc_segment=test&sc_country=CN&trkCampaign=request_for_pilot_account&trk=baidu-ppc-test
```

### Ikoula

免费领取100欧元。

```
https://www.xobk.cn/159.html?btwaf=60719348
```

### Atlantic

免费12个月试用。

```
https://www.atlantic.net/
```

### Evolution Host

需要有个人网站才可以申请。

```
https://evolution-host.com/free-vps.php
```

### FossHost

```
https://fosshost.org/
```

### OpenShift

```
https://github.com/bclswl0827/v2ray-openshift
```

### redislabs

```
https://redislabs.com/try-free/
```

### Staroid

```
https://staroid.com/
```

### platform9

```
https://platform9.com/
```

### BackBlaze

```
https://www.backblaze.com/b2/cloud-storage.html
```

### invantive cloud

需要翻墙。

```
https://cloud.invantive.com/language/en
```

### Kubernautic

```
https://kubernauts.sh/
```

### Clever Cloud

前20欧元免费。

```
https://www.clever-cloud.com/en/pricing
```

### cloudcannon

```
https://cloudcannon.com/
```

### render

```
https://render.com/
```

### BackBlaze

```
https://www.backblaze.com/b2/cloud-storage.html
```

### Appharbor

支持.NET。

```
https://appharbor.com
```

### FreeASPHosting

支持.NET。

```
https://freeasphosting.net/
```

### Dev Graph

需要有公司邮箱。

```
https://www.engineyard.com/
```

### LeanCloud

```
https://leancloud.app/
```

### AWS Cloud9

用于编写、运行和调试代码的云IDE。

```
https://aws.amazon.com/cn/cloud9/
```

### M3O

```
https://m3o.com/about
```

### Terraform

```
https://www.terraform.io/
```

### UCloud

```
https://www.ucloud.cn/
```

### 听云

可免费试用。

```
https://www.tingyun.com/
```

### Scalingo

```
https://scalingo.com/
```

### skyvia

```
https://skyvia.com/
```

### Okta Developer Platform

```
https://developer.okta.com/
```

### JumpCloud

```
https://jumpcloud.com/
```

### Shippable

```
https://www.shippable.com/pricing.html
```

### GearHost

```
https://www.gearhost.com/pricing
```

### Glitch

```
https://glitch.com/
```

### Krucible

```
https://usekrucible.com/
```

### Terraform

```
https://www.terraform.io/
```

### Scalingo

```
https://scalingo.com/
```

# 通讯与匿名身份

## 通讯工具

### 环信

```
https://www.easemob.com/
```

### 亲加通讯云

```
http://www.gotye.com.cn/
```

## 虚拟号码

### Sonetel

可免费试用。

```
https://sonetel.com/zh-hans/
```

## 短信平台

### 云片

```
http://www.yunpian.com/
```

### SendCloud

```
https://sendcloud.sohu.com/
```

## 消息推送和同步

### ably

```
https://www.ably.io/
```

### Iron

快速通过托管消息队列传递消息。

```
https://www.iron.io/
```

### OneSignal

Web推送，电子邮件和应用内消息。

```
https://onesignal.com/
```

### QuickBlox

提供即时消息API。

```
https://quickblox.com/
```

### simperium

跨平台的数据同步服务。

```
https://simperium.com/
```

### pusher

跨平台的推送通知API。

```
https://pusher.com/beams
```

### VWO

通过网络推送通知和Facebook Messenger使您的访客参与自动营销活动。

```
https://vwo.com/engage/?ref=pc
```

### pushbots

```
https://pushbots.com/
```

# VPS

## 国内免费VPS

### 三丰云

国内VPS，需要通过发帖进行免费续费。

```
https://www.sanfengyun.com/
```

### 新睿云

首月免费。

```
https://activity.xinruiyun.cn/free/
```

### 腾讯云

免费使用15天。

```
https://cloud.tencent.com/act/free
```

### OneNET

可免费试用OneNET应用托管服务的全栈服务，试用时有资源限制。

```
https://open.iot.10086.cn/cloud/introduction/cloud-server
```

## 国外免费VPS

### Oracle Cloud

前一个月免费试用300美元，同时提供永久免费额度，可以创建并运行2个VPS主机，包含足够用的月流量和100GB的存储。

```
https://wzfou.com/oraclecloud/
https://www.oracle.com/cloud/free/
```

部分教程如下。

```
https://51.ruyo.net/16216.html
```

### VPN Jantit

```
https://www.vpnjantit.com/free-softether.html
```

### R-VPN Network

通过代码`TRIAL`可以进行24小时的免费试用。

```
https://w.rvpn.space/
```

### XREA

需要日本手机号。

```
https://www.xrea.com/
```

教程如下。

```
https://51.ruyo.net/5611.html
```

### Serverspace

试用送1卢布体验金，开最低配置的服务器能用3天。

```
https://serverspace.by/services/vps-server/
```

### OVHCloud

免费30欧元。

```
https://www.ovh.com/fr/
```

### Qwiklabs

无限免费体验GCP服务器，单次时长3小时。打开以下链接以注册。

```
https://google.qwiklabs.com/
```

注册完成后打开以下链接开启课程即可。

```
https://google.qwiklabs.com/focuses/3393?catalog_rank=%7B%22rank%22%3A1%2C%22num_filters%22%3A0%2C%22has_search%22%3Atrue%7D&parent=catalog&search_id=7893158
```

### virtualmaster

无限免费4小时的VPS。

```
https://www.virtualmaster.com/en
```

### HyperVMart

```
http://hypervmart.com/
```

免费1个月VPS，优惠码如下。

```
https://51.ruyo.net/4902.html
```

## 主机管理面板

### Porta1.Net CDN

CloudFlare Partner可视化管理面板，Cloudflare CNAME/IP/NS配置。

```
https://github.com/ZE3kr/Cloudflare-CNAME-Setup/blob/master/README.zh.md
https://dns.porta1.net/
```

教程如下。

```
https://51.ruyo.net/5145.html
```

### LAMP

```
http://86.re/index_cn.html
```

### Appnode

Linux服务器集群管理面板。

```
https://www.appnode.com/
```

### Peerflix Server

Node.js流媒体客户端。

```
https://github.com/asapach/peerflix-server
```

教程如下。

```
https://51.ruyo.net/2761.html
```

### 宝塔

```
http://www.bt.cn/
```

### AMH

```
http://amh.sh/
```

### WDCP

```
http://www.wdlinux.cn/bbs/
```

### VestaCP

自带中文。

```
http://vestacp.com/
```

### Kloxo-MR

有中文包。

```
https://github.com/mustafaramadhan/kloxo/
```

### Webmin/Virtualmin

自带中文。

```
http://www.webmin.com/virtualmin.html
```

### Ispconfig

有中文包。

```
https://github.com/dclardy64/ISPConfig-3-Debian-Installer
```

### i-MSCP

自带中文。

```
http://i-mscp.net/
```

### EasySCP

自带中文。

```
http://www.easyscp.net/
```

### Ajenti

轻量级，类似Webmin，自带中文。

```
http://ajenti.org/
```

### zPanel(sentora)

支持Windows，有中文包。

```
http://www.zpanelcp.com/
http://www.sentora.org/
```

### centos webpanel

```
http://centos-webpanel.com/
```

### Froxlor

轻量，适应各种环境。

```
http://www.froxlor.org/
```

### AlternC

```
https://alternc.org/
```

### Easy Hosting Control Panel

有第三方汉化包。

```
http://ehcp.net/
```

### CyberPanel

只支持Centos 7.x。

```
http://docs.cyberpanel.net/doku.php
```

### 宝塔英文版

```
http://www.aapanel.com
```

### runcloud.io

有免费版和付费版。

```
https://runcloud.io
```

### froxlor

没有汉化。

```
https://www.froxlor.org/
```

## 主机管理面板一键安装脚本

### LNMP

```
http://lnmp.org/
```

### Oneinstack

```
https://oneinstack.com/
https://github.com/oneinstack/lnmp
```

### Teddyson

```
http://teddysun.com/lamp
http://teddysun.com/lamp-yum
```

### 阿里云LNAMP

```
https://developer.aliyun.com/ask/163675?spm=a2c6h.13524658
```

### LTMP

支持CentOS/RadHat。

```
http://www.ltmp.cc/
```

### Appnode

免费版不支持面板。

```
https://www.appnode.com/
```

### CENTMIN MOD

```
http://centminmod.com/
```

### VPSSIM

```
https://vpssim.com/
```

### lowendscript

```
https://github.com/Xeoncross/lowendscript
```

### monkeyServer

轻量级的web服务器。

```
https://github.com/alexandreteles/monkeyServer
```

### UPUPW

Windows平台环境搭建。

```
http://www.upupw.net/
```

### PhP study

```
https://www.xp.cn/linux.html#install-show
```

## 网络重装系统一键脚本

### 原系统为Linux

特别注意OpenVZ构架不适用，安装之前备份重要数据。适用于由GRUB引导的CentOS/Ubuntu/Debian系统。

使用官方发行版去掉模板预装的软件，同时也可以解决内核版本与软件不兼容的问题。

全自动安装默认root密码为Vicer。

#### Debian 7 x32

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 7 -v 32
```

#### Debian 7 x64

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 7 -v 64
```

#### Debian 8 x32

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 8 -v 32
```

#### Debian 8 x64

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 8 -v 64
```

#### Debian 9 x32

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 9 -v 32
```

#### Debian 9 x64

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d 9 -v 64
```

#### Ubuntu 14.04 x32

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d trusty -v 32
```

#### Ubuntu 14.04 x64

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d trusty -v 64
```

#### Ubuntu 16.04 x32

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d xenial -v 32
```

#### Ubuntu 16.04 x64

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d xenial -v 64
```

#### Ubuntu 17.04 x32

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d zesty -v 32
```

#### Ubuntu 17.04 x64

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && chmod -x DebianNET.sh && bash DebianNET.sh -d zesty -v 64
```

#### CentOS 6+ X64

root用户密码为`xiaofd.win`。

```
wget xiaofd.github.io/centos.sh && bash centos.sh
```

#### Windows

```
wget --no-check-certificate -qO DebianNET.sh 'https://moeclub.org/attachment/LinuxShell/DebianNET.sh' && bash DebianNET.sh -dd 'https://moeclub.org/get-win7embx86-auto'

```

#### 注意事项

##### 软件依赖

相关软件依赖如下。

```
// Debian/Ubuntu
apt-get update
apt-get install -y gawk sed grep
 
// RedHat/CentOS
yum update
yum install -y gawk sed grep
```

##### 常见问题

在安装Ubuntu时可能会遇到`Getting the time form a network time server...`并界面进度条很长时间不会动，可以等待它超时，或更换别的版本。

#### 代码备份

##### 安装Linux

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

##### 安装Windows

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

### 原系统为Windows

#### Linux

下载以下脚本并运行即可。

```
https://moeclub.org/attachment/WindowsSoftware/win32loader.bat
```

#### 代码备份

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

## 后台控制面板

### AdminLTE

```
https://github.com/ColorlibHQ/AdminLTE
```

### VUE ELEMENT ADMIN

```
https://github.com/PanJiaChen/vue-element-admin
```

### Tabler

```
https://github.com/tabler/tabler
```

### gentelella

```
https://github.com/ColorlibHQ/gentelella
```

### ngx-admin

```
https://github.com/akveo/ngx-admin
```

### Ant Design Pro

```
https://github.com/ant-design/ant-design-pro
```

### BlurAdmin

```
https://github.com/akveo/blur-admin
```

### Vue Bulma

```
https://github.com/vue-bulma/vue-admin
```

### iView Admin

```
https://github.com/iview/iview-admin
```

### Material Dashboard

```
https://github.com/creativetimofficial/material-dashboard
```

# 虚拟主机

虚拟主机是网络服务器上分出的一定的磁盘空间。与VPS不同，虚拟主机一般不能用SSH连接，也不能搭建宝塔。

## 获取

### 合集

```
http://www.100bestfreewebspace.com/searchPage0/Free-Host.html
```

### GearHost

```
https://my.gearhost.com/Account/Login?ReturnUrl=%2fCloudSite
```

### targetbird

```
https://targetbird.com/
```

### FREEHOSTING

```
https://www.freehosting.com/
```

### 强人网络

```
https://www.qiangren.net/html/active/vhost-free/
```

### gigarocket

```
https://www.gigarocket.net/
```

### Glyptodon Enterprise

免费体验15分钟亚马逊Windows虚拟机。

```
https://enterprise.glyptodon.com/
```

### VPSWALA

```
https://vpswala.org/
```

### lensdump

```
https://lensdump.com/
```

### com.nu

```
http://www.com.nu/
```

### NodeHost

```
https://www.nodehost.ca/teachers
```

### Nexus Bytes

```
https://my.nexusbytes.com/cart.php
```

### 热铁盒虚拟主机

```
https://host.retiehe.com/
```

### 奔腾主机

```
https://benteng.host/
```

### 热卖互联

```
https://www.remaile.cn/index.php/buy/index/?
```

### 酷番云

```
https://www.kufanyun.com/freehost
```

### 主机蛋

```
http://www.idcegg.com/
```

### 金泰联

```
http://www.iiddcc.com/
```

### 恒爱网络

```
http://www.zzhidc.com/services/freehost/
```

### 番茄互联公益景安主机

```
https://www.fqidc.cn/free/
```

### HostingFacts

```
https://hostingfacts.com/
```

### X10hosting

```
https://x10hosting.com/
```

### AwardSpace

```
https://www.awardspace.com/free-hosting/
```

### Free Web Hosting Area

```
https://www.freewebhostingarea.com/
```

### GoogieHost

```
https://googiehost.com/freehosting.html
```

### freehostingnoads

```
https://freehostingnoads.net/
```

### biz.nf

```
https://www.biz.nf/
```

### lima-city

```
https://www.lima-city.de/
```

### VIEWEN

```
https://viewen.com/free-cloud-hosting/
```

### wpnode

```
https://wpnode.net/free-hosting/
```

### mofh

```
https://www.myownfreehost.net/
```

### olab

```
http://olab.in/
```

### HostAll

```
http://www.uhostall.com/free-hosting.php
```

### efreehost

```
http://efreehost.com/
```

### BYET

```
https://byet.host/free-hosting
```

### loft

```
https://devspace.cloud/
```

### K8Spin

```
https://k8spin.cloud/oss-projects/
```

### Education Host

Github学生包可获得一年免费虚拟主机。

```
https://github-students.educationhost.co.uk/
```

### Somee

需要翻墙才能使用。

```
https://somee.com/
```

### InfinityFree

无法搭建翻墙。

```
https://infinityfree.net/
```

### 彩虹云

国内虚拟空间。可用性差，无法搭建翻墙。

```
https://www.cccyun.net/
```

### 我的免费云

国内虚拟空间，需要持续产生推荐才可能长期免费试用。

```
https://www.myfreeyun.com/
```

### Sentris

没有库存。

```
https://www.sentris.net/billing/cart.php
```

### IONOS

六个月免费。

```
https://www.ionos.co.uk/hosting/web-hosting
```

### 云上萝莉

```
http://yunloli.com/index.php/buy/index/
```

### Edison

暂时缺货。

```
https://id.ews1.com/cart.php
```

### nexusbytes

```
https://my.nexusbytes.com/cart.php?gid=1
```

### aspify

```
https://order.aspify.com/cz/freehosting/
```

### BYET

```
https://byet.host/free-hosting
```

### Commons Host

```
https://commons.host/
```

### ucraft

```
https://www.ucraft.com/free-web-hosting
```

### 景安

```
https://www.zzidc.com/main/huodong/freehost/spreadid_103307.html
```

## 使用

### 搭建反向代理网站

可通过Glype搭建代理，在网站中输入外网地址即可访问。

打开以下链接下载glype安装包，名称为`glype-master.zip`。

```
https://codeload.github.com/k1995/glype/zip/master
```

进入虚拟主机控制后台，为虚拟主机绑定一个域名，此处假定为`https://test.com`。在文件管理中进入网站根目录，此处为`wwwroot`。上传下载好的压缩包，完成后解压。

进入以下网站并完成账号注册，其中`test.com`为域名。完成后再次进入即可。

```
https://test.com/glype-master
```

可通过以下网站进入管理后台。

```
https://test.com/glype-master/admin.php
```

### 搭建You2PHP

#### 过程

打开以下网站并用Google账号登录。若之前使用过GCP，则已有项目，否则新建一个项目。选择好项目后点击`启用API和服务`，在左侧下拉列表找到Youtube，点击`Youtube Data API`并启用。然后选择`创建凭据`，凭据种类选`YouTube Data API v3`，调用API选`网页服务器`，`要访问`的数据选公开数据，点击按钮并复制API密钥即可。

```
https://console.developers.google.com/
```

打开以下链接下载You2PHP安装包，此处名称设为`you2php`。将安装包放到虚拟主机的网站根目录下并解压。

```
https://github.com/pigmanidea/you2php-000webhost/blob/master/you2php.zip
```

访问以下网站以完成安装即可，其中`test.com`为域名。

```
https://test.com/you2php/install.php
```

#### 对Heroku的说明

You2PHP也可搭建到Heroku上。获取Youtube的API密钥后，按照以下两种方法中的一种即可。

##### 通过Heroku CLI

克隆以下仓库并修改仓库名为`you2php-heroku`。编辑web/config.php文件，修改APIKEY后面的星号为所申请到的API密钥，其余资料也可修改。编辑根目录的composer.json和composer.lock，将php的版本号改为一个较高的值，此处采用`7.1.33`。

```
https://github.com/You2php/-heroku
```

打开终端并运行以下命令即可。其中第一句为安装Heroku CLI，`[github用户名]`需替换为自己的账号。

```
brew tap heroku/brew && brew install heroku
git clone https://github.com/[github用户名]/you2php-heroku.git
cd you2php-heroku
heroku create {You APP Name}
git push heroku master
heroku ps:scale web=1
heroku open
```

Heroku CLI的具体安装方法可点击以下链接。

```
https://devcenter.heroku.com/articles/heroku-cli#download-and-install
```

##### 通过github仓库

克隆以下仓库后，编辑根目录的composer.json和composer.lock，将php的版本号改为一个较高的值。

```
https://github.com/onplus/you2php-heroku/tree/web
```

在浏览器打开Heroku后台，新建一个app后进入设置，点击`Deploy`选项卡并选择连接到github。连接完成后，在下面搜索仓库名，并选择刚才修改好的仓库，点击部署即可。

### 探针

探针可显示服务器情况。以Holy Lance探针为例，项目地址如下。

```
https://github.com/Brant2005/Holy-Lance
```

打开终端并运行以下命令以下载探针。

```
wget https://od.fbk.ink/ufile/php/holy_lance.php
```

进入虚拟主机后台并将文件放到网站根目录，通过以下链接即可访问，其中`test.com`为域名。

```
https://test.com/holy_lance.php
```

### 搭建WordPress

打开以下链接下载最新的WordPress安装包，并上传到虚拟主机网站根目录。将文件夹里的所有内容都复制到根目录下，而不是单独放到wordpress文件夹。

```
https://cn.wordpress.org/latest-zh_CN.zip
```

完成后进入以下网站完成安装即可。

```
https://test.com/
```

# 网盘

## 国内免费网盘

### U-File

```
https://u-file.cn/
```

### 和彩云

```
https://yun.139.com/w/#/
```

### 天翼云盘

```
https://cloud.189.cn/
```

### 沃云

```
http://www.wocloud.com.cn/;
```

### 和彩云

```
https://yun.139.com/w/#/
```

### 曲奇云盘

```
https://www.quqi.com/
```

### ASUS WebStorage

免费5G空间。

```
https://www.asuswebstorage.com/navigate/a/#/index
```

### 蓝奏云

```
https://www.lanzou.com/
```

### 6盘

```
https://2dland.cn/
https://www.liupan.net/
```

### 超星云盘

```
http://pan-yz.chaoxing.com/app/download
```

### 盛天云盘

```
https://pan.bitqiu.com/
```

### 90网盘

有下载收益。

```
https://www.90pan.com/
```

### 小麦云盘

免费5TB，最大单文件200MB。

```
https://b.own-cloud.cn/
```

## 国外免费网盘

### mega.nz

```
https://mega.nz/
```

### MediaFire

```
https://www.mediafire.com/
```

### Dropbox

```
https://www.dropbox.com/
```

### Google云端硬盘

```
https://www.google.com/drive/
```

### pCloud

```
https://www.pcloud.com/zh/
```

### Yandex Disk

```
https://disk.yandex.com/
```

### Google Drive

```
https://gd.zxd.workers.dev/
```

### TeraCLOUD

免费10GB容量。输入邀请码`BLU6Y`可再获得5GB。

```
https://teracloud.jp/en/
```

该网盘可挂载到VPS上，作为额外的磁盘使用。输入以下命令安装davfs2。

```
// Ubuntu/Debian
apt-get install davfs2

// CentOS
yum install davfs2
```

然后输入以下命令，其中网址需要替换为网盘详情页中的WebDAV URL。

```
mkdir /TeraCloud
mount -t davfs https://uno.teracloud.jp/dav/ /TeraCloud
```

输入以下命令以检查是否已经被挂载。

```
df -h
```

本地挂载的教程如下。

```
https://51.ruyo.net/5577.html
```

### DOSDrive

前50GB免费。

```
https://dosdrive.com/
```

## 分布式存储

### Google Cloud Storage

```
https://cloud.google.com/storage/
```

### 网易数帆

```
https://www.163yun.com/product-cloudcompute?h=fc
```

### 阿里云对象存储OSS

```
https://www.aliyun.com/product/oss
```

### 腾讯云对象存储COS

```
https://cloud.tencent.com/product/cos
```

## 网盘管理

### MultCloud

通过一个应用程序传输和管理多个云文件。

```
https://www.multcloud.com/
```

### DockerRegisterCloud

基于Docker仓库协议的网盘客户端，可以将目前众多的免费容器仓库服务用于网盘。

```
https://github.com/xausky/DockerRegisterCloud
```

教程如下。

```
https://51.ruyo.net/16526.html
https://51.ruyo.net/16572.html
```

# 网络处理

## 网络抓取

### prerender.io

```
https://prerender.io/
```

### ScrapingAnt

```
https://scrapingant.com/
```

### Sheetson

从Google表格构建即时API。

```
https://sheetson.com/
```

### scrapinghub

网页抓取服务和工具。

```
https://www.scrapinghub.com/
```

### proxycrawl

```
https://proxycrawl.com/
```

### parsehub

```
https://parsehub.com/
```

### Scraper.AI

```
https://scraper.ai/
```

## 网络加速与优化

### 烧饼托管

```
https://sb.sb/blog/css-cdn/
```

### china-speed

国内镜像加速。

```
https://github.com/china-speed/china-speed.github.io
```

### LiteSpeed

免费为单个站点所有者提供LiteSpeed Web Server的功能。

```
https://www.litespeedtech.com/experience-litespeed-for-free
```

### Statically

用于优化Web项目的工具。

```
https://statically.io/
```

### RAPICOM seed

WAN优化/加速软件。

```
https://clealink.jp/products/rapicom/contents/seed/outline/index.html
```

### UDPspeeder

```
https://github.com/wangyu-/UDPspeeder/
```

## 网络托管

### Keen

完全托管的事件流平台。

```
https://keen.io/
```

### awardspace

附带免费短域名。

```
https://www.awardspace.com/
```

### Bip

静态网站托管，七天免费试用。

```
https://bip.sh/
```

### Freehosting

```
https://freehostingnoads.net/
```

### Freehostia

```
https://www.freehostia.com/
```

### HelioHost

```
https://www.heliohost.org/
```

### PonyICU

```
https://pony.icu/
```

### Raidboxes

```
https://raidboxes.io/
```

### Read the Docs

```
https://readthedocs.org/
```

### render

```
https://render.com/
```

### surge

```
https://surge.sh/
```

### HostingTocDo

```
https://hostingtocdo.cyou/
```

### Endless Hosting

```
https://theendlessweb.com/
```

### 000webhost

需要翻墙。

```
https://www.000webhost.com/
```

### surge

面向前端开发人员的静态Web发布。

```
https://surge.sh/
```

### netlify

运行Web项目。

```
https://www.netlify.com/
```

## 网络与企业营销

### 你好现在

```
https://www.ipaynow.cn/productservices
```

### ZSITE

```
https://www.zsite.com/index.html
```

## 网络安全

### imperva

```
https://www.imperva.com/
```

### Opswat

```
https://www.opswat.com/
```

### DUO

```
https://duo.com/
```

### Crypteron

```
https://www.crypteron.com/
```

### 魔盾安全

```
https://www.maldun.com/
```

### 云锁

```
https://www.yunsuo.com.cn/download.html
```

### Comodo

```
https://www.comodo.com/secure-dns/
```

## 网络支付

### stripe

网络支付基础设施。

```
https://stripe.com/hk
```

### temenos

```
https://www.temenos.com/solutions/
```

## 内网穿透、frp与流量转发

### CoNET

用户通过连接第三方公众邮件服务器，接入CoNET的匿名虚拟网络，它的技术被称为折射网络，可以帮助用户逃避被网络监控，访问被网络屏蔽的服务器。

```
https://github.com/CoNETProject/CoNET
```

### Reqrypt

隧道和加密Web浏览器请求的工具。在其他网络审查不像中国那么严格的地区，常搭配着DNSCrypt一起使用进行穿墙。

```
https://github.com/basil00/reqrypt
```

### Dragonite

把TCP转为UDP。

```
https://github.com/dragonite-network/dragonite-java
```

### KCP

```
https://github.com/skywind3000/kcp
```

### kcptun

把TCP转为UDP。

```
https://github.com/xtaci/kcptun
```

### ZeroNet

```
https://github.com/HelloZeroNet/ZeroNet
```

### Dog Tunnel

基于kcp的p2p端口映射工具，同时支持socks5代理。

```
https://github.com/vzex/dog-tunnel
```

### GO Simple Tunnel

gost-GO简单隧道。

```
https://github.com/ginuerzh/gost
```

### 网云穿

送免费体验隧道。

```
https://xiaomy.net/
```

### 魔法隧道

```
http://www.mofasuidao.cn/
```

### 量子互联

```
https://www.nsloop.com/
```

### 路由侠

```
http://www.luyouxia.com/
```

### 花生壳

```
https://hsk.oray.com/
```

### inlets

将本地服务暴露到公网。

```
https://github.com/inlets/inlets/blob/master/README_CN.md
```

### Wisdom

```
http://wdom.net/
```

### NATAPP

```
https://natapp.cn/
```

### Ngrok

提供美国Ngrok免费服务器。

```
https://www.ngrok.cc/
```

### 神卓

免费3天。

```
https://www.shenzhuohl.com/price/index.html
```

### Napvy

```
https://napyy.com/
```

### lu8

免费内网穿透服务及搭建免费内网穿透服务器。

```
https://www.lu8.win/
```

### 铂金frp

```
https://bob.kim/frp
```

### SAKURA FRP

```
https://www.natfrp.com/
```

### QYDEV

```
https://qydev.com/index.html
```

### ngrok

```
https://ngrok.com/
```

### TunnelNat

```
https://www.tunnelnat.com/
```

### lanproxy

```
https://github.com/ffay/lanproxy
```

### 网络通

```
http://www.youtusoft.com/
```

### Notr

```
https://www.notr.tech/
```

# 域名处理

## 域名解析

### He.NET

```
https://ipv6.he.net/certification/register.php
```

### CloudXNS

```
https://www.cloudxns.net/Sign/signup.html
```

### 教程

```
https://51.ruyo.net/3878.html
```

## 域名注册

### freedom

可注册免费域名后缀cf、ga、gq、ml、tk。

注意搜索时需要输入域名全称，如`nionguan.ga`，否则无法将域名加入购物车。完成订单时选择一年免费，在即将到期前续期即可继续免费使用。

```
https://www.freenom.com/zh/index.html?lang=zh
http://www.dot.tk/en/index.html
```

### nic.ua

可注册免费域名后缀pp.ua。注册完成后需要打开邮箱完成验证，然后重新登录账号以查看域名是否已经被分配。若域名仍在等待验证，则需要在Telagram上完成手机验证，用注册时的手机号登录Telegram并搜索`@ppuabot`，按照流程进行即可。

```
https://nic.ua/en/domains/.pp.ua
```

### .tech

可从Github学生包中免费获得。

```
https://get.tech/
```

### Name.com

可从Github学生包中免费获得。

```
https://www.name.com/zh-cn/products
```

### namecheap

学生免费12个月。

```
https://nc.me/
```

### porkbun

.design域名首年免费，优惠码为`EWEBDESIGN19HO`。

```
https://porkbun.com/
```

最新优惠码可通过以下链接获取。

```
https://51.ruyo.net/4862.html
```

### webnames.ca

.ca免费一年，已暂停。

```
https://www.webnames.ca/covid19-nonprofit-response
```

### Exabytes

有5美金的优惠券。

```
http://www.exabytes.com/domains/domain-name-search
```

### 钻磊解析网

```
https://dns.txizd.cn/
```

### co.nr

```
https://www.freedomain.pro/
```

### EU.org

```
https://nic.eu.org/
```

### 快乐域名分发系统

仓库如下。

```
https://github.com/klsf/kldns
```

教程如下。

```
https://51.ruyo.net/4087.html
```

### 奥特二级域名分发系统

```
http://cc.1xie.xyz/
```

# 数据库相关

## GeoIP2国家数据库

### Free IP Geolocation API

```
https://freegeoip.app/
```

### GeoDataSource

```
https://www.geodatasource.com/
```

### Maxmind

```
https://dev.maxmind.com/geoip/geoip2/geolite2/
https://geolite.clash.dev/
https://github.com/Dreamacro/maxmind-geoip
```

## 数据库

### Navicat Premium

可免费试用。

```
https://www.navicat.com.cn/sponsorship/education/student
https://www.navicat.com.cn/products
```

### Scaleway

```
https://www.scaleway.com/en/object-storage/
```

### DataStax

```
https://astra.datastax.com/register
```

### db4free

```
https://db4free.net/
```

### ElephantSQL

```
https://www.elephantsql.com/plans.html
```

### FreeSQLdatabase

```
https://www.freesqldatabase.com/
```

### SQLGate

Github学生包可免费使用一年。

```
https://www.sqlgate.com/student-pack
```

### mLab

mongoDB数据库。

```
https://mlab.com/
```

### Free MySQL Hosting

```
https://www.freemysqlhosting.net/
```

### EasyDB

```
https://easydb.io/
```

### n:point

JSON数据库。

```
https://www.npoint.io/
```

### FREE MYSQL

```
https://remotemysql.com/
```

### ElephantSQL

PostgreSQL。

```
https://www.elephantsql.com/
```

### DataStax Astra

免费使用5GB的空间。

```
https://astra.datastax.com/register
```

### UNBOUNDED

```
https://www.unbounded.cloud/
```

### Lambda Store

```
https://lambda.store/
```

### GrapheneDB

免费试用两周。

```
https://www.graphenedb.com/
```

### EverSQL

数据库优化平台。

```
https://www.eversql.com/
```

### caspio

```
https://free.caspio.com/
```

### iH5

```
http://www.ih5.cn/editor3/?nid=10985497
```

### Navicat Premium 10

```
链接 / http://pan.baidu.com/s/1c2B1t1A
密码 / s3sw
```

### DB DESIGNER

```
https://www.dbdesigner.net/
```

### restdb.io

```
https://restdb.io/
```

### UNBOUNDED

```
https://www.unbounded.cloud/
```

### graphenedb

```
https://www.graphenedb.com/
```

# 文件处理

## 文件下载

### Bitport.io

```
https://bitport.io/pricelist
```

### sonic.SeedBox

```
https://www.sonicseedbox.com/
```

### Torrent Safe

```
https://www.torrentsafe.com/cn/
```

### bytebx

```
https://bytebx.com/
```

### seedr

```
https://www.seedr.cc/
```

### FileStream

```
https://filestream.me/
```

### ZBIGZ

下载种子。

```
https://zbigz.com/
```

### Direct Torrents

```
https://www.direct-torrents.com/
```

### Streamza

```
https://streamza.com/
```

### OffCloud

```
https://www.offcloud.com/
```

### Cloud torrent

自远程torrent客户端。

```
https://github.com/jpillora/cloud-torrent
```

教程如下。

```
https://51.ruyo.net/2723.html
```

### Aria2

仓库如下。

```
https://github.com/aria2/aria2
https://aria2.github.io/
```

安装配置使用教程如下。

```
https://51.ruyo.net/2607.html
```

### Cloud torrent

```
https://github.com/jpillora/cloud-torrent
```

## 文件管理

### 润普

```
http://www.everydo.com/
```

### IPFS

持久且分布式存储和共享文件的网络传输协议。

```
https://hoochanlon.github.io/fq-book/#/ipfs/ipfs-use-naive
https://hoochanlon.github.io/fq-book/#/ipfs/ipfs
```

### tagLyst Next 3

标签式文件资料管理利器。

```
http://www.taglyst.com/
```

### SparkleShare

```
https://www.sparkleshare.org/
```

### Syncthing

文件同步程序。

```
https://syncthing.net/
```

### ZOHO

适用于团队和个人的在线文件管理。

```
https://www.zoho.com.cn/docs/
```

### Cloudreve

```
https://github.com/cloudreve/Cloudreve
```

教程如下。

```
https://xiaoyou66.com/archives/769
```

### MOVER.IO

文件迁移服务。支持OneDrive/GoogleDrive/Dropbox等。

```
https://mover.io/
```

教程如下。

```
https://51.ruyo.net/15587.html
```

### GDIndex

```
https://gdindex-code-builder.glitch.me/
```

部署教程如下。

```
https://51.ruyo.net/14013.html
```

### OLAINDEX

OneDrive文件管理。

```
https://github.com/WangNingkai/OLAINDEX
```

教程如下。

```
https://51.ruyo.net/10067.html
```

### OneList

OneDrive文件管理。

```
https://github.com/malaohu/OneList--
```

教程如下。

```
https://51.ruyo.net/12396.html
```

### Directory Lister

公开任何可通过Web访问的文件夹的内容以进行浏览和共享。

```
https://www.directorylister.com/
```

### Tiny File Manager

基于Web的文件管理器。

```
https://github.com/prasathmani/tinyfilemanager
```

### Z-File

在线文件目录。

```
https://github.com/zhaojun1998/zfile
```

### SeaweedFS

简单且高度可扩展的分布式文件系统。

```
https://github.com/chrislusf/seaweedfs
```

### Syncthing

文件同步程序。

```
https://github.com/syncthing/syncthing
```

### h5ai

HTTP Web服务器的现代文件索引器。

```
https://larsjung.de/h5ai/
```

### ocDownloader

```
https://github.com/e-alfred/ocdownloader
```

### Nextcloud

```
https://docs.nextcloud.com/server/17/user_manual/
https://github.com/nextcloud/docker
```

### filebrowser

```
https://github.com/filebrowser/filebrowser
```

教程如下。

```
https://51.ruyo.net/5570.html
```

### 微力同步

高效的数据传输工具。

```
http://www.verysync.com/
```

### FileRun

自托管文件同步和共享。

```
https://filerun.com/
```

网盘程序部署使用教程如下。

```
https://51.ruyo.net/5309.html
```

### Furk.net

获取媒体文件并立即流式传输。

```
https://www.furk.net/
```

### KodExplorer

网络文件管理器。

```
https://github.com/kalcaddle/KODExplorer
https://kodcloud.com/
```

### MultCloud

跨网盘传输文件在线服务。

```
https://www.multcloud.com/
```

### Air Explorer

全球各大网盘服务之间文件同步工具。免费版每个云服务器只能登陆1个账号，不支持计划任务，不支持限速。

```
https://www.airexplorer.net/en/
```

### filestack

上载，转换任何文件并将其传送到应用中。

```
https://www.filestack.com/
```

# 安全扫描/沙箱

## 微步在线云沙箱

```
https://s.threatbook.cn/
```

## Hybrid Analysis

```
https://www.hybrid-analysis.com/
```

## 腾讯哈勃分析系统

```
https://habo.qq.com/
```

## Sndbox

```
https://app.sndbox.com/
```

## Anxin

```
https://scan.anxinsec.com/
```

## 奇安信网站卫士

```
https://wangzhan.qianxin.com/
```

# 网页与网站处理

## 网页APP制作

### anvil

```
https://anvil.works/
```

## 网站监控、记录和测试

### nynex

```
https://nynex.de/tools/
```

### bearer

```
https://www.bearer.sh/
```

### mmTrix

云评测、云监测、云优化。

```
http://www.mmtrix.com/price
```

### acunote

项目管理和Scrum软件。

```
https://www.acunote.com/
```

### DOMINO

企业数据科学团队的记录系统。

```
https://www.dominodatalab.com/
```

### Instabug

通过全面的错误和崩溃报告，性能监控以及实时用户调查和反馈。

```
https://instabug.com/
```

### CARTO

空间分析资源。

```
https://carto.com/industries/
```

### Statping

状态页和监视服务器。

```
https://github.com/statping/statping
```

### UptimeRobot

运行时间监控服务。

```
https://uptimerobot.com/
```

### Atlassian

向用户传达实时状态。

```
https://www.atlassian.com/zh/software/statuspage
```

### ServerStatus-Hotaru

云探针、多服务器探针、云监控、多服务器云监控。

```
https://github.com/CokeMine/ServerStatus-Hotaru
```

### falcon-plus

开源的企业级监视系统。

```
https://github.com/open-falcon/falcon-plus
```

### Grafana

用于监视和观察的开源平台。

```
https://github.com/grafana/grafana
```

### NETDATA

对系统，容器，应用程序和基础架构的最佳监视和故障排除。

```
https://github.com/netdata/netdata
```

### Glances

跨平台的监视工具。

```
https://github.com/nicolargo/glances
```

### Statusfy

状态页面系统。

```
https://github.com/juliomrqz/statusfy
https://marquez.co/statusfy?ref=aceforth
```

### Cachet

开源状态页面系统。

```
https://cachethq.io/
```

### GoAccess

```
https://goaccess.io/
```

### import.io

```
https://www.import.io/standard-plans/
```

### Ghost Inspector

```
https://ghostinspector.com/
```

### ZuluTrade

```
https://www.zulutrade.cn/?Lang=zh-CN
```

### pyup

确保Python依赖项安全，最新且合规。

```
https://pyup.io/
```

### .Contriber

```
https://www.contriber.com/
```

### Quickmetrics

无需信用卡，提供免费套餐。

```
https://quickmetrics.io/
```

### Bugfender

远程记录器，崩溃报告器和应用内用户反馈。

```
https://bugfender.com/
```

### loggky

```
https://www.loggly.com/
```

### humio

```
https://www.humio.com/pricing/
```

### sematext

14天免费试用。

```
https://sematext.com/logsene
```

### papertrail

七天免费试用。

```
https://www.papertrail.com/plans/
```

### logz.io

一天免费试用。

```
https://logz.io/
```

### logentries

免费试用。

```
https://logentries.com/
```

### sumo logic

90天免费试用。

```
https://www.sumologic.com/
```

### Redsmin

```
https://www.redsmin.com/
```

### Datadog

#### 官网

```
https://www.datadoghq.com/
```

#### 教育版

```
https://studentpack.datadoghq.com/
```

### freeboard

```
https://freeboard.io/
```

### freshping

```
https://www.freshworks.com/website-monitoring/
```

### Circonus

```
https://www.circonus.com/
```

### inspector

可免费试用。

```
https://www.inspector.dev/
```

### Assertible

```
https://assertible.com/
```

### AppNeta

```
https://www.appneta.com/
```

### APImetrics

针对开发人员，消费者，提供者和监管者的实时生产API监视。

```
https://apimetrics.io/
```

### Pingmeter

```
https://pingmeter.com/
```

### amixr

```
https://amixr.io/
```

### CloudSploit

```
https://cloudsploit.com/
```

### Typo CI

检查您的提交中是否存在拼写错误，可免费试用。

```
https://github.com/marketplace/typo-ci
```

### GTmetrix

```
https://gtmetrix.com/
```

### Server酱

从服务器推报警和日志到手机的工具。

```
https://sc.ftqq.com/3.version
```

### redsmin

```
https://www.redsmin.com/
```

## 网站搭建

### layer

web弹层组件。

```
https://layer.layui.com/
```

### typecho

博客平台。

```
https://typecho.org/
```

### ghost

博客平台。

```
https://ghost.org/
```

### Halo

博客平台。

```
https://halo.run/
```

### Jekyll

博客平台。

```
https://github.com/jekyll/jekyll
```

### Caddy

```
https://caddyserver.com/
```

实现反向代理/镜像的教程如下。

```
https://51.ruyo.net/3461.html
```

### FreeYun

网络验证。

```
http://www.freeyun.net/
```

### fenix

Web服务器。

```
https://preview.fenixwebserver.com/
```

### zmirror

Python反向HTTP代理程序。

```
https://github.com/aploium/zmirror
```

### FraudLabs Pro

帮助商家保护其在线电子商务商店免受恶意欺诈者的侵害。

```
https://www.fraudlabspro.com/
```

### pipedream

通过REST或SSE API检查每个事件。

```
https://requestbin.com/
```

### Webhook.site

测试和自动化任何传入的HTTP请求或电子邮件。

```
https://webhook.site/#!/fa7fde59-b4b9-490f-89c8-171dd07c4ab7
```

### loginradius

身份验证。

```
https://www.loginradius.com/
```

### thedev.id

Web开发验证。

```
https://github.com/fransallen/thedev.id
```

### blockspring

增强列表构建，报告和登录页面的功能。

```
https://www.blockspring.com/
```

### umso

```
https://www.umso.com/
```

### neocities

```
https://neocities.org/
```

## 网页/博客生成

### Baklib

免费搭建博客/文档/手册平台。

```
https://www.baklib.com/
```

### pancake

博客平台，14天免费试用，然后5美元一个月。

```
https://www.pancake.io/
```

教程如下。

```
https://51.ruyo.net/3562.html
```

### postach.io

免费博客平台。

```
https://postach.io/
```

### hashnode

```
https://hashnode.com/
```

### Staticman

```
https://staticman.net/
```

### neocities

```
https://neocities.org/
```

### netlify

```
https://www.netlify.com/
```

### Tilda

```
https://tilda.cc/
```

### Versoly

```
https://versoly.com/
```

### Pantheon

```
https://pantheon.io/
```

## 博客评论插件

### Disqus

```
https://disqus.com/
```

### txti

```
http://txti.es/
```

### utterances

```
https://utteranc.es/
```

## 网站与代码检查

### browserling

实时交互式跨浏览器测试。

```
https://www.browserling.com/
```

### AccessLint

在代码上线之前检查更改和注释以及任何新的可访问性问题。

```
https://github.com/marketplace/accesslint
```

### Testin云测

云测试服务、AI数据标注服务、安全服务及推广服务。

```
https://www.testin.cn/
```

### HTTPS检测工具

```
http://s.tool.chinaz.com/https?url=
```

### Qualys SSL Labs

SSL服务器测试。

```
https://www.ssllabs.com/ssltest/index.html
```

### DNSViz

检查网站是否有DNSSEC。

```
https://dnsviz.net/
```

### HTTP/2检测

```
(function(){
    // 保证这个方法只在支持loadTimes的chrome浏览器下执行
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

### 17ce

```
https://www.17ce.com/
```

### EveryStep

网站测试自动化。

```
https://www.everystep-automation.com/
```

### KEYCHEST

速度测试。

```
https://keychest.net/speedtest
```

### dareboost

```
https://www.dareboost.com/en
```

### Observatory

```
https://observatory.mozilla.org/
```

### Internet.nl

```
https://internet.nl/
```

### DJ Checkup

```
https://djcheckup.com/
```

### Webdir+

```
https://scanner.baidu.com/#/pages/intro
```

### IP.City

```
https://ip.city/
```

### IPList.cc

```
https://iplist.cc/
```

### IP2Location

```
https://www.ip2location.com/
```

### iptrace.io

```
https://iptrace.io/
```

# 编程处理

## 编译工具

### JetBrains

包括Clion、Pycharm等。

#### 学生包

```
https://www.jetbrains.com/community/education/#students
```

#### 激活

```
http://shaofan.org/jetbrains-active/
https://www.jetbrains.com/shop/eform/opensource
```

### BELSoft

Java平台和应用。

```
https://bell-sw.com/
```

### Jupyter

```
https://www.cnblogs.com/jayafsmw/p/12286580.html
https://blog.csdn.net/hxpjava1/article/details/86763331
```

## 在线编译

### ShiftEdit

用于开发网站的功能强大的在线IDE。

```
https://shiftedit.net/about
```

### 1MB

```
https://1mb.co/
```

### codesandbox

```
https://codesandbox.io/s/wild-hill-f9e9p?file=/src/App.js
```

### JSFiddle

```
https://jsfiddle.net/
```

### codepad

```
http://codepad.org/
```

### JS Bin

```
https://jsbin.com/
```

### ideone

```
https://ideone.com/
```

### 学而思编程

支持C。

```
https://code.xueersi.com/ide/code/1
```

### codingground

```
https://www.tutorialspoint.com/codingground.htm
```

### apiary

```
https://apiary.io/
```

### microlink

```
https://microlink.io/
```

### Code Snippets online

```
https://codesnip.com.br/
```

## 云编程环境

### Play with Docker

```
https://labs.play-with-docker.com/
```

### Play with Kubernetes

```
https://labs.play-with-k8s.com/
```

### Visual Studio Codespaces VS Code

```
https://docs.microsoft.com/zh-cn/visualstudio/codespaces/how-to/vscode#self-hosted
```

### jupyter notebook

基于web端的IDE。

```
https://xiaoyou66.com/archives/1095
```

### pythonanywhere

```
https://www.pythonanywhere.com/
```

### Cloud Studio

支持连接到云主机，生成在线预览链接。

```
https://cloudstudio.net/
```

### Colaboratory

编写和执行Python代码，可免费使用GPU。

```
https://colab.research.google.com/notebooks/intro.ipynb
```

### kaggle

提供CPU和GPU以运行代码。

```
https://www.kaggle.com/
```

### repl.it

协作式在线编程，可推送到Github。

```
https://repl.it/
```

### Deepnote

协作式在线编程，可推送到Github。

```
https://deepnote.com/
```

### trinket

协作式在线编程。

```
https://trinket.io/
```

### jor1k

带有网络的Linux的JS编程环境。

```
http://s-macke.github.io/jor1k/demos/main.html?user=WnxlHGdQBY&cpu=asm&n=1&relayURL=wss%3A%2F%2Frelay.widgetry.org%2F
```

### conveyor

远程访问Visual Studio Web应用程序。

```
https://conveyor.cloud/
```

### localhost.run

共享本地主机环境。

```
https://localhost.run/
```

### Pusher

```
https://pusher.com/
```

### Gitpod

```
https://www.gitpod.io/
```

### GitDuck

```
https://gitduck.com/
```

### termible

基于Docker的网站终端。

```
https://termible.io/
```

### codepen

```
https://codepen.io/
```

### codesandbox

```
https://codesandbox.io/s/
```

### Eclipse Che

```
https://www.eclipse.org/che/
```

### JDOODLE

```
https://www.jdoodle.com/
```

# API相关

## API调用

### GraphQL Inspector

更好地维护和改进GraphQL API以及GraphQL使用者。

```
https://github.com/marketplace/graphql-inspector
```

### axway

```
https://www.axway.com/en/streamdataio
```

### Tyk

API和服务管理平台。

```
https://tyk.io/
```

### AIPage

```
https://aipage.baidu.com/
```

### Time Door

用于集成时间序列分析的API。

```
https://timedoor.io/
```

### SOFODATA

从CSV文件创建API。

```
https://www.sofodata.com/
```

### Postbacks

免费8000个回调。

```
https://www.postbacks.io/
```

### calendarific

RESTful API。

```
https://calendarific.com/
```

### Cloudmersive

文件和资料转换API。

```
https://cloudmersive.com/
```

### currencyscoop

```
https://currencyscoop.com/
```

### DreamFactory

数据库API。

```
https://www.dreamfactory.com/
```

### postman

API开发协作平台。

```
https://www.postman.com/
```

### microlink.io

```
https://microlink.io/
```

### ipgeolocation

```
https://ipgeolocation.io/
```

### Vattly

```
https://vattly.com/
```

### WrapAPI

```
https://wrapapi.com/
```

### zenscrape

网络抓取API。

```
https://zenscrape.com/
```

### happi.dev

```
https://happi.dev/
```

### OCRSpace

OCR API。

```
https://ocr.space/
```

### stream

聊天API。

```
https://getstream.io/
```

### imagecharts

生成图表图像的API。

```
https://www.image-charts.com/
```

### reSmush

提供图像优化的免费API。

```
https://resmush.it/
```

### abstract

API​​抽象套件。

```
https://www.abstractapi.com/
```

### BigData Cloud

大数据云API。

```
https://www.bigdatacloud.com/
```

### OpenAPI 3

```
https://interaction-design.co.za/openapidesigner/
```

### ip-api

```
https://ip-api.com/
```

## API模拟

### Pixela

记录并跟踪习惯或努力。

```
https://pixe.la/
```

### microenv

REST API模拟。

```
https://microenv.com/
```

### mockoon

API模拟，测试和重新创建所有类型HTTP和HTTPS响应。

```
https://mockoon.com/
```

### RunKit

提供模拟JS环境，可测试npm模块。

```
https://runkit.com/home
```

### micro-jaymock

微型API模拟微服务，用于生成伪造的JSON数据。

```
https://micro-jaymock.now.sh/
```

### JSONPlaceholder

免费使用伪造的在线REST API进行测试和原型设计。

```
http://jsonplaceholder.typicode.com/
```

### fake{JSON}

```
https://fakejson.com/
```

### apiary

```
https://apiary.io/
```

### My Fake API

```
https://myfakeapi.com/
```

### ETF Data API

```
https://etf-data.com/
```

### API Mocha

```
https://apimocha.com/
```

### mockapi.io

模拟REST API。

```
https://www.mockapi.io/
```


# WHMCS

## 微信支付网关

```
链接 / http://pan.baidu.com/s/1nu6FV65
密码 / ruyo
```

## WHMCS Shadowsocks

```
https://github.com/NETOUCHER/WHMCS_Shadowsocks_Admin/wiki/Home:-SSAdmin-User-Guide
```

## 支付宝

```
链接 / https://pan.baidu.com/s/1nvv1dbF
密码 / n4kr
```

教程如下。

```
https://www.freehao123.com/whmcs-alipay/
```

# 应用分发

## 手机应用分发

### RabbitMQ

开源消息代理。

```
https://www.rabbitmq.com/
```

### PubNub

远程交互。

```
https://www.pubnub.com/
```

### fir.im

```
https://www.jappstore.com/
```

## 系统应用开发

CI/CD及低代码。

### twilio

```
https://www.twilio.com/
```

### Deis Workflow

部署和管理应用程序。

```
https://github.com/teamhephy/workflow
```

### twilio

```
https://www.twilio.com/
```

### CloudFoundry

```
https://www.cloudfoundry.cn/
```

### Flynn

```
https://flynn.io/
```

### PaaSfinder

```
https://paasfinder.org/vendors
```

### Dokku

构建和管理应用程序的生命周期。

```
http://dokku.viewdocs.io/dokku/
```

### RIO

```
https://rio.io/
```

### Nanobox

```
https://nanobox.io/
```

### Koding

创建和共享全自动开发环境。

```
https://www.koding.com/
```

### convox

部署，扩展和管理应用程序。

```
https://convox.com/
```

### Rainbond

```
https://www.rainbond.com/
```

### CapRover

适用于NodeJS，Python，PHP，Ruby，Go应用程序的最简单的应用程序/数据库部署平台和Web服务器软件包。

```
https://github.com/caprover/caprover
```

### influxdata

构建和操作时间序列应用程序的平台。

```
https://www.influxdata.com/
```

### APPDYNAMICS

现代化基础架构。

```
https://www.appdynamics.com/
```

### Airtable

```
https://airtable.com/
```

### Scrutinizer

软件质量管理平台。

```
https://scrutinizer-ci.com/
```

### Vercel

```
https://vercel.com/
```

部署OneIndex教程如下。

```
https://51.ruyo.net/9976.html
```

### whales

将应用程序Docker化。

```
https://github.com/Gueils/whales
```

### cloudno.de

```
https://cloudno.de/
```

### FOSSA

用于企业安全性和合规性的最完整的开源管理和策略引擎。

```
https://fossa.com/
```

### OneAPM

可免费试用。

```
https://www.oneapm.com/
```

### KintoHub

将后端服务，网站，cron作业，数据库以及您的应用程序所需的所有内容整合并部署到一个地方。

```
https://www.kintohub.com/
```

### Deta

构建应用程序。

```
https://www.deta.sh/
```

### FeaturePeek

UI/UX团队的部署预览。

```
https://featurepeek.com/
```

### openode

应用开发。

```
https://www.openode.io/admin
```

### maxLeap

应用开发。

```
https://maxleap.cn/s/web/zh_cn/devcenter-pricing.html
```

### Power Apps

快速构建和共享低代码应用。

```
https://powerapps.microsoft.com/zh-cn/
```

### 禅道

项目管理软件。

```
https://www.zentao.net/
```

### New Relic

帮助工程师创建更完善的软件。从整体到无服务器，您可以检测所有内容，然后进行分析，故障排除和优化整个软件堆栈。

```
https://newrelic.com/
```

### Apex Up

部署无限扩展的无服务器应用程序，API和静态网站。

```
https://apex.sh/up/
```

### Drone

自动化啊软件构建和测试。

```
https://drone.io/
```

### bitnami

```
https://bitnami.com/
```

### kaltura

2个月的VPaaS免费试用，$200的赠金。

```
https://corp.kaltura.com/video-paas/registration/
```

### Gigalixir

```
https://gigalixir.com/
```

### H3BPM

```
https://www.h3bpm.com/
```

### Google App Maker

```
https://developers.google.com/appmaker
```

### 云表

```
https://www.iyunbiao.com/
```

### iVX.cn

```
https://pricing.ivx.cn/
```

### 云枢

```
https://www.cloudpivot.cn/
```

### 宜搭

```
https://www.aliwork.com/
```

### outsystems

```
https://www.outsystems.com/pricing-and-editions/
```

### mendix

```
https://www.mendix.com/
```

### Enhancer

一站式开发平台。

```
https://wuyuan.io/
https://enhancer.io/
```

### Workfine

企业办公系统快速开发与应用平台。

```
http://www.bossietech.com/
```

### bit

构建和重用组件的可伸缩的协作方式。免费试用。

```
https://bit.dev/
```

### Stackblitz

```
https://stackblitz.com/
```

### fauna

```
https://fauna.com/
```

### 易开发

```
http://dev.easydo.cn/home/@zopen.portlet:view_all_overview
```

### 进云

```
http://www.jinyunweb.com/
```

### putdb

```
http://www.putdb.com/
```

### 捷得

可以免费试用。

```
https://www.joget.cn/
```

### Back4App

```
https://www.back4app.com/
```

### .bubble

```
https://bubble.io/
```

### bitrise

```
https://www.bitrise.io/
```

### Buddy

```
https://buddy.works/
```

### buddybuild

```
https://www.buddybuild.com/
```

### circleci

```
https://circleci.com/
https://cirrus-ci.org/
```

### codemagic

```
https://codemagic.io/start/
```

### codeship

```
https://codeship.com/
```

### AppVeyor

```
https://www.appveyor.com/
```

### 蒲公英

```
https://www.pgyer.com/
```

### APICloud

```
https://www.apicloud.com/
```

### 个推

可免费试用。

```
https://www.getui.com/
```

### HASURA

```
https://hasura.io/
```

### Bootify

```
https://bootify.io/
```

### Apache NetBeans

开发环境，工具平台和应用程序框架。

```
https://netbeans.apache.org/
```

### AppSheet

```
https://www.appsheet.com/
```

### thunkable

```
https://thunkable.com/#/
```

### 华炎魔方

```
https://www.steedos.com/pricing/platform/
```

### 轻流

```
https://qingflow.com/
```

### 牛刀

```
https://www.newdao.net/
```

### odoo

```
https://www.odoo.com/zh_CN/
```

### ZOHO

```
https://www.zoho.com/creator/
```

### ClickPaaS

```
https://www.clickpaas.com/
```

### 宜创无代码

```
https://www.wudaima.com/
```

### learun

```
https://www.learun.cn/
```

### 活字格

```
https://www.grapecity.com.cn/solutions/huozige
```

### amis

```
https://baidu.gitee.io/amis/docs/index
```

### Liteflow

```
https://liteflow.com/
```

### zapier

```
https://zapier.com/
```

### Configure.IT

```
https://www.configure.it/
```

### CodenameOne

```
https://www.codenameone.com/
```

### DronaHQ

```
https://www.dronahq.com/
```

### pipedream

```
https://pipedream.com/
```

### fly.io

```
https://fly.io/
```

### backendless

```
https://backendless.com/
```

### PUSH Technology

```
https://www.pushtechnology.com/
```

### StackStorm

连接所有应用程序，服务和工作流程。

```
https://stackstorm.com/
```

### Kong

提供API管理，Ingress和Service Mesh。

```
https://konghq.com/
```

### outsystems

```
https://www.outsystems.com/
```

### Progress

```
https://www.progress.com/kinvey
```

# 搜索引擎

## Startpage

```
https://www.startpage.com/
```

## Ask

```
https://www.ask.com/
```

## NAVER

```
https://www.naver.com/
```

## DuckDuckGo

```
https://duckduckgo.com/
```

## Ecosia

```
https://www.ecosia.org/
```

## Yandex

```
https://yandex.com/
```

## Qwant

```
https://www.qwant.com/
```

## Shodan

互联网连接设备搜索引擎。

```
https://www.shodan.io/
```

## Whoogle Search

获取Google搜索结果，但不包含任何广告，javascript，AMP链接，cookie或IP地址跟踪。一键式轻松部署为Docker应用程序，并且可通过单个配置文件进行自定义。

```
https://github.com/benbusby/whoogle-search
```

# DevOps

## UNPKG

npm上所有内容的快速全球内容交付网络。

```
https://unpkg.com/
```

## blackfire

```
https://blackfire.io/
```

## alwaysdata

```
https://www.alwaysdata.com/en/
```

## cloud66

```
https://www.cloud66.com/
```

## GitLab

在30天内无限访问GitLab的所有功能。

```
https://about.gitlab.com/
```

## appflow

免费试用。

```
https://ionicframework.com/appflow
```

## amezmo

```
https://www.amezmo.com/
```

## appcircle

```
https://appcircle.io/
```

## codefresh

```
https://codefresh.io/
```

# 图片处理

## Gumlet

提供正确尺寸和质量的图像。

```
https://www.gumlet.com/
```

## Imgbot

优化图像并节省时间。

```
https://github.com/marketplace/imgbot
```

## Robohash

从任何文本中生成图片。

```
https://robohash.org/
```

# Hosts

## 源

### GoogleHosts

```
https://github.com/googlehosts/hosts
```

### ipv6-hosts

```
https://github.com/lennylxx/ipv6-hosts
```

## 管理工具

快速切换hosts。

### SwitchHosts!

全平台均适用。

```
https://oldj.github.io/SwitchHosts/
```

### Gas Mask

适用于Mac。

```
https://www.macupdate.com/app/mac/29949/gas-mask/
```

### Hozz

全平台均适用。

```
https://blog.zhangruipeng.me/Hozz/
```

# 密码管理

## LastPass

### 教育版

```
https://lastpass.com/edupromo.php
```

## bitwarden

```
https://bitwarden.com/
```

## keybase

```
https://keybase.io/
```

## RoboForm

### 教育版

```
https://www.roboform.com/promotions/college
```

## LoginTC

可免费试用。

```
https://www.logintc.com/
```

## Have I Been Pwned

检查密码是否已经泄露。

```
https://haveibeenpwned.com/
```

## onelogin

```
https://www.onelogin.com/
```

## CloudSploit

```
https://cloudsploit.com/
```

## BitNinja

```
https://bitninja.io/
```

## Auth0

```
https://auth0.com/
```

# 机器学习

## Mapbox

```
https://www.mapbox.com/community/education/
```

## Vertabelo

专业数据建模。

```
https://www.vertabelo.com/
```

## SAS教育版

进行统计分析，数据挖掘和预测。

```
https://www.sas.com/en_us/software/university-edition.html
https://www.sas.com/en_us/software/on-demand-for-academics.html
```

## MinIO

构建用于机器学习，分析和应用程序数据工作负载的高性能基础架构。

```
https://github.com/minio/minio
```

教程如下。

```
https://www.digitalocean.com/community/tutorials/how-to-set-up-an-object-storage-server-using-minio-on-ubuntu-18-04
```

## datapane

用Python创建和发布交互式报告。

```
https://datapane.com/
```

## MonkeyLearn

训练定制的机器学习模型以获取主题，情感，意图，关键字等。

```
https://monkeylearn.com/
```

## tamber

```
https://tamber.com/
```

## Algorithmia

开发机器学习运营成熟度。

```
https://algorithmia.com/
```

## bigML

```
https://bigml.com/
```

## clarifai

计算机视觉和人工智能。

```
https://www.clarifai.com/
```

# 教育相关

## IdeaScale

思想管理平台。

```
https://ideascale.com/
```

## COCALC

远程授课。

```
https://cocalc.com/
```

## write.as

在线写作，14天免费试用。

```
https://write.as/
```

# 邮箱

## mailgun

发送，接收和跟踪电子邮件。

```
https://www.mailgun.com/
```

使用教程如下。

```
https://51.ruyo.net/4188.html
```

## emkei

伪造电子邮件发件人。

```
https://emkei.cz/
```

## 密信

电子邮件加密。

```
https://www.mesign.com/zh-cn/index.html
```

# 部分代码

## 生成指定前缀信用卡卡号

### Python代码

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

### 调用示例

```
#生成前缀为'123456',1个,卡号长度16位
fakecard = credit_card_number('123456')
#生成前缀为'123456',1个,卡号长度16位
fakecard = credit_card_number('123456',1,16)
#生成前缀为'123'或'234'的信用卡卡号，共15个，卡号长度16位
fakecard = credit_card_number(['123','234'], 15,16)
```

### 简单封装

```
wget https://raw.githubusercontent.com/malaohu/ruyo-shell/master/credit_card_number.py
python credit_card_number.py
```

# 教程

## 大陆访问Discord

大陆访问Discord不了的可以尝试编辑电脑hosts文件。

```
104.16.248.144   dl.discordapp.net
104.16.60.37   gateway.discord.gg
104.16.11.231  status.discordapp.com
104.16.9.231    cdn.discordapp.com
104.16.59.5  discordapp.com
104.16.248.144 media.discordapp.net
104.16.249.144 images-ext-2.discordapp.net
104.16.248.144 images-ext-1.discordapp.net
151.101.78.214  static-cdn.jtvnw.net
```

## Paypal优惠券/现金券套现

注册后切换货币为美金。购买前最好先联系客服。

```
https://www.on9gamer.com/directory/point_card/%E4%B8%AD%E5%9B%BD%E6%94%AF%E4%BB%98%E5%B9%B3%E5%8F%B0%E7%82%B9-Ping-Tai-Dian-(CN)/463
```

## 从微软官方直接下载Windows 10的ISO镜像

打开以下链接，按F12，切换为手机浏览视图即可看到下载按钮。

```
https://www.microsoft.com/zh-cn/software-download/windows10/
```

## Chrome安装离线扩展程序

### 法一

将crx扩展名重命名为zip后解压，然后打开Chrome并进入扩展程序，点击`加载已解压的扩展程序`，选择刚才解压好的文件夹即可。

### 法二

在Chrome浏览器中打开以下链接，搜索`extensions-on-chrome-urls`并打开即可。

```
chrome://flags
```

# 软件

## Maple

数学建模软件。

## Mathematica

数学建模软件。

```
https://www.zhihu.com/question/27834147/answer/855131318
```

## nexusfile

适用于Windows的快速强大的文件管理器。

```
http://www.xiles.net/
```

## AJJCut

剪掉爱剪辑的片头和片尾。

```
https://github.com/117503445/AJJCut
```

## 诺顿杀毒软件

激活码提取方法教程如下。

```
https://51.ruyo.net/3965.html
```

## McAfee

```
https://home.mcafee.com/root/LandingPage.aspx?affid=1208&cid=154623&culture=EN-US&lpname=mls_1yr_adt
```

## EditPlus

注册码在线网站如下。

```
https://51.ruyo.net/test/editplus-generate.html
```

源码如下。

```
var list = [0,49345,49537,320,49921,960,640,49729,50689,1728,1920,51009,1280,50625,50305,1088,52225,3264,3456,52545,3840,53185,52865,3648,2560,51905,52097,2880,51457,2496,2176,51265,55297,6336,6528,55617,6912,56257,55937,6720,7680,57025,57217,8000,56577,7616,7296,56385,5120,54465,54657,5440,55041,6080,5760,54849,53761,4800,4992,54081,4352,53697,53377,4160,61441,12480,12672,61761,13056,62401,62081,12864,13824,63169,63361,14144,62721,13760,13440,62529,15360,64705,64897,15680,65281,16320,16000,65089,64001,15040,15232,64321,14592,63937,63617,14400,10240,59585,59777,10560,60161,11200,10880,59969,60929,11968,12160,61249,11520,60865,60545,11328,58369,9408,9600,58689,9984,59329,59009,9792,8704,58049,58241,9024,57601,8640,8320,57409,40961,24768,24960,41281,25344,41921,41601,25152,26112,42689,42881,26432,42241,26048,25728,42049,27648,44225,44417,27968,44801,28608,28288,44609,43521,27328,27520,43841,26880,43457,43137,26688,30720,47297,47489,31040,47873,31680,31360,47681,48641,32448,32640,48961,32000,48577,48257,31808,46081,29888,30080,46401,30464,47041,46721,30272,29184,45761,45953,29504,45313,29120,28800,45121,20480,37057,37249,20800,37633,21440,21120,37441,38401,22208,22400,38721,21760,38337,38017,21568,39937,23744,23936,40257,24320,40897,40577,24128,23040,39617,39809,23360,39169,22976,22656,38977,34817,18624,18816,35137,19200,35777,35457,19008,19968,36545,36737,20288,36097,19904,19584,35905,17408,33985,34177,17728,34561,18368,18048,34369,33281,17088,17280,33601,16640,33217,32897,16448];
var hexchars = ['0','1','2','3','4','5','6','7','8','9','A','B','C','D','E','F'];
var regcode = new Array(29);
var i = 0, j = 0, k = 0;
var len, temp, sum, result;
var username = document.getElementById("username").value;
username = username.replace(/^\s+|\s+$/g, "");
 
for(i = 0;i < 5;i++,k++)
{
    for(j = 0;j < 5;j++,k++)
    {
        regcode[k] = hexchars[parseInt(Math.random() * 16)];
    }
    if(k == 29)
        break;
    regcode[k] = '-';
}
    
len = username.length;
 
sum = 1;
for(i = 0;i < len;i++)
    sum += username.charCodeAt(i);
temp = (parseInt( (sum + 23) / 6 ) + 3) * 7 % 16;
regcode[6] = hexchars[temp & 0xF];

sum = 1;
for(i = 0;i < len;i++)
    sum += username.charCodeAt(i);
temp = parseInt( (3 * sum + 39) / 8 ) % 16;
regcode[9] = hexchars[temp & 0xF];

sum = 1;
for(i = 0;i < len;i++)
    sum += username.charCodeAt(i);
temp = parseInt( (3 * sum + 19) / 9 ) % 16;
regcode[7] = hexchars[temp & 0xF];

sum = 1;
for(i = 0;i < len;i++)
    sum += username.charCodeAt(i);
temp = parseInt( (sum + 10) / 3 ) * 8 % 16;
regcode[10] = hexchars[temp & 0xF];

sum = 1;
for(i = 0;i < len;i++)
    sum += username.charCodeAt(i);
temp = (parseInt( (9 * sum + 10) / 3 ) + 36) % 16;
regcode[4] = hexchars[temp & 0xF];

sum = 1;
for(i = 0;i < len;i++)
    sum += username.charCodeAt(i);
temp =  parseInt( (5 * sum + 11) / 5 ) % 16;
regcode[8] = hexchars[temp & 0xF];
 
result = 0;
for(i = 0;i < len;i++)
    result = ((result >> 8) & 0xFF) ^ list[username.charCodeAt(i) ^ (result & 0xFF)];
result = result.toString(16).toUpperCase();
regcode[2] = result.charAt(0);
regcode[3] = result.charAt(1);

len = regcode.length;
result = 0;
for(i = 2;i < len;i++)
    result = ((result >> 8) & 0xFF) ^ list[regcode[i].toString().charCodeAt(0) ^ (result & 0xFF)];
result = result.toString(16).toUpperCase();
regcode[0] = result.charAt(0);
regcode[1] = result.charAt(1);
console.log(regcode.join(''));
```

## XSHELL+XFTP

### 学生包

```
https://www.netsarang.com/zh/free-for-home-school/
```

### 破解

使用前需在hosts文件添加以下内容。

```
127.0.0.1 transact.netsarang.com
127.0.0.1 update.netsarang.com
127.0.0.1 www.netsarang.com
127.0.0.1 www.netsarang.co.kr
127.0.0.1 sales.netsarang.com
```

注册码如下。

```
// Xshell 5
181226-111351-999033
 
// Xshell 6
181226-111725-999177
 
// XshellPlus 6
181226-117860-999055
 
// Xftp 5
181227-114744-999004
 
// Xftp 6
181227-114016-999644
 
// XmanagerPowerSuite 6
181226-116119-999510
```

### 配色方案

打开XShell，选择工具-配色方案-导入即可。

#### 方案一

文件名称为mycolor.xcs。

```
[mycolor]
text(bold)=e9e9e9
magenta(bold)=ff00ff
text=00ff80
white(bold)=fdf6e3
green=80ff00
red(bold)=ff0000
green(bold)=3c5a38
black(bold)=808080
red=ff4500
blue=00bfff
black=000000
blue(bold)=1e90ff
yellow(bold)=ffff00
cyan(bold)=00ffff
yellow=c0c000
magenta=c000c0
background=042028
white=c0c0c0
cyan=00c0c0
[Names]
count=1
name0=mycolor
```

#### 方案二

文件名称为monokai.xcs。

```
[monokai]
text=ffffff
cyan(bold)=a6e22e
text(bold)=ffffff
magenta=f92672
green=80ff80
green(bold)=80ff80
background=272822
cyan=a6e22e
red(bold)=de8e30
yellow=66d9ef
magenta(bold)=f92672
yellow(bold)=66d9ef
red=de8e30
white=c0c0c0
blue(bold)=ae81ff
white(bold)=ffffff
black=000000
blue=ae81ff
black(bold)=000000
[Names]
name0=monokai
count=1
```

#### 方案三

文件名称为solarized.xcs。

```
[Solarized Dark]
text=BDB76B
text(bold)=FFD700
background=002B36
red=CB4B16
red(bold)=E14B16
magenta=6C71C4
magenta(bold)=7684D8
yellow=B58900
yellow(bold)=C99D00
blue=268BD2
blue(bold)=26A0D2
cyan=2AA198
cyan(bold)=2AB5AC
green=639A07
green(bold)=77AE1B
white=eee8d5
white(bold)=F8F2DF
black=143F4A
black(bold)=004954
[Names]
name0=Solarized Dark
count=1
```

## Match Tracer

正则表达式编写及调试工具。

```
http://www.regexlab.com/zh/mtracer/
```

## 卡巴斯基

```
https://www.kaspersky.com.cn/free-antivirus
```

## cFosSpeed

网络质量以及本地延迟优化软件。网站需要翻墙才能打开。

```
http://www.cfos.de/zh-cn/index.htm
```

教程如下。

```
https://51.ruyo.net/5397.html
```

# 其它

## MemCachier

托管内存缓存。

```
https://www.memcachier.com/
```

## Buttondown

生成新闻通讯。

```
https://buttondown.email/
```

## Public Resources on SAE

SAE公共资源。

```
http://lib.sinaapp.com/
```

## transifex

基于云的翻译管理。

```
https://www.transifex.com/
```

## Atlas

工具包。

```
https://atlastk.org/
```

## APIFY

从任何网站提取数据。

```
https://apify.com/
```

## posthook

在准确的时间或按照定义的工作流程回电。

```
https://posthook.io/
```

## Netlicensing

简化许可证激活。

```
https://netlicensing.io/
```

## asciinema

终端录制。

```
https://asciinema.org/
```

## HELPlightning

提高首次修复率，扩展员工能力并提高客户满意度。

```
https://helplightning.com/
```

## Scrubit

获得准确的手术设置。

```
https://www.scrubit.com/
```

## HuBoard

GitHub项目的即时项目管理。

```
https://huboard.com/
```

## Cloudcraft

可视化云架构。

```
https://www.cloudcraft.co/
```

# 参考教程

## Glype Proxy搭建和安装

```
https://coderschool.cn/2491.html
```

## You2PHP

```
https://you2php.github.io/doc/
```

## 少凡的独立博客

```
http://shaofan.org/
```

## 如有乐享

```
https://51.ruyo.net/
```

## ACfox Blog

```
https://fbk.ink/
```

## 在校师生福利：Apple、微软、Adobe 等产品如何通过教育优惠购买

```
https://sspai.com/post/39430
```

## qinghuaiorg/free-for-dev-zh: 国内免费服务聚合

```
https://github.com/qinghuaiorg/free-for-dev-zh
```

## ripienaar/free-for-dev: A list of SaaS, PaaS and IaaS offerings that have free tiers of interest to devops and infradev

```
https://github.com/ripienaar/free-for-dev
```

## AchoArnold/discount-for-student-dev: This is list of discounts on software (SaaS, PaaS, IaaS, etc.) and other offerings for developers who are students

```
https://github.com/AchoArnold/discount-for-student-dev
```

## FreeForStudents: Exclusive Freebies and Discounts

```
https://www.freeforstudents.org/
```

## ivmm/Student-resources: 本文介绍的是利用学生、教职工身份可以享受到的相关学生优惠、教育优惠或教师优惠的权益，但也希望各位享受权利的同时不要忘记自己的义务，不要售卖、转手自己的学生优惠、教育优惠的资格，使得其他同学无法受益。

```
https://github.com/ivmm/Student-resources
```

## vincenthou/free-for-china-dev: 受free-for-dev启发，用于收集国内SaaS, PaaS 和 IaaS 免费或者低付费服务，欢迎pull request

```
https://github.com/vincenthou/free-for-china-dev
```

## 一些免费的云资源

```
https://gist.github.com/imba-tjd/d73258f0817255dbe77d64d40d985e76
```

## CentOS/Debian/Ubuntu网络重装系统一键脚本

```
https://51.ruyo.net/5561.html
```

## 免费DNS推荐 （国外篇）

```
https://51.ruyo.net/609.html
```

## 如何从微软官方直接下载Windows 10的ISO镜像

```
https://51.ruyo.net/10149.html
```

## Google搜索实用技巧：帮你解除谷歌搜图安全搜索限制

```
https://51.ruyo.net/748.html
```

## Fikker网站缓存/反向代理/自建CDN软件

```
https://51.ruyo.net/7349.html
```

## 一键全自动DD安装Windows系统

```
https://51.ruyo.net/6333.html
```

## Windows一键重装系统为Linux (netboot,网络安装)

```
https://51.ruyo.net/10538.html
```

## Chrome安装离线扩展程序和非WebStore扩展程序方法

```
https://51.ruyo.net/10499.html
```

## 推荐几款比较顺眼的XShell配色方案

```
https://51.ruyo.net/6304.html
```

## Xshell/Xftp/XshellPlus 5和6版本官方下载+”注册码”

```
https://51.ruyo.net/11447.html
```

## EditPlus注册码在线生成，附开源算法代码

```
https://51.ruyo.net/4780.html
```

## 迈克菲（McAfee）杀毒软件无限叠加时间使用

```
https://51.ruyo.net/3945.html
```

## Paypal优惠券/现金券的使用（套现）方法

```
https://51.ruyo.net/6636.html
```

## 有哪些优秀且免费的云存储服务？

```
https://www.zhihu.com/question/51309695
```

## Listing the best services for free domain email addresses in 2020

```
https://dev.to/doobled/listing-the-best-services-for-free-domain-email-addresses-in-2020-3hkb
```

## Github上10个开源免费且优秀的后台控制面板

```
https://www.jianshu.com/p/3bc7404af887
```

## 国内外公共CDN静态资源网站大全

```
https://www.awaimai.com/327.html
```

## 国外免费BT离线下载服务收集

```
https://51.ruyo.net/4140.html
```

## 国内外提供免费的域名DNS解析的服务商

```
https://51.ruyo.net/13274.html
```

## 利用Github Action刷Microsoft 365 E5开发者订阅API实现续订

```
https://51.ruyo.net/15646.html
https://www.bilibili.com/video/av95688306/
```

## hoochanlon/w3-goto-world

```
https://github.com/hoochanlon/w3-goto-world
```