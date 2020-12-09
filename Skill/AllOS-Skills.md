# 系统技巧

## 电脑与手机/平板共享文件

手机或平板与电脑需要链接同一个Wi-Fi，但不能连接同一个公共Wi-Fi，如校园网，也不能一方开热点，另一方连接该热点。

### 电脑端配置

#### Windows

找到想要共享的文件夹，右键点击属性，切换到共享标签栏下，点击共享。点击下拉图标，在下拉选项中选择Everyone，点击共享按钮返

进入网络和共享中心，开启`启用网络发现`并勾选启用网络连接设备的自动设置，然后选择`关闭密码保护共享`。然后进入程序与功能，点击启用或关闭Windows功能，开启`SMB 1.0/CIFS文件共享支持`。

若需要取消共享，则在文件夹的共享页点击高级共享，并取消勾选`共享此文件夹`即可。

#### Mac

打开系统偏好设置，点击共享—勾选文件共享，添加要共享的文件夹，点击选项，开启共享。

### 手机端配置

可使用nPlayer、OPlayer、VLC等。进入视频APP并选择扫描互联网/本地网络等选项，进入共享的电脑即可。

也可通过Documents等文档APP连接共享的电脑。进入文档APP并添加SMB服务器即可。


## 主题美化

### 启动器

```
http://sao.gpbeta.com/
```

### 桌面

#### 全平台

```
http://www.huoying666.com/
https://www.rainmeter.net/
```

#### Windows

```
http://www.apumiao.com/
```

### 指针

```
http://www.cursors-4u.com/
https://zhutix.com/tag/cursors/
```

### 图标

```
https://zhutix.com/tag/tubiao/
```

## 浏览器用户代理字符串

### IE

| 平台          | 浏览器 | 用户代理字符串                                               |
| ------------- | ------ | ------------------------------------------------------------ |
| Windows       | IE 9   | Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0) |
| Windows       | IE 10  | Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0) |
| Windows       | IE 11  | Mozilla/5.0 (Windows NT 6.3; Trident/7.0; rv:11.0) like Gecko |
| Windows Phone | IE 10  | Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 920) |

### Firefox

| 平台    | 浏览器         | 用户代理字符串                                               |
| ------- | -------------- | ------------------------------------------------------------ |
| Windows | Firefox        | Mozilla/5.0 (Windows NT 6.1; WOW64; rv:6.0.2) Gecko/20100101 Firefox/6.0.2 |
| Android | Firefox Mobile | Mozilla/5.0 (Android; Mobile; rv:18.0) Gecko/18.0 Firefox/18.0 |

### Opera

| 平台    | 浏览器                | 用户代理字符串                                               |
| ------- | --------------------- | ------------------------------------------------------------ |
| Windows | Opera 12及以前        | Opera/9.80 (Windows NT 6.1; WOW64) Presto/2.12.388 Version/12.12 |
| Android | Opera Mobile 12及以前 | Opera/9.80 (Android 2.3.4; Linux; Opera Mobi/ADR-1301071546) Presto/2.11.355 Version/12.10 |
| Windows | Opera 14及以后        | Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/28.0 Safari/537.36 OPR/15.0 |
| Android | Opera Mobile 14及以后 | Mozilla/5.0 (Linux; Android 4.1.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0 Mobile Safari/537.36 OPR/16.0 |

### Chrome

| 平台           | 浏览器             | 用户代理字符串                                               |
| -------------- | ------------------ | ------------------------------------------------------------ |
| Windows        | Chrome             | Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.17 (KHTML, like Gecko) Chrome/24.0.1312.57 Safari/537.17 |
| Android Phone  | Chrome for Android | Mozilla/5.0 (Linux; Android 4.0.4; Galaxy Nexus Build/IMM76B) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.133 Mobile Safari/535.19 |
| Android Tablet | Chrome for Android | Mozilla/5.0 (Linux; Android 4.2; Nexus 7 Build/JOP40C) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.166 Safari/535.19 |
| Chrome OS      | Chrome             | Mozilla/5.0 (X11; CrOS armv7l 3428.193.0) AppleWebKit/537.22 (KHTML, like Gecko) Chrome/25.0.1364.126 Safari/537.22 |
| iPhone         | Chrome for iOS     | Mozilla/5.0 (iPhone; CPU iPhone OS 6_0_2 like Mac OS X; en-us) AppleWebKit/536.26 (KHTML, like Gecko) CriOS/23.0.1271.100 Mobile/10A551 Safari/8536.25 |
| iPad           | Chrome for iOS     | Mozilla/5.0 (iPad; CPU OS 5_1_1 like Mac OS X; zh-cn) AppleWebKit/534.46 (KHTML, like Gecko) CriOS/23.0.1271.100 Mobile/9B206 Safari/7534.48.3 |

### Safari

#### 用户代理字符串

| 平台    | 浏览器            | 用户代理字符串                                               |
| ------- | ----------------- | ------------------------------------------------------------ |
| Windows | Safari            | Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2 |
| Mac     | Safari            | Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_3) AppleWebKit/534.53.11 (KHTML, like Gecko) Version/5.1.3 Safari/534.53.10 |
| iPhone  | Safari            | Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_0 like Mac OS X; en-us) AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8A293 Safari/6531.22.7 |
| iPad    | Safari            | Mozilla/5.0 (iPad; CPU OS 5_1_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9B206 Safari/7534.48.3 |
| Android | Android自带浏览器 | Mozilla/5.0 (Linux; U; Android 2.3.4; zh-cn; SonyEricssonMT15i Build/4.0.2.A.0.62) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1 |
|         | Android自带浏览器 | Mozilla/5.0 (Linux; U; Android 4.1.1; zh-cn; M040 Build/JRO03H) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30 |

#### iOS自带Safari版本

| ioS版本 | Safari版本 |
| ------- | ---------- |
| 1.0     | 3.0        |
| 2.0     | 3.1        |
| 3.0     | 4.0        |
| 4.0     | 4.0        |
| 4.3     | 5.0        |
| 5.0     | 5.1        |
| 6.0     | 6.0        |

### 国内浏览器

| 平台    | 浏览器                    | 用户代理字符串                                               |
| ------- | ------------------------- | ------------------------------------------------------------ |
| Windows | 傲游桌面浏览器            | Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN) AppleWebKit/533.9 (KHTML, like Gecko) Maxthon/3.0 Safari/533.9 |
| Android | 傲游移动浏览器            | Mozilla/5.0 (Linux; U; Android 2.3.4; zh-cn; MT15i Build/4.0.2.A.0.62) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1 Maxthon/4.0.3.3000 |
| Windows | 搜狗浏览器                | Mozilla/5.0 (Windows NT 6.1) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.802.30 Safari/535.1 SE 2.X MetaSr 1.0 |
| Windows | 百度桌面浏览器            | Mozilla/5.0 (Windows NT 5.1) AppleWebKit/534.7 (KHTML, like Gecko) Safari/534.7 Chrome/7.0 baidubrowser/1.x |
| Android | 百度移动浏览器            | Mozilla/5.0 (Linux; U; Android 2.3.4; zh-cn; MT15i Build/4.0.2.A.0.62) AppleWebKit/533.1 (KHTML, like Gecko) FlyFlow/2.4 Version/4.0 Mobile Safari/533.1 baidubrowser/042_1.8.4.2_diordna_458_084/nosscirE-ynoS_01_4.3.2_i51TM/1000464b/174FA38EF54F67DF0EBC472658BA862B%7C101931900307210/1 |
| Windows | 百度移动浏览器            | Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN) AppleWebKit/531.1 (KHTML, like Gecko) FlyFlow/2.4 Version/5.0 Safari/531.1 baidubrowser/042_1.8.4.2_diordna_458_084/nosscirE-ynoS_01_4.3.2_i51TM/1000464b/174FA38EF54F67DF0EBC472658BA862B%7C101931900307210/1 |
| Windows | 360极速浏览器             | Mozilla/5.0 (Windows NT 5.1) AppleWebKit/535.1 (KHTML, like Gecko) Safari/535.1 Chrome/14.0.835.202 360EE |
| Android | 360移动浏览器             | Mozilla/5.0 (Linux; U; Android 2.3.4; zh-cn; MT15i Build/4.0.2.A.0.62) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1; 360browser(securitypay,securityinstalled); 360 Aphone Browser (3.2.1) |
| Windows | QQ桌面浏览器              | Mozilla/5.0 (Windows NT 6.1) AppleWebKit/534.36 (KHTML, like Gecko) Chrome/12.0.742.53 Safari/534.36 QQBrowser/6.5.9225.201 |
|         | QQ 移动浏览器（中转浏览） | MQQBrowser/2.9/Adr (Linux; U; 2.3.4; zh-cn; MT15i Build/4.0.2.A.0.62;480*854) |
| Android | QQ 移动浏览器（直接浏览） | MQQBrowser/2.9/Mozilla/5.0 (Linux; U; Android 2.3.4; zh-cn; MT15i Build/4.0.2.A.0.62) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1 |
| iOS     | QQ 移动浏览器（直接浏览） | Mozilla/5.0 (iPad; U; CPU OS 4_3_5 like Mac OS X; zh-cn) AppleWebKit/533.17.9 (KHTML, like Gecko) MQQBrowser/3.1 Mobile/8L1 Safari/7534.48.3 |
| Android | UC浏览器                  | Mozilla/5.0 (Linux; U; Android 2.3.4; zh-cn; MT15i Build/4.0.2.A.0.62) UC AppleWebKit/530+ (KHTML, like Gecko) Mobile Safari/530 |

# 操作技巧

## 搜索引擎语句

以下命令可以组合使用，中间空一格即可。

### 指定站内搜索

```
[关键词] site:[指定站点]
```

### 指定文件类型搜索

```
[关键词] filetype:[文件类型]
```

### 关键词完全匹配

```
"[关键词]"
```

### 排除关键词

```
[关键词] -[要排除的关键词]
```

### 

```
[关键词] site:[指定站点]
```

### 

```
[关键词] site:[指定站点]
```

### 

```
[关键词] site:[指定站点]
```

## 资源搜索

### 通过网站

```
http://bt.btdad.in/
https://www.xiaokesoso.com/
https://jubt.net/cn/index.html
https://biedian.me/
http://www.aiosearch.com/
http://kisssub.org/
https://www.torrentkitty.tv/search/
```

### 通过工具

#### magnetW

```
https://github.com/xiandanin/magnetW
```

## 邮箱别名

### Gmail

Gmail邮箱账号支持中间无限加点（.）、加（+）任意字符、将后缀改为`@googlemail.com`。以`tuoks.com@gmail.com`
为例，可生成以下别名。

```
tuoks.c.o.m@gmail.com
tuoks.com+ruyo.net@gmail.com
```

### improvmx

```
https://improvmx.com/
```

### 傲游百变邮箱

```
https://www.uu.me/
```

## 下载相关

### 下载被屏蔽的资源

#### 通过迅雷

##### 迅雷5

##### 迅雷会员通过hosts屏蔽

在hosts文件添加以下内容。

```
127.0.0.1 hub5btmain.sandai.net
127.0.0.1 hub5emu.sandai.net
127.0.0.1 upgrade.xl9.xunlei.com
```

完成后打开命令行并运行以下命令即可。

```
ipconfig /flushdns
```

#### 通过其他BT下载器

##### 黑科云

```
http://www.heikeyun.net/
```

##### qBittorrent

```
https://portableapps.com/apps/internet/qbittorrent_portable
```

##### uTorrent

##### 比特彗星

#### 通过国内离线下载

##### 百度云

##### 微云

#### 通过国外离线下载

教程如下。

```
https://sofun.tw/bt-download-service/
https://www.138vps.com/freeot/1299.html
```

#### 在线磁力链接转种子

```
http://www.torrent.org.cn/
http://www.btspread.cn/
http://2p1.com/
```

### 应用下载

#### 应用推荐

```
https://github.com/hzlzh/Best-App
https://github.com/jaywcjlove/awesome-mac/blob/master/README-zh.md
```

#### 查找相似应用

```
https://alternativeto.net/
```

#### 全平台应用库

```
https://www.gopojie.net/
https://www.0daydown.com/
https://www.rjsos.com/
https://www.softaculous.com/
https://www.iplaysoft.com/
http://free.apprcn.com/category/ios/
```

#### 绿色软件库

```
https://ilvruan.com/
https://www.rjno1.com/
https://www.liberkey.com/en.html
https://www.appcgn.com/
https://portableapps.com/
```

#### Mac应用库

```
https://github.com/catofmrlu/MacApps
http://www.sdifenchou.com/
https://mac-torrent-download.net/
https://cmacapps.com/
https://xclient.info/
https://www.macxin.com/
https://www.macbed.com/
https://www.macenjoy.co/
https://www.maczapp.com/
https://www.digit77.com/
https://www.macbl.com/
https://www.icheese.org/
https://www.macsky.net/
https://www.macdo.cn/
https://www.imacapp.net/
```

#### Mac付费应用优惠库

```
https://setapp.com/https://setapp.com/
https://sspai.com/mall
https://www.lizhi.io/
```

#### Windows应用库

```
http://itsoftblog.com/
https://getintopc.com/
https://github.com/Awesome-Windows/Awesome/blob/master/README-cn.md
https://amazing-apps.gitbook.io/windows-apps-that-amaze-us/zh-cn
```

#### Windows 95虚拟机

```
https://github.com/felixrieseberg/windows95/releases
```

# OCR

## 网站

```
https://ocr.wdku.net/
https://ocr.space/
http://www.gkocr.com/web/index.html
https://zhcn.109876543210.com/
http://www.ocrmaker.com/
https://www.newocr.com/
https://www.onlineocr.net/
```

## Chrome扩展

```
https://ocr.space/copyfish
http://willingstudio.com/
```

## 全平台应用

```
https://github.com/AnyListen/tools-ocr/releases
```

## Windows应用

```
https://github.com/AnyListen/tianruoocr
https://www.52pojie.cn/thread-754074-1-1.html
```

## 软件库

```
https://mooc1.chaoxing.com/zt/203917711.html
```

# 视频相关

## 视频剪辑

```
https://mooc1.chaoxing.com/zt/201533924.html
```

## 视频去水印

### 全平台

#### 快剪辑

```
http://kuai.360.cn/home.html
```

### Windows

#### 格式工厂

```
https://pan-yz.chaoxing.com/external/m/file/447904642101260288
https://pan-yz.chaoxing.com/external/m/file/447905398419349504
https://axu.lanzous.com/iQ1Cqd92vyj
```

#### Remove Logo Now!

```
https://pan-yz.chaoxing.com/external/m/file/324681592051896320
```

#### Easy Video Logo Remover

```
https://pan-yz.chaoxing.com/external/m/file/324690584870350848
```

### Android

#### 乐视频

```
https://axu.lanzous.com/ibld75c
```

## 免费视频

### 视频网站导航

```
http://www.yingmi123.com/
```

### 视频解析网站搭建

未验证。

```
https://github.com/abbeyokgo/ojbk_jiexi
```

### 免费电视直播源

下载以下m3u8文件，导入到能播放流媒体的软件即可。iOS可导入到nPlayer。

```
https://iptv-org.github.io/iptv/index.m3u
```

其余仓库地址如下。

```
https://github.com/iptv-org/iptv
https://github.com/m3u8playlist/dp
https://github.com/nzw9314/iptv
https://github.com/iptv-org/iptv
https://github.com/meishixiu/mkPlayer
https://github.com/EvilCult/iptv-m3u-maker
```

### 免费Netfilx/国外电视

```
https://tecknity.com/
https://www.mycookiesfree.com/
https://www.alliance4creativity.com/where-to-watch/
```

### 免费电视直播

```
http://www.daoiqi.com/iptv6.html
http://ivi.bupt.edu.cn/
https://itv.ahcme.edu.cn/
```

### 免费视频网站

```
https://tv.syrme.top/
http://www.yhdm.io/
https://www.cnwml.com/
https://www.piaohua.com/
http://neets.cc/
http://www.youzhidy.com/
http://www.zhuixinfan.com/main.php
https://www.dy2018.com/
http://lcoc.top/vip2.3/
http://lcoc.top/2.2/
http://www.7meiju.com/
https://ddrk.me/
https://www.lol5s.com/
https://www.dandanzan.com/
https://www.yunbtv.com/
https://www.agefans.tv/
https://www.ifvod.tv/
https://qinmei.video/
http://www.mvcat.com/
http://www.qnvod.net/
https://www.hktvyb.com/
https://53ys.cc/
https://www.duboku.co/
https://www.pianku.tv/
https://www.nfmovies.com/
https://www.dililitv.com/
http://www.kkkkwo.com/
https://ddrk.me/
https://vip.kkroot.com/
http://lab.liumingye.cn/
https://kast.gg/
```

#### 剧集

```
https://www.meijutt.tv/
https://www.novipnoad.com/
http://www.meijuniao.com/
http://www.wwmulu.com/
http://www.zzrbl.com/
http://www.yueyuwu.com/
http://www.ttzmz.vip/
http://91mjw.com/
```

#### 动漫

```
http://www.miobt.com/
http://www.kisssub.org/
https://bangumi.moe/
https://mikanani.me/
https://www.dongmanhuayuan.com/
https://dongmanhuayuan.myheartsite.com/
https://share.dmhy.org/
http://www.dmhy.org/
https://nyaa.si/
https://share.acgnx.se/
```

### 视频解析源

在`=`后面跟视频链接即可。

```
http://jx.618g.com/?url=
https://jiexi.071811.cc/jx.php?url=
http://tv.wandhi.com/go.html?url=
http://www.zhmdy.top/index.php?zhm_jx=
https://www.sonimei.cn/?url=
https://660e.com/?url=
http://q.z.vip.totv.72du.com/?url=
http://u2h.cn/vip.php?url=
```

以下未验证。

```
http://v.d9y.net/vip/?url=
http://jx.du2.cc/?url=
http://api.bbbbbb.me/jx/?url=
https://api.bbbbbb.me/yun/?url=
https://api.bbbbbb.me/vip/?url=
https://api.bbbbbb.me/yunjx/?url=
https://api.bbbbbb.me/sg/?url=
http://api.51ckm.com/jx.php?url=
http://api.hlglwl.com/jx.php?url=
http://www.82190555.com/index.php?url=
http://app.baiyug.cn:2019/vip/?url=
http://api2.club/index.php?url=
http://all.baiyug.cn:2021/QQB_kandianm/kandians_api.php?url=
http://app.baiyug.cn:2019/vip/player.php?vid=
http://69p.top/?url=
http://74t.top/?url=
http://mimijiexi.top/?url=
http://55jx.top/?url=
http://playx.top/?url=
http://nitian9.com/?url=
http://19g.top/?url=
http://607p.com/?url=
http://52088.online/?url=
http://bofang.online/?url=
http://play1.online/?url=
http://ckplay.online/?url=
http://api.baiyug.vip/?url=
http://880kan.com/?url=
http://59uv.com/?url=
http://api.smq1.com/?url=
https://api.smq1.com/?url=
https://jx.hezeshi.net/ce/jlexi.php?url=
http://api.hellosex.cc/jlexi.php?url=
https://api.pangujiexi.com/player.php?url=
http://at520.cn/jx/?url=
http://www.cmys.tv/?url=
https://jx.km58.top/jx/?url=
http://www.fantee.net/fantee/?url=
http://api.51ckm.com/Box.php?url=
https://www.3aym.cn/?url=
http://beaacc.com/api.php?url=
http://api.zuilingxian.com/jiexi.php?url=
http://jx.2tv.org/?url=
http://api.bbbbbb.me/ipsign/player.php?v=
http://17kyun.com/api.php?url=
https://jx.hezeshi.net/ce/jlexi.php?url=
http://api.hellosex.cc/jlexi.php?url=
https://api.pangujiexi.com/player.php?url=
http://at520.cn/jx/?url=
http://player.jidiaose.com/supapi/iframe.php?v=
https://jx.okokjx.com/okok/?url=
http://api.jykkk.com/?url=
https://jx.km58.top/jx/?url=
http://jx.fantee.net/fantee/?url=
http://api.51ckm.com/Box.php?url=
http://www.1717yun.com/jx/ty.php?url=
https://www.3aym.cn/?url=
http://beaacc.com/api.php?url=
http://api.zuilingxian.com/jiexi.php?url=
http://jx.2tv.org/?url=
http://cdn.yangju.vip/k/?url=
http://api.bbbbbb.me/ipsign/player.php?v=
http://17kyun.com/api.php?url=
http://www.iwkan.cn/jx2/?url=
http://api.ledboke.com/?url=
http://ckparse.kaizhoukm.com/play/vippcq.php?url=
http://jqaaa.com/jx.php?url=
http://api.47ks.com/webcloud/?v=
http://api.xfsub.com/index.php?url=
http://jx.du2.cc/jx6.php?url=
http://jx.du2.cc/?url=
http://lykezhan.com/vip/
http://yy.zhiliaotang.com/vip/
http://www.5ifxw.com/vip/
http://www.qmaile.com
http://tv.myhack58.com
http://www.51onb.com/filmvip/
https://api.pangujiexi.com/player.php?url=
http://api.ledboke.com/vip/?url=
http://vip.qike.ink/?url=
http://jx.598110.com/index.php?url=
http://player.jidiaose.com/supapi/iframe.php?v=
http://api.91exp.com/svip/?url=
http://v.72du.com/api/?url=
http://api.bbbbbb.me/ipsign/player.php?v=
http://17kyun.com/api.php?url=
http://www.662820.com/xnflv/index.php?url=
http://api.lldyy.net/svip/?url=
http://www.82190555.com/index/qqvod.php?url=
http://jiexi.92fz.cn/player/vip.php?url=
http://jiexi.071811.cc/jx2.php?url=
http://api.wlzhan.com/sudu/?url=
http://beaacc.com/api.php?url=
http://qxkkk.bid/jx/jx.php?url=
http://www.27v9.cn/index.php?url=
http://www.ckplayer.tv/kuku/?url=
http://o8ve.cn/1/?url=
http://api.xyingyu.com/?url=
https://jx.kt111.top/jx/mf/?url=
https://api.pangujiexi.com/player.php?url=
http://api.lvcha2017.cn/?url=
http://kkk.2016ve.cn/kkjx/index.php?url=
http://mlxztz.com/vip/?url=
http://www.aktv.men/?url=
http://jy777.cn/XSD/XSD/?url=
http://api.visaok.net/?url=
http://api.xyingyu.com/?url=
http://api.greatchina56.com/?url=
http://jx.618g.com/?url=
http://api.baiyug.vip/index.php?url=
http://jx.jfysz.cn/jx.php/?url=
http://jx.ektao.cn/jx.php/?url=
http://jx.reclose.cn/jx.php/?url=
http://jx.eayn.org.cn/jx.php/?url=
http://api.xyingyu.com/?url=
http://jx.iaeec.cn/jx.php/?url=
http://jx.83y4n7a.cn/jx.php/?url=
http://jx.cmbzzs.cn/jx.php/?url=
http://api.greatchina56.com/?url=
http://jx.as19811.cn/jx.php/?url=
http://jx.sdjnd09.cn/jx.php/?url=
http://api.baiyug.vip/index.php?url=
http://jx.09876as.cn/jx.php/?url=
http://jx.17ktv.com.cn/jx.php/?url=
http://jx.ab78a.cn/jx.php/?url=
http://jx.09877as.cn/jx.php/?url=
http://jx.yipolo111.cn/jx.php/?url=
http://jx.908astbb.cn/jx.php/?url=
http://jx.dlzyrk001.cn/jx.php/?url=
http://jx.dccmy.org.cn/jx.php/?url=
http://jx.boctx.cn/jx.php/?url=
http://jx.hxbte.cn/jx.php/?url=
http://api.visaok.net/?url=
http://jx.618g.com/?url=
http://yun.baiyug.cn/vip/?url=
http://api.baiyug.cn/vip/?url=
https://api.flvsp.com/?url=
http://api.xfsub.com/index.php?url=
http://jiexi.071811.cc/jx2.php?url=
http://player.jidiaose.com/supapi/iframe.php?v=
http://www.82190555.com/index/qqvod.php?url=
http://api.pucms.com/?url=
http://api.baiyug.cn/vip/index.php?url=
https://api.flvsp.com/?url=
http://2gty.com/apiurl/yun.php?url=
http://v.2gty.com/apiurl/yun.php?url=
http://api.visaok.net/?url=
http://api.xyingyu.com/?url=
http://api.greatchina56.com/?url=
http://jx.618g.com/?url=
http://api.xyingyu.com/?url=
http://api.greatchina56.com/?url=
http://api.visaok.net/?url=
http://jx.618g.com/?url=
http://zuiai.ml/dh/jx/ajx.php?url=
http://zuiai.ml/dh/jx/jx.php?url=
http://zuiai.ml/dh/jx/jiexi.php?url=
http://player.jidiaose.com/supapi/iframe.php?v=
http://api.xfsub.com/index.php?url=
https://jx.okokjx.com/okokjx/?url=
http://api.47ks.com/webcloud/?v=
http://wwwhe1.177kdy.cn/4.php?pass=2&url=
https://api.daidaitv.com/index/?url=
http://jx.aeidu.cn/index.php?url=
http://api.baiyug.cn/vip/?url=
http://api.baiyug.cn/vip/index.php?url=
http://www.82190555.com/index/qqvod.php?url=
http://17kyun.com/api.php?url=
http://jx.biaoge.tv/index.php?url=
http://www.85105052.com/admin.php?url=
http://api.iy11.cn/apiget.php?url=
http://api.baiyug.cn/vip/index.php?url=
http://jx.hanximeng.com/m3u8.php?url=
http://jx.39book.com/Client/apiget.php?url=
http://014670.cn/jx/ty.php?url=
http://www.ibb6.com/x1/?url=
http://tv.x-99.cn/api/wnapi.php?id=
http://www.52rjb.cn/vip1/?url=
http://www.52rjb.cn/vip2/?url=
http://www.52rjb.cn/vip3/?url=
http://49.4.144.33/xfjx/1.php?url=
http://7cyd.com/vip/?url=
https://47ksvip.duapp.com/vip/2mm/?vid=
http://jx.vgoodapi.com/jx.php?url=
http://api.wlzhan.com/sudu/?url=
http://vip.72du.com/api/?url=
http://api.nepian.com/ckparse/?url=
http://api.91exp.com/svip/?url=
http://qtzr.net/s/?qt=
http://www.chepeijian.cn/jiexi/vip.php?url=
http://www.wmxz.wang/video.php?url=
http://api.baiyug.cn/vip/index.php?url=
http://player.jidiaose.com/supapi/iframe.php?v=
http://api.pucms.com/index.php?url=
1http://jiexi.92fz.cn/player/vip.php?url=
http://lookxw.com/yingzi/?url=
http://api.97kn.com/?url=
http://api.baiyug.cn/vip/index.php?url=
http://jx.ejiafarm.com/dy.php?url=
http://www.vipjiexi.com/yun.php?url=
http://api.1008net.com/v.php?url=
http://api.nepian.com/ckparse/?url=
http://player.jidiaose.com/supapi/iframe.php?v=
http://api.pucms.com/index.php?url=
http://api.wlzhan.com/sudu/?url=
http://jiexi.yanjiaozhaopinwang.com/jx1/dy.php?url=
http://www.0335haibo.com/yun.php?url=
http://yun.mt2t.com/yun?url=
http://y.mt2t.com/lines?url=
http://www.0335haibo.com/yun.php?url=
http://www.sfsft.com/video.php?url=
http://www.82190555.com/video.php?url=
http://2.jx.72du.com/video.php?url=
http://www.97panda.com/kkflv/index.php?url=
http://jx.vgoodapi.com/jx.php?url=
http://www.dgua.xyz/webcloud/?url=
http://api.91exp.com/svip/?url=
http://vip.jlsprh.com/index.php?url=
http://api.xfsub.com/index.php?url=
http://jiexi.071811.cc/jx.php?url=
http://jiexi.92fz.cn/player/vip.php?url=
http://api.baiyug.cn/vip/index.php?url=
http://api.662820.com/xnflv/index.php?url=
http://www.82190555.com/index/qqvod.php?url=
http://yyygwz.com/index.php?url=
http://www.97panda.com/kkflv/index.php?url=
http://jx.api.163ren.com/vod.php?url=
http://lookxw.com/yingzi/?url=
http://www.0335haibo.com/tong.php?url=
http://www.wmxz.wang/video.php?url=
http://api.97kn.com/?url=
https://aikanapi.duapp.com/odflv105/index.php?url=
http://api.lequgirl.com/?url=
https://jxapi.nepian.com/ckparse/?url=
http://91k.co/play.php?url=
http://www.52jiexi.com/yun.php?url=
http://mt2t.com/yun?url=
http://000o.cc/jx/ty.php?url=
http://yyygwz.com/index.php?url=
http://vipjiexi.com/vip.php?url=”
http://www.wmxz.wang/video.php?url=
http://jx.ck921.com/?url=
http://s1y2.com/?url=
http://www.ou522.cn/t2/1.php?url=
http://jx.xuanpianwang.com/parse?url=
http://jiexi.92fz.cn/player/vip.php?url=
http://api.taoge.la/jiexi/index.php?url=
http://vip.jlsprh.com/index.php?url=
http://api.baiyug.cn/vip/index.php?url=
http://jiexi.92fz.cn/player/vip.php?url=
http://api.nepian.com/ckparse/?url=
http://aikan-tv.com/?url=
http://j.zz22x.com/jx/?url=
http://www.efunfilm.com/yunparse/index.php?url=
https://api.flvsp.com/?url=
http://api.xfsub.com/index.php?url=
http://api.47ks.com/webcloud/?v=
http://player.jidiaose.com/supapi/iframe.php?v=
http://65yw.2m.vc/chaojikan.php?url=
http://lookxw.com/yingzi/?url=
http://api.wlzhan.com/sudu/?url=
http://jx.api.163ren.com/vod.php?url=
http://dingdang-tv.com/jx?url=
https://api.flvsp.com/?url=
http://www.viyun.me/jiexi.php?url=
http://jiexi.92fz.cn/player/vip.php?url=
http://api.baiyug.cn/vip/index.php?url=
http://www.wmxz.wang/video.php?url=
http://2.jx.72du.com/video.php?url=
http://www.vipjiexi.com/yun.php?url=
http://api.cloudparse.com/?url=
http://yyygwz.com/index.php?url=
http://000o.cc/jx/ty.php?url=
http://5qiyi.sdyhy.cn/5qiyi/5qiyi1.php?url=
http://5qiyi.sdyhy.cn/5qiyi/5qiyi2.php?url=
http://api.47ks.com/webcloud/?v=
http://jx.71ki.com/?url=
http://yun.mt2t.com/yun?url=
http://jx.haokan5.net/ty/tong.php?url=
http://jx.haokan5.net/jx/?url=
http://player.gakui.top/?url=
http://qtzr.net/s/?qt=
http://api.ccoyo.cc/jiexi.php?url=
http://v.kandian.info/jiexi.php?url=
http://yyygwz.com/index.php?url=
http://apn.zhibo99.cn/mdparse/letv.php?id=
https://api.47ks.com/webcloud/?v=
http://api.mp4la.net/?url=
http://v.72du.com/api/?url=
http://api.47ks.com/webcloud/?v=
http://jx.71ki.com/index.php?url=
http://000o.cc/jx/ty.php?url=
http://www.yydy8.com/Common/?url=
http://yun.mt2t.com/yun?url=
http://api.mp4la.net/?url=
http://yyygwz.com/index.php?url=
http://v.72du.com/api/?url=
http://api.47ks.com/webcloud/?v=
http://jx.71ki.com/index.php?url=
http://www.97zxkan.com/jiexi/97zxkanapi.php?url=
http://www.wmxz.wang/video.php?url=
http://www.bbshanxiucao.top/video.php?url=
http://www.xiguaso.com/api/index.php?url=
http://www.kppev.cn/jiexi/5/1/1.php?url=
http://api.mp4la.net/?url=
http://v.72du.com/api/?url=
http://api.47ks.com/webcloud/?v=
http://jx.71ki.com/index.php?url=
http://000o.cc/jx/ty.php?url=
http://5qiyi.sdyhy.cn/5qiyi/5qiyi2.php?url=
http://vip.sdyhy.cn/ckflv/?url=
http://player.gakui.top/?url=
http://qtzr.net/s/?qt=
http://www.vipjiexi.com/yun.php?url=
https://apiv.ga/magnet/
http://api.baiyug.cn/vip/index.php?url=
http://977345961.kezi.wang/ykyun/c.php?vid=
```

### 视频解析网站

```
https://www.urlgot.com/
https://www.clipconverter.cc/2/
https://www.parsevideo.com/
```

### Chrome视频解析插件

```
http://vip.ufanw.com/
```

### ZY Player资源播放器

```
https://github.com/Hunlongyu/ZY-Player/releases
```

### iOS应用

#### sPlayer

可用于查看电视直播。下载以下直播源后在sPlayer打开即可。

```
https://wwa.lanzous.com/inL9feto6jc
```

#### 蚂蚁书签

在App Store上可以下载。下载完成后打开以下链接，点击`点我恢复备份`，在蚂蚁书签中打开，即可导入主流视频网站及解析线路。

```
http://mvipsp.com/20190919234346575.html
```

点击任意一个网站，右下角将出现两个图标。播放按钮可以无广告观看，适用于观看免费视频，放大镜按钮则是解析观看，一键探测播放线路，适用于观看会员视频。打开想看的视频，点击软件右下角的放大镜按钮后会弹出线路探测界面，探测完成后，点击播放按钮即可在线观看。

可以手动添加视频网站，回到软件主页，点击右上角的+号 ，输入名称和网址之后点击保存即可。也可手动添加解析线路，点击`功能设置`，选择`书签探索`后添加即可。

#### 视频解析脚本

适用于Alook和iHTTP浏览器。打开以下链接并下载，导入alook文件到Alook浏览器或txt文件到iHTTP浏览器即可，访问视频网站时会自动显示解析源。

```
https://share.weiyun.com/5iQW2n1
```

#### 花样影视

```
https://i.hyys.me
```

### Windows应用

#### 万里影视

```
https://www.lanzous.com/i9svnri
```

#### 电影搜索

```
https://www.lanzous.com/i8gdtub
```

#### You-Get

免费下载Youtube视频。

```
https://github.com/soimort/you-get
```

#### 荐片播放器

```
https://www.jianpian6.com/
```

## 视频下载

### 普通视频

#### 通过网站

```
https://zooqle.com/
https://www.tzfile.com/
https://btdb.io/
http://www.miobt.com/
http://www.comicat.org/
https://www.dytt8.net/index.htm
https://www.dy2018.com/
https://www.ygdy8.com/index.html
http://dbfansub.com/category/tvshow/baskets/
https://www.amoyshare.com/free-video-downloader/
https://zh.savefrom.net/7/
https://www.videofk.com/
http://www.kupian.cc/
https://zh.pickfrom.net/pickvideo
https://www.videograbber.net/zh/
http://weibodang.cn/
http://www.flvcd.com/
http://www.parsevideo.com/
https://www.cnblogs.com/iMath/p/LYYDownloader.html
```

#### 通过解析接口

```
http://www.ytb.io/
https://en.savefrom.net/17/
https://www.clipconverter.cc/2/
https://www.converto.io/en2
https://bitdownloader.com/en2
https://toutiao.iiilab.com/
http://www.shipinyu.com/
http://www.downfi.com/video/
```

#### 通过工具

```
https://evilcult.github.io/moviecatcher/index.html
https://github.com/EvilCult/Video-Downloader
https://github.com/ingbyr/VDM
http://www.vidown.cn/

```

#### 通过chrome插件

```
https://chrome.google.com/webstore/detail/flash-video-downloader/aiimdkdngfcipjohbjenkahhlhccpdbc
https://github.com/ovear/Adkill-and-Media-Download
```

### B站

```
http://www.jijidown.com/
https://www.kanbilibili.com/
http://www.ibilibili.com/
```

### Twitter

```
https://twdown.net/
```

### Instragram

```
https://www.w3toys.com/
```

# 软件技巧

## 浏览器阅读模式

### 全平台

#### 简悦

```
https://simpread.pro/
```

## B站观看港澳台番剧

通过哔哩哔哩UWP版或油猴脚本上搜索`解除b站区域限制`。

## 浏览器去广告

### 电脑端浏览器插件

```
https://ublock.org/
https://adguard.com/zh_cn/adguard-browser-extension/overview.html
https://adblockplus.org/zh_CN/download
https://www.adtchrome.com/
```

### 电脑端软件

#### Adsafe

v5.3下载链接如下。

```
https://axu.lanzous.com/icbob6j
```

#### 其余软件

```
http://www.admflt.com/
http://www.admon.cn/
```

#### 安卓端

##### 软件

```
https://adguard.com/zh_cn/adguard-android/faq.html
```

##### Xposed插件

```
https://repo.xposed.info/module/tw.fatminmin.xposed.minminguard
https://www.coolapk.com/apk/com.dahuo.sunflower.xp.none 
https://repo.xposed.info/module/com.gy.xposed.skip
```

## Chrome扩展

### uBlock

高效拦截器插件。

```
https://github.com/gorhill/uBlock
```

### 油猴插件

```
https://github.com/XIU2/UserScript
```

### 下载库

```
http://www.cnplugins.com/
https://www.crx4chrome.com/
https://www.chromefor.com/
https://173app.com/chrome-ext
https://chrome-extension-downloader.com/
http://yurl.sinaapp.com/crx.php
https://chrome.pictureknow.com/
```

### 推荐库

```
https://www.runningcheese.com/extensions
https://github.com/runningcheese/RunningCheese-Firefox/tree/master/Extensions
```

## Adobe系破解补丁

Adobe的产品可先通过Creative Cloud下载试用版。

### Windows端

可使用GenP。

### Mac端

可使用Adobe Zii破解。注意补丁需适配Adobe的版本号。

```
https://www.adobezii.com/
```

# 常用工具

## 下载工具

### Tracker

列表如下。

```
https://www.52pojie.cn/thread-1032873-1-1.html
https://github.com/XIU2/TrackersListCollection
https://trackerslist.com/#/zh
https://github.com/ngosang/trackerslist
http://github.itzmx.com/1265578519/OpenTracker/master/tracker.txt
https://newtrackon.com/list
http://www.torrenttrackerlist.com/torrent-tracker-list
https://tinytorrent.net/best-torrent-tracker-list-updated/
```

### qBittorrent

```
https://github.com/c0re100/qBittorrent-Enhanced-Edition
https://www.qbittorrent.org/
```

### IDM

在Windows XP上使用可能会造成连不上网的问题。

#### 不调用IDM下载

按住Alt键再点击下载按钮即可。

#### 断点续传

打开主界面，在所需任务上右键并点击`刷新下载地址`。获取URL后复制，在刚才的任务上右键点击属性，将地址一栏替换为刚才复制的URL，继续下载即可。

#### 设置多线程

进入设置，选择连接，连接类型/速度选择`较高速率连接`，默认最大连接数按照需求选择。建议小于2G的文件可以开32线程，大于2G的文件减少到8-16线程。

#### 插件修复

##### Chrome

打开以下链接进行安装即可。

```
https://chrome.google.com/webstore/detail/idm-integration-module/ngpampappnmepgilojfohadhhmbhlaek
```

##### Edge

打开以下链接进行安装即可。

```
ms-windows-store://review/?PFN=TonecInc.IDMIntegrationModule_e7b5mm5d3r6v2
```

#### 悬浮嗅探窗修复

首先尝试重新启用浏览器扩展，并在IDM扩展属性中勾选允许访问文件网址。

若无效，着进入IDM设置，在常规选项卡下取消勾选`使用高级浏览器集成`。进入`C:\Users\你的用户名\AppData\Roaming\IDM`，删除该目录下所有文件，重新测试悬浮窗是否可用。

也可尝试使用IE浏览器。

#### 下载直播视频

打开`C:\Users\你的用户名\AppData\Roaming\IDM\DwnlData`， 进入路径内部直至看到一份没有后缀名的文件。若要停止录制，则将文件复制一份，并添加后缀名`ts`或`mp4`即可。

### XDM

```
https://subhra74.github.io/xdm/
```

### NDM

```
http://www.neatdownloadmanager.com/index.php/en/
```

### FDM

```
https://www.freedownloadmanager.org/zh/
```

### Xdown

```
https://xdown.org/
```

### DL磁力下载

```
https://www.lanzous.com/i68544h
```

### 迅雷X

可无限循环获得试用会员加速。

```
https://www.52pojie.cn/thread-1012585-1-1.html
```

### BitComet

原版软件下载链接如下。

```
http://www.bitcomet.com/en
```

破解版和教程如下。

```
https://www.52pojie.cn/thread-1034122-1-1.html
https://www.52pojie.cn/thread-1049654-1-1.html
```

点比特彗星里上方栏位的`工具`-`电驴插件`-`显示插件窗口`，拉长弹出来的插件窗口，点击`工具`-`直接下载`，复制要下载的ed2k链接后回车即可。

# 音乐相关

## 录音软件

```
https://www.lanzoux.com/ibGF8h0cguf
```

## 音乐封面下载

```
http://coverbox.henry-hu.com/
```

## 人声分离

教程如下。

```
https://mp.weixin.qq.com/s/CtQ1yqbnAuMFbbd7KvlJFA
```

### Adobe Audition 2020

```
https://pan-yz.chaoxing.com/external/m/file/476456698956173312
```

### SpleeterGUI

```
https://pan-yz.chaoxing.com/external/m/file/476427523265245184
```

### iZotope RX 7 Audio Editor

```
https://pan-yz.chaoxing.com/external/m/file/476456201711878144
```

## 音乐编辑

### 网站

```
https://mp3cut.net/cn/
```

### 软件

#### Audacity

```
https://axu.lanzoux.com/igeNYe04dne
```

#### GoldWave

```
https://axu.lanzoux.com/iqGQGe04e1i
```

#### WavePad

```
https://axu.lanzoux.com/izccwe04dri
```

## MV下载

```
http://www.170mv.com/tool/jiexi/
```

## 音乐学习

### 歌者盟

```
https://space.bilibili.com/25872247
https://zgsing.ke.qq.com/
http://v.qq.com/s/videoplus/174248852
```

### 其余网站

```
http://vocalbalance.net/cn/
http://www.yinyuexuexi.com/index.html
```

## 免费音乐

### Chrome插件

```
https://github.com/seekerlee/SoundPirate
```

### 音乐网站

```
http://music.ifkdy.com/
https://music.hwkxk.cn/
http://tool.liumingye.cn/music/
http://www.lxh5068.com/music/
http://tool.wotula.com/t/web/music-master/
http://www.xieqian.vip/music/
http://tongzhong.xyz/
https://netease-music.fe-mm.com/#/music/playlist
https://yinyue.qugeek.com/app/player
https://www.postgraduate.top/app.php/music#/music/playlist
http://tool.yiixue.com/Tools/mk_music/
http://ayy.ayxhk.com/
http://vip.xcsee.cn/music/
https://myfreemp3cc.com
http://tool.liumingye.cn/music/?page=homePage
http://www.guqiankun.com/tools/music/
http://music.moresound.tk/
http://tool.yijingying.com/musictools/
http://music.ziyuandi.cn/
http://music.moresound.tk/
https://www.jamendo.com/start
https://mixkit.co/free-stock-music/
https://www.bensound.com/
http://beemp3s.org/
https://soundcloudmp3.org/zh
http://www.51ape.com/
```

### 音乐伴奏

#### 通过网站

```
http://soubanzou.com/
http://5sing.kugou.com/index.html
http://www.zhiqubz.com/
https://ifengui.com/
https://moises.ai/zh-cn/
https://ezstems.com/
https://thepirat000.github.io/spleeter-api/
https://melody.ml/
https://splitter.ai/
```

#### 通过软件

##### SpleeterGui

```
https://github.com/boy1dr/SpleeterGui
```

##### RX7

打开RX7，拖入要消音的音频，然后在右边的面板找到Music Rebalance，接着在弹出的面板里面把最左边的人声拉到最低，接着点击下方的Render确定修改即可。

##### Adobe Audition

启动AU，拖入音频，选中需要修改的音频，点击效果-立体声音像-中置声道提取器。选择预设中的`人声移除`，然后把右边的中心声道电平拉到最低，最后点击应用。

### 音乐软件

#### 茉莉音乐

```
https://www.zdfans.com/html/7639.html
```

#### Listen1

```
http://listen1.github.io/listen1/
```

#### 音乐搜索器

```
https://github.com/maicong/music
```

#### 洛雪音乐助手桌面版

```
https://github.com/lyswhut/lx-music-desktop
```

### 解锁网易云音乐灰色歌曲

#### Windows端

在网易云音乐的设置中设置代理即可，配置如下。

```
类型 / HTTP
地址 / xbmmw.xyz
端口 / 1001

// 以下未测试
服务器 / 60.205.213.236
端口 / 8888

IP / 103.118.42.145
端口 / 1236
```

#### iOS端

##### 通过WiFi代理

打开设置-Wi-Fi，点击正在连接的WiFi旁边的感叹号，选择配置代理。代理类型选择自动，URL填写以下链接，储存即可。

此法也适用于其他平台，包括Android、Windows和macOS。

```
https://wy.ydlrqx.com/proxy.pac
// 或
http://taron.top:100/proxy.pac
```

##### 通过Shadowrocket

用Safari打开以下链接，安装证书并信任。

```
https://raw.githubusercontent.com/nondanee/UnblockNeteaseMusic/master/ca.crt
```

打开Shadowrocket，点击`+`号，按照以下配置添加节点。

```
类型 / HTTP
地址 / xbmmw.xyz
端口 / 1001
```

下载以下配置文件并用Shadowrocket打开，选择新加入的配置。全局路由选择配置，节点选择新添加的节点，打开连接开关即可。

```
链接 / https://pan.baidu.com/s/1XSAZ4bc3hItftn_Usv6-FA
提取码 / t5tq
```

也可手动配置规则。在配置文件中按照以下类型添加规则即可。

```
类型 / USER-AGENT
选项 / 新添加的节点
用户代理 / NeteaseMusic*
```

##### 通过Quantumult

证书和节点同上。点击设置，进入分流页面，添加以下三条规则。注意，如果本来已经有相同匹配的分流规则，则先删除。

```
类型 / USER-AGENT
行为 / 新添加的节点
匹配 / NeteaseMusic*

类型 / HOST-SUFFIX
行为 / 新添加的节点
匹配 / 163.com

类型 / HOST-SUFFIX
行为 / 新添加的节点
匹配 / 126.net
```

### 自行搭建解锁网易云服务器

需要有一台服务器。以CentOS为例，连接到服务器后输入以下命令。

```
sudo su root
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
yum install nodejs git -y
git clone https://github.com/nondanee/UnblockNeteaseMusic.git
yum install screen
screen -S xxx（可以自己改）cd UnblockNeteaseMusic
node app.js -p 1234:2345 -e http://music.163.com(1234:2345)
```

端口可以自己随意更改，设置后需要在服务器放行端口。搭建完成后在客户端连接到该服务器即可。

# 话术生成

## 恋爱

```
https://www.tool22.com/Tools-LiaoMei.html
```

## 网名

```
https://www.qmsjmfb.com/
```

## 营销号

```
https://codepen.io/kasei-dis/full/JjYjwza
```

## 毒鸡汤

```
https://du.shadiao.app/
```

## 朋友圈

```
https://pyq.shadiao.app/
```

## 藏头诗

```
https://www.shicimingju.com/cangtoushi/index.html
https://cts.chazhi.net/
```

## 骂人

```
https://zuanbot.com/
https://nmsl.shadiao.app/
```

## 抽象话

```
https://cxh.papapoi.com/
```

## 彩虹屁

```
https://chp.shadiao.app/
```

## 文章

```
https://github.com/menzi11/BullshitGenerator
https://suulnnka.github.io/BullshitGenerator/index.html
```

# GIF与表情

## GIF压缩

### UleadGIFAnimator

```
https://www.lanzoux.com/iUEKYgxu9zi
```

### PP鸭

```
https://pan-yz.chaoxing.com/external/m/file/413806043510824960
```

## 表情库

```
https://emojipedia.org/
https://getemoji.com/
https://www.acfun.cn/emot/
https://www.doutula.com/
https://www.fabiaoqing.com/
http://52doutu.cn/
http://www.dbbqb.com/
https://www.diydoutu.com/diy/zhuangx
https://www.diydoutu.com/diy/gif
https://app.xuty.tk/static/app/index.html
https://imgflip.com/memetemplates
```

## 表情制作

### 举牌小人

```
http://upuptoyou.com/
http://1.hus.tcapps.twocola.com/
```

### P站logo

```
https://www.zerobbs.net/pornhub/
https://www.qingwei.tech/somehub/
https://www.logoly.pro/
https://lab.bangbang93.com/porn-hub
```

### 奥利奥

```
http://ljl.li/oreooo/
```

### 面试四格

```
http://r.ftqq.com/fangtangGif/interview/
```

### petpet

```
https://benisland.neocities.org/petpet/
```

### 口罩护目镜头像

```
https://h5.codefuture.top/2020-mask/
```

### 其余表情

```
https://www.danmu9.com/sticker
https://lab.bangbang93.com/weidao
https://lab.bangbang93.com/jichou
https://jichou.shadiao.app/
```

## GIF压缩

### 通过网站

```
https://ezgif.com/optimize
https://www.yasuotu.com/gif
```

### 通过软件

```
http://ppduck.com/
```

## GIF转换

教程如下。

```
https://mp.weixin.qq.com/s/yRTL5pJpAv_OZxOL9tykug
```

## GIF录制

```
https://www.screentogif.com/
```

## GIF处理

```
https://ezgif.com/
https://www.soogif.com/compress
```

## GIF库

```
https://giphy.com/
http://www.gifparanoia.org/
```

# PS相关

## 插件

### topaz remask 5

用于抠图。

## 教程

### 去水印

```
https://mp.weixin.qq.com/s/sJSUm68TTbCosUAWoSDWyQ
```

## 图片拼接

### 网站

```
https://xiuxiu.web.meitu.com/
```

### Windows应用

可用美图秀秀。

### Mac应用

可用Fotor。

# 数据恢复

## Windows

```
https://www.lanzoux.com/b06aavj5c
```

## Mac

```
https://www.lanzoux.com/b06aavi2d
```

## Android

```
https://www.lanzoux.com/b06aavi0b
https://www.lanzoux.com/b06aavhvg
```

## 图片艺术

```
http://weavesilk.com/
https://wangyasai.github.io/designtools.html
http://matthew.wagerfield.com/flat-surface-shader/
http://www.mazegenerator.net/
```

## 图片搜索

### 普通图片

#### 图片库

```
https://zh.moegirl.org.cn/zh-hans/Help:%E5%AF%BB%E6%89%BE%E5%8E%9F%E5%9B%BE
```

#### 通过网站

```
https://images.google.com/
https://gfsoso.xz95.top/image.html
https://yandex.com/images/
https://image.baidu.com/?fr=shitu
https://www.everypixel.com/
https://pic.sogou.com/
http://st.so.com/
https://tineye.com/
https://saucenao.com/
https://www.imageidentify.com/
```

#### 通过Chrome扩展

```
https://chrome.google.com/webstore/detail/noobox/kidibbfcblfbbafhnlanccjjdehoahep
```

### 衣服图片

```
http://paitogo.com/
```

### 鸟类图片

```
http://www.nsii.org.cn/2017/birdApp.php
```

### 动漫图片

```
https://tools.miku.ac/what_anime_is_this/
https://trace.moe/
http://3d.iqdb.org/
http://iqdb.org/
https://ascii2d.net/
```

### 被盗图片

```
https://berify.com/
```

### Reddit图片

```
http://karmadecay.com/
```

### 搜索教程

```
https://www.chongbuluo.com/thread-3071-1-1.html
```

## Pixiv相关

### XZPixivDownloader

Pixiv图片下载器。

```
https://github.com/xuejianxianzun/XZPixivDownloader
```

### Pixiv-Nginx

PIXIV网页版及客户端访问恢复。

```
https://github.com/mashirozx/Pixiv-Nginx
https://2heng.xin/2017/09/19/pixiv/
```

### P站非会员查看人气作品

```
https://github.com/chenjiandongx/pixiv
```

## 图片下载

```
https://imagecyborg.com/
```

## 图片库

### 通过网站

```
https://www.poco.cn/?classify_type=0&works_type=medal
https://imgur.com/
https://500px.com/
https://www.flickr.com/
https://tuchong.com/
https://gratisography.com/
https://www.publicdomainpictures.net/en/index.php
https://www.everypixel.com/
https://www.hippopx.com/zh
http://on.thisistap.com/stock-images/
http://www.polayoutu.com/collections
https://www.stockvault.net/
https://pixabay.com/
https://wallhaven.cc/
https://colorhub.me/
https://www.pexels.com/zh-cn/
https://mockup.photos/
https://unsplash.com/
https://www.desktopnexus.com/
https://dynamicwallpaper.club/gallery
https://www.desktopnexus.com/
https://wall.alphacoders.com/
http://wallpaperswide.com/
https://www.freerangestock.com/
https://isorepublic.com/
https://psiupuxa.com/
https://nomad.pictures/
https://avopix.com/
http://alana.io/
https://jeshoots.com/
https://libreshot.com/
https://littlevisuals.co/
https://stocksnap.io/
http://cupcake.nilssonlee.se/
https://picjumbo.com/
https://www.lifeofpix.com/
https://beta.freelyphotos.com/
https://goodstock.photos/
https://picography.co/
https://negativespace.co/
https://www.splitshire.com/
https://www.foodiesfeed.com/
https://picalls.com/
http://www.pngall.com/
https://visualhunt.com/
https://duotone.shapefactory.co/
https://freestocks.org/
http://www.bara-art.com/
https://stokpic.com/
http://www.streetwill.co/
http://www.nipic.com/
https://focastock.com/
https://pixelmob.co/
https://www.nasa.gov/multimedia/imagegallery/index.html
https://freenaturestock.com/
https://realisticshots.com/
https://skitterphoto.com/
http://streetwill.co/
https://jaymantri.com/
https://barnimages.com/
https://www.chamberofcommerce.org/findaphoto/
http://trunklog.com/
https://www.freepik.com/
https://www.pexels.com/zh-cn/
https://discx.yuntu.io/
https://allthefreestock.com/
https://picsum.photos/
https://www.deviantart.com/
https://sc.chinaz.com/tupian/
```

### 通过软件

#### 全平台

```
https://juniperphoton.net/myersplash/
https://github.com/aitexiaoy/Strawberry-Wallpaper
https://www.artpip.com/
```

#### Windows

```
https://github.com/t1m0thyj/WinDynamicDesktop
https://www.microsoft.com/zh-cn/p/dynamic-theme/9nblggh1zbkw#activetab=pivot:overviewtab
```

#### Mac

```
https://satelliteeyes.tomtaylor.co.uk/
http://paper.meiyuan.in/
```

## 随机图片

```
https://img.xjh.me/
https://api.lolicon.app/#/setu
```

## 图片尺寸改变

```
https://bigjpg.com/
https://imagetoolbox.app/
https://placeimg.com/
https://shields.io/
```

## 图片编辑

```
https://www.iloveimg.com/zh-cn
https://www.gaitubao.com/
http://xiuxiu.web.meitu.com/
https://photomosh.com/
https://squoosh.app/
https://images.weserv.nl/
https://www.photopea.com/
https://pixlr.com/cn/
```

## 图片背景消除

```
https://www.remove.bg/zh
https://www.gaoding.com/koutu
https://photoscissors.com/?move
```

## 图片压缩

```
https://www.picdiet.com/zh-cn
https://zhitu.isux.us/
https://tinypng.com/
https://img.top/
https://www.photopea.com/
```

## 插图

```
https://undraw.co/
```

## 图片格式转换

```
https://www.jpeg.io/
https://vectormagic.com/
```

## 地图图片生成

```
https://pixelmap.amcharts.com/
```

## 图片存储

```
https://photos.google.com/
```

## 图片外链/图床

```
https://imgurl.org/
https://pic.xiaojianjian.net/
https://imgchr.com/
https://postimages.org/
https://www.picgd.com/
https://imgbb.com/
https://moetu.org/
https://jpg.dog/
https://www.moepicx.cc/login
https://www.superbed.cn/
https://img.kuibu.net/
https://imgbox.com/
https://sm.ms/
https://www.freeimagehosting.net/
https://www.hualigs.cn/
https://chrome.google.com/webstore/detail/%E5%BE%AE%E5%8D%9A%E5%9B%BE%E5%BA%8A/pinjkilghdfhnkibhcangnpmcpdpmehk
https://github.com/Suxiaogang/WeiboPicBed
https://upload.cc/
https://www.z4a.net/
https://imagebin.ca/
https://molunerfinn.com/PicGo/
https://thumbsnap.com/
https://app.photobucket.com/explore
https://ctrlq.org/images/

https://github.com/Chevereto/Chevereto-Free/tree/1.1.3
https://xiaoyou66.com/archives/774

https://github.com/Molunerfinn/PicGo
https://www.zhihu.com/question/21667151
```

## ICON库

```
https://www.iconfont.cn/
https://thenounproject.com/
https://adioma.com/icons
https://icons8.com/
https://iconmonstr.com/
https://logopond.com/
https://primer.style/octicons/
https://www.easyicon.net/
http://www.xitongzhijia.net/sbzz/2001.html
https://www.onlinewebfonts.com/
```

## 词典

```
https://www.cilin.org/
```

## ASCII绘画

```
http://patorjk.com/software/taag/#p=display&f=Graffiti&t=Type%20Something%20
http://www.network-science.de/ascii/
https://www.degraeve.com/img2txt.php
https://www.gaituba.com/text2img
http://life.chacuo.net/convertphoto2char
http://asciiflow.com/
http://www.makepic.net/Tool/Image2ascii.html
```

## 词云

```
https://wordart.com/create
https://www.moage.cn/wordart
https://www.weiciyun.com/
http://yciyun.com/
https://www.ciyunwenzi.com/
https://kt.fkw.com/ciyun.html
https://www.kt1.com/
https://wordsift.org/
http://www.picdata.cn/picdata/index.php
http://cloud.niucodata.com/
```

教程如下。

```
https://mp.weixin.qq.com/s/Hw9fbyRFg-KRJLElBUJY6g
```

# 论文相关

## 论文查重

### 通过网站

```
http://xueshu.baidu.com/usercenter/papercheck/
https://www.beiying.online/
```

### 通过软件

```
https://www.lanzoux.com/i9wkwjc
```

## 论文降重

### 通过网站

```
https://www.cnkisvip.com/
```

### 降重方法

```
https://mp.weixin.qq.com/s/b9EEoqmwFEmllAUW2YtYbg
https://zhuanlan.zhihu.com/p/21433536
https://zhuanlan.zhihu.com/p/29024560
https://www.zhihu.com/question/263528573/answer/373193244
```

## 论文下载

### 论文下载网站

```
https://www.cn-ki.net/
https://www.academia.edu/
http://www.koovin.com/
http://www.ncpssd.org/
http://www.lunwenxiazai.com/
```

### 上海科技创新资源中心

```
http://www.sgst.cn/
http://www.sstir.cn/register
```

### 图书馆馆藏

#### 广西壮族自治区图书馆

打开以下网站并注册账号，然后点击数字资源导航，选择知网进入。选择包库入口，输入之前注册的账号密码登陆即可。

```
http://www.gxlib.org.cn/
```

#### 浙江图书馆

打开支付宝并进入`浙江图书馆`生活号，点击服务-我的读者证，办理读者证。完成后打开以下链接，登录即可。

```
https://www.zjlib.cn/
```

#### 绍兴图书馆

打开支付宝并进入`绍兴图书馆`生活号，其余同上。

```
http://www.sxlib.com/
```

#### 辽宁图书馆

打开以下链接注册即可。

```
http://index.lnlib.net.cn/opac/reader/register
```

馆藏资源如下。

```
http://sso.lnlib.com/interlibSSO/main/main.jsp
```

#### 贵州图书馆

打开以下链接注册即可。

```
http://www.gzlib.org/
```

#### 长春图书馆

打开支付宝，搜索`借书`并关注生活号。点击`我要借书`，点左上角切换为`长春图书馆`，接着点击右下方我的－我的电子证，办理读者证。完成后打开以下链接，登录即可。

```
http://www.ccelib.cn/
```

#### 其余图书馆

打开支付宝，搜索`借阅宝`，并关注。点击`图书馆`，然后点击左上角，搜索需要注册的图书馆。以杭州图书馆为例，切换后点击`在借中`，按提示注册即可。

## 文库下载

### 冰点文库

适用于百度文库。

```
https://www.lanzous.com/i50pm1i
https://axu.lanzoux.com/b06aezmeh
```

### 百度文库下载利器

适用于百度文库。

```
链接 / https://pan.baidu.com/s/10ZHoaW3ECOWIZE-2EuhR6Q
提取码 / 7g09
```

### 大圣文库下载器

适用于百度文库。

```
https://www.lanzous.com/i6tve2h
```

### DoDo百度文库提取工具

适用于百度文库，只能提取doc格式的文件。

```
https://www.lanzoux.com/iknkfe9fvji
```

### 文库下载器

适用于百度文库。

```
https://naitangmao.lanzoui.com/iIkPzhsbvif
```

不支持百度文库。

```
https://www.lanzoux.com/icy3yg8a0va
```

### 豆丁当当

适用于豆丁、道客巴巴等。

```
https://www.lanzoux.com/iblf8di
```

### 豆丁当当之道客专版

适用于豆丁。

```
https://www.lanzoux.com/i1OG2g89zyh
```

### Tampermonkey脚本

适用于百度文库。

```
https://greasyfork.org/zh-CN/scripts/405373
```

### 其余网站

适用于百度文库。

```
http://wenku.bemfa.com/
http://hiwenku.com/
```

# 字体相关

## 资源

```
链接 / https://share.weiyun.com/5K5l6xo
密码 / yfvi45
```

## 字体管理软件

```
https://www.hellofont.cn/
https://typefaceapp.com/
```

## 字体示例

```
https://beizhedenglong.github.io/weird-fonts/
https://cn.piliapp.com/cool-text/
```

## 字体网站

```
https://fonts2u.com/
https://fontjoy.com/
https://archetypeapp.com/
https://www.calligraphr.com/en/
https://www.modularscale.com/
https://www.typewolf.com/
http://emotype.webflow.io/
https://www.myfonts.com/WhatTheFont/
https://www.fontsquirrel.com/
http://www.googlefonts.cn/
http://reeji.com/font/rui_zi_zhen_yan_ti/
https://www.foundertype.com/
http://www.qiuziti.com/
http://fontmap.ideo.com/
https://www.kevinandamanda.com/lifestyle/crafts-projects/free-fonts/
https://zenozeng.github.io/Free-Chinese-Fonts/
https://www.webfont.com/onlinefont/index
https://www.marksimonson.com/fonts
https://github.com/microsoft/cascadia-code
https://www.jetbrains.com/zh-cn/lp/mono/
http://www.diyiziti.com/
https://github.com/saurabhdaware/text-to-handwriting
https://github.com/FortAwesome/Font-Awesome
http://shufazi.cn/
```

# PDF

## 去水印

教程如下。

```
https://mp.weixin.qq.com/s/uDGVK439x57UeUgmGRBSOQ
```

## 转换

### PDF转Word

#### 迅捷PDF转换器

```
https://pan-yz.chaoxing.com/external/m/file/312608395106230272
```

#### Solid Converter PDF

```
https://pan-yz.chaoxing.com/external/m/file/312609211821096960
```

### 图片转PDF

#### 电脑端

##### Acrobat

打开后选择扫描和OCR，输出选择`可搜索的图像（精确）`即可。

#### 手机端

##### Adobe Scan

国区没有此APP。

## PDF内容搜索

### 手机端

#### PDF Search

## PDF全文翻译

### 通过网站

#### 腾讯文档

打开以下链接，上传文档后点击该文件进行预览，并点击右上角的翻译按钮即可。

```
https://docs.qq.com/
```

#### 其余网站

```
https://fanyi.caiyunapp.com/
http://fanyi.youdao.com/
https://fanyi.baidu.com/
https://fanyi.atman360.com/file
```

### 通过软件

```
https://copytranslator.github.io/
http://www.zhiyunwenxian.com/
```

## PDF处理

```
https://www.pdfpai.com/
https://www.ilovepdf.com/zh-cn
https://lightpdf.com/zh/
http://www.pdfdo.com/
https://smallpdf.com/
https://www.hipdf.com/
http://www.html22.com/zh/
https://issuu.com/
https://tools.pdf24.org/zh/webpage-to-pdf
```

# 全平台应用

## 全文检索

### Windows

#### Archivarius 3000

```
https://axu.lanzoux.com/ix0fdhthm6b
```

#### FileLocator Pro

```
https://axu.lanzoux.com/isXexhtp2ad
```

#### Microsoft Office 2010筛选器

添加docx/pptx/xlsx格式搜索支持。

```
https://axu.lanzoux.com/i8i14hthwxi
```

### Mac

#### DocFetcher

```
http://docfetcher.sourceforge.net/en/download.html
```

## 操作录制

### Windows

#### Quite Imposing Plus 5

```
https://axu.lanzoux.com/iQEOoidfl4b
```

#### Acrobat DC 2020

```
https://pan-yz.chaoxing.com/external/m/file/474329810964955136
```

#### TinyTask

```
https://axu.lanzoux.com/iG9inidfnba
```

## 屏幕录像

### 网页

```
https://www.apowersoft.cn/free-online-screen-recorder
https://showmore.com/zh/
```

### Mac

打开QuickTime Player后，点击文件-新建屏幕录制即可。

### 其余软件

```
https://mooc1.chaoxing.com/zt/201726403.html?_from_=&rtag=
https://getkap.co/
https://monosnap.com/welcome
https://www.apowersoft.cn/record-all-screen
```

## 划词翻译

```
https://hcfy.limingkai.cn/
https://saladict.crimx.com/
```

# 文本语音转换

## 文字转语音

### 网站

```
https://www.xfyun.cn/services/online_tts
http://www.ffkuaidu.com/
http://xfyousheng.com/
https://peiyin.wozhiyi.com/production.html
```

### 电脑端软件

```
https://www.lanzoux.com/i1Ks9e04ecj
```

### 手机端APP

```
http://www.voiceclub.cn/index.html#/
https://www.lanzoux.com/ixU3Ce050xc
https://www.lanzoux.com/iq1kKe050rg
```

## 语音转文字

### 网站

```
http://www.iyuji.cn/iyuji/home
https://www.iflyrec.com/
```

### 免费API接口

创建应用后即可获取API Key和Secret Key。

```
https://developer.baidu.com/vcast
```

其余API如下。

```
// 语音转文字
http://ai.baidu.com/tech/speech/asrpro

// 截图文字识别
http://ai.baidu.com/tech/ocr/general

// 文字转语音
http://ai.baidu.com/tech/speech/tts

// 复制翻译
http://fanyi-api.baidu.com/api/trans/product/index
```

### 软件

```
https://axu.lanzoux.com/ipdDye04dij
```

# 格式转换

## 普通格式

```
https://cn.office-converter.com/
https://www.online-convert.com/
https://convertio.co/zh/
https://cloudconvert.com/
https://www.cleverbrush.com/
```

## 视频格式转换

### Windows

#### M3U8视频辅助工具

```
https://www.52pojie.cn/thread-878944-1-1.html
```

#### QQ影音转码

```
http://pan-yz.chaoxing.com/external/m/file/322880726122655744?appId=1000&name=QQ%E5%BD%B1%E9%9F%B3%E8%BD%AC%E7%A0%81%E5%B7%A5%E5%85%B7.rar
http://pan-yz.chaoxing.com/external/m/file/322852715956563968?appId=1000&name=QQ%E5%BD%B1%E9%9F%B3%E8%BD%AC%E7%A0%81%E5%B7%A5%E5%85%B7.exe
```

#### 迅捷视频转换

```
https://pan-yz.chaoxing.com/external/m/file/312568666797150208
https://pan-yz.chaoxing.com/external/m/file/322851553912061952
```

#### 万兴全能格式转换器

```
https://pan-yz.chaoxing.com/external/m/file/322853804621729792
https://pan-yz.chaoxing.com/external/m/file/322879210716426240
```

### Mac

#### 万兴全能格式转换器

```
https://pan-yz.chaoxing.com/external/m/file/322879349300432896
```

### 其余软件

```
https://maruko.appinn.me/
```

## APE转换为单首歌曲

把cue文件拖入千千静听中，选中你需要转换的歌曲，右键选择`转换格式`即可。

## NCM/QMC格式转换

### 通过网站

```
http://tool.moresound.tk/
https://ix64.github.io/unlock-music/
```

### 通过软件

#### 全平台

```
https://github.com/Presburger/qmc-decoder/releases
```

#### Windows

```
https://lanzoux.com/i8fAuez0yjc
```

#### Mac

```
https://lanzoux.com/iQyFQez0zgf
https://lanzoux.com/iydObez0zpe
```

### 通过APP

#### Android

```
https://lanzoux.com/ivcrPez10fa
```

## TS转MP4

### Medlexo

```
https://www.lanzoux.com/b06a213gj
https://www.lanzoux.com/b06a274lg
```

### M3U8视频辅助工具

```
https://www.lanzoux.com/b06a213ra
```

## MKV/FLV/F4V秒转MP4

### 小丸工具箱

```
http://pan-yz.chaoxing.com/external/m/file/375059997288976384
http://pan-yz.chaoxing.com/external/m/file/375059995717173248
http://pan-yz.chaoxing.com/external/m/file/375028291233288192
```

XP和WIN7可能需要安装`.Net Framework 4.0`。

```
http://pan-yz.chaoxing.com/external/m/file/375059978435461120
```

# 网盘相关

## 网盘中转

```
https://www.mipony.net/en/
https://suufun.com/
```

## 百度网盘视频倍速播放

切换为电脑版，或用夸克浏览器、Alook浏览器，或用ES文件管理器。也可使用以下脚本。

```
https://greasyfork.org/zh-CN/scripts/382913
```

也可打开视频后按F12，切换到Console一栏并输入以下代码。

```
videojs.getPlayers("video-player").html5player.tech_.setPlaybackRate(2)
```

## 百度网盘下载

### baiduwp

```
https://github.com/TkzcM/baiduwp
```

### 百度企业网盘

打开以下链接以开启企业网盘，然后道路百度网盘文件并下载即可。

```
https://eyun.baidu.com/enterprise/package
```

## 搜索网盘内容

```
https://www.xiaokesoso.com/
http://baiduyun.6miu.com/
https://www.pansoso.com/
http://www.rufengso.net/
https://www.sobaidupan.com/
https://www.lingfengyun.com/
https://www.lanzou8.com/
https://www.yunpanjingling.com/
http://www.aisouziyuan.com/
https://wowenda.com/
https://so.pansoso.com/
https://www.58wangpan.com/
https://www.quzhuanpan.com/
https://www.soyunpan.com/
http://wangpan.renrensousuo.com/
https://www.sov5.cn/
http://www.verypan.com/
http://uzi8.cn/
http://www.weiyoou8.com/
http://www.kengso.com/
http://www.jisoupan.com/
https://m.51caichang.com/
http://slimego.cn/
https://www.fastsoso.cn/
http://xibianyun.com/
https://www.xiaoso.net/
http://magnet.chongbuluo.com/
http://wpsoso.com/
http://ct.vpan123.com/
```

# 流程图

## 网站

```
https://gitmind.cn/
https://processon.com/
https://bpmn.io/
https://c.runoob.com/more/shapefly-diagram/
https://app.diagrams.net/
https://www.gleek.io/
https://www.gliffy.com/
https://www.ilograph.com/
https://gitmind.cn/
https://whimsical.com/
https://www.mindmup.com/
https://my-mind.github.io/
```

## Windows

### draw.io

```
https://www.lanzoux.com/iQUObgedhrc
```

### Processist

```
https://www.lanzoux.com/idmOCgedlsh
```

### Dia

```
https://www.lanzoux.com/i5E9bgedkng
```

### ClickCharts

```
https://www.lanzoux.com/ii7PNgedi8j
```

# 网站导航

## 搜题

```
https://www.shangxueba.com/
https://www.woyaosouti.com/
https://so.kaoshibao.com/
http://www.chatiba.com//
https://www.ppkao.com/tiku/
https://so.tikuol.com/
https://shuashuati.com/
https://www.jiandati.com/
https://www.asklib.com/
https://www.xilvedu.cn/
https://www.zqnf.com/
http://sxbccss.towerboy.top/register.php?uid=67d0ff6cee
```

## 手机网页导航

```
https://www.sanfenzhong.net/
```

## 电脑网页导航

```
http://www.kguowai.com/
https://www.kanguowai.com/
https://www.ifanqiang.com/
https://botw.org/
http://www.gitnavi.com/
http://oo1.win/
http://ilxdh.com/
https://www.24kdh.com/
https://www.chaidu.com/
```

## 书签管理

```
https://raindrop.io/
https://saved.io/
https://atavi.com/
https://usepanda.com/
https://papaly.com/
```

## 聊天平台

```
https://discord.com/
https://gitter.im/
```

## 办公套件与团队工作区

```
https://trello.com/
https://www.teambition.com/
https://worktile.com/
https://shimo.im/
https://tower.im/
https://www.flowdock.com/
https://meet.jit.si/
https://miro.com/
https://element.io/
https://rocket.chat/
https://tadum.app/
https://www.rongcloud.cn/
https://asana.com/
https://basecamp.com/personal
https://www.bitrix24.com/
https://cacoo.com/
https://clickup.com/
https://clubhouse.io/
https://www.atlassian.com/software/confluence
https://funretro.io/
https://www.atlassian.com/software/jira
https://kanbanflow.com/
https://www.zoho.com.cn/workdrive/
https://www.figma.com/
https://zenkit.com/en/
https://modao.cc/
https://www.taskade.com/
https://airtable.com/
https://www.zerotier.com/
https://www.box.com/home
https://co.youdao.com/index.html
```

## 代码比较

```
https://editor.mergely.com/
```

## 信息统计与分析

```
https://tongji.baidu.com/web/welcome/login
https://marketingplatform.google.com/about/
https://www.umeng.com/
https://www.fullstory.com/
https://panelbear.com/
https://www.hitsteps.com/
https://analytics.google.com/analytics/web/provision/#/provision
https://heap.io/
https://amplitude.com/
https://www.mf999.com/
https://www.splunk.com/
```

## 空间分析和地图绘制

```
https://carto.com/
https://datamaps.world/
https://developers.arcgis.com/
https://www.mongodb.com/cloud/atlas
```

## 制作二维码连接WiFi

```
https://qifi.org
```

## 电子课本

```
https://ebook.hep.com.cn/ebooks/index.html#/
```

## 资源汇总

```
https://jubt.gq/cn/index.html
https://hao.su/tos.html
```

## 短网址生成

```
https://sina.lt/
https://u.nu/
https://suowo.cn/
https://bitly.com/
https://www.rebrandly.com/
https://tinyurl.com/
https://github.com/ellisonleao/pyshorteners
https://polrproject.org/
https://adf.ly/
https://tr.im/
https://is.gd/
```

## 地图

```
https://earth.google.com/web/
```

## 网站存档

```
https://archive.org/
http://web.archive.org/
```

## PPT模板

```
http://www.officeplus.cn/Template/Home.shtml
http://www.ypppt.com/
http://www.hippter.com/
http://www.1ppt.com/
http://www.51pptmoban.com/
```

## 电子书

```
https://sharewareonsale.tradepub.com/
```

## 工具网站

```
https://tools.kalvinbg.cn/
https://addoncrop.com/en/
https://www.vidpaw.com/en/
http://www.toolzl.com/
http://www.weibodang.cn/index
https://www.atool99.com/
https://www.punycoder.com/
https://extendsclass.com/rest-client-online.html
https://www.jiuwa.net/
```

## 低多边形生成

```
https://trianglify.io/
```

## 坐标拾取

```
http://api.map.baidu.com/lbsapi/getpoint/index.html
```

## 在线游戏

```
https://github.com/lmk123/t-rex-runner
https://aidn.jp/contents/
https://zh.y8.com/
http://www.kguowai.com/news/593.html
https://www.miniclip.com/games/en/#privacy-settings
https://neave.com/
```

## 搜索工具

```
https://search.chongbuluo.com/
http://searchguide.level3.com/
https://ahmia.fi/
https://fofa.so/
https://goobe.io/
http://www.sousuoyinqingtijiao.com/
http://magnet.chongbuluo.com/
https://weixin.sogou.com/
https://www.sogou.com/wapindex/
https://mengso.com/
https://mijisou.com/
https://searx.space/
https://github.com/searx/searx
http://simcast.com/?d=bdysou.com&s=bone&sw=18&tr=8062471120
https://www.cilin.org/
```

## 英语语法搜索

```
https://www.linggle.com/
```

## Paste Bin

```
https://hastebin.com/
https://pastebin.ubuntu.com/
https://rentry.co/
https://ghostbin.com/
https://pastebin.com/
```

## 网页快照

```
https://2tool.top/
```

## 在线平面设计

```
https://www.fotor.com.cn/
https://www.canva.cn/zh_cn/
https://www.chuangkit.com/
https://codemyui.com/
https://www.designer.io/en/
https://xlayers.dev/#/home
https://www.gaoding.com/introduction
```

## 设计流程

```
https://www.mockplus.com/cloud
https://www.invisionapp.com/
https://www.framer.com/pricing/
```

## 交互制作

```
https://octopus.do/
https://proto.io/
https://marvelapp.com/
http://pencil.evolus.vn/
```

## 产品模版

```
https://smartmockups.com/
```

## 图形计算器

```
https://www.desmos.com/calculator?lang=zh-CN
https://www.geogebra.org/graphing
```

## 在线画树

```
http://mshang.ca/syntree/
https://csacademy.com/app/graph_editor/
```

## 查找相似网站

```
https://www.similarsites.com/
https://www.similarsitesearch.com/cn/
```

## 网站制作

```
https://zh.wix.com/
https://webflow.com/
https://www.launchaco.com/
https://www.sxl.cn/
https://cn.strikingly.com/
https://www.qifeiye.com/
https://sites.google.com/new
https://jz.fkw.com/
http://www.wqdian.com/
http://www.zhuzi.me/
https://www.weebly.com/
https://www.ih5.cn/not-logged-in
```

## 海报制作

```
http://maka.im/
```

## 动漫绘画

```
https://waifulabs.com/
```

## 手机壁纸

```
http://www.wlppr.co/
```

## 文件传输与分享

### 网站

```
https://snapdrop.net/
https://www.wenshushu.cn/
https://cowtransfer.com/
http://pan.duiai.cc/
https://www.loadbt.com/en
https://letsupload.io/
https://secufiles.com/
https://anonfiles.com/
http://ge.tt/
https://uguu.se/
https://www.up-4ever.org/
https://www.zippyshare.com/
https://catbox.moe/
https://s.threatbook.cn/
http://dewdrop.io/
https://www.mirrored.to/
https://www.virustotal.com/gui/
http://filecargo.com/
https://app.tmp.link/
https://www.ppzhilian.com/
```

### 手机APP

```
http://es.vivo.com.cn/
https://airmore.cn/
https://www.airdroid.com/zh-cn/
```

## 配色网站

```
https://picular.co/
https://colorhunt.co/palettes
https://www.webdesignrankings.com/resources/lolcolors/
http://zhongguose.com/
https://nipponcolors.com/
http://www.peise.net/tools/web/
https://webgradients.com/
https://www.design-seeds.com/
```

## 设计素材

```
https://www.zcool.com.cn/
https://www.brusheezy.com/
https://mixkit.co/
https://www.vecteezy.com/
https://undraw.co/illustrations
http://www.51yuansu.com/
https://bj.96weixin.com/tools/rgb
http://bm.straightline.jp/
https://matomeno.in/
https://informationisbeautiful.net/
https://www.booooooom.com/
https://www.creativereview.co.uk/
https://freebiesbug.com/
https://niice.co/
https://dribbble.com/
https://www.skinnyships.com/
https://www.awwwards.com/
https://www.wikiart.org/
https://www.seeseed.com/
```

## 设计灵感

```
https://huaban.com/
https://www.yankodesign.com/
https://vanschneider.com/
https://thedieline.com/?
https://identitydesigned.com/
https://www.japandesign.ne.jp/
http://www.pfeifferlab.com/
https://packageinspiration.com/
http://www.knstrct.com/
https://www.thisiscolossal.com/
https://www.archdaily.com/
http://fuckingyoung.es/
https://www.designboom.com/
https://design-milk.com/
https://www.itsnicethat.com/
http://www.knstrct.com/
https://thefwa.com/
https://underconsideration.com/
https://www.vespoe.com/
https://99u.adobe.com/
https://www.howdesignlive.com/
https://www.cleverbrush.com/
```

## 材质网站

```
https://www.transparenttextures.com/
https://www.toptal.com/designers/subtlepatterns/
```

## 旅游资讯

```
https://seaside-station.com/
https://travelcoffeebook.com/
```

## 笔记

```
https://www.yinxiang.com/
https://evernote.com/intl/zh-cn/
https://note.youdao.com/
https://www.microsoft.com/zh-cn/microsoft-365/onenote/digital-note-taking-app?ms.url=onenotecom&rtc=1
https://www.google.com/keep/
```

## 临时邮箱

```
https://temp-mail.org/zh/
https://www.mohmal.com/zh/inbox
https://linshiyouxiang.net/
http://www.yopmail.com/zh/
http://www.moakt.com/zh
http://24mail.chacuo.net/
https://10minutemail.net
https://10minutemail.org/
https://10minutemail.com/
https://jovita.net/
https://t.odmail.cn/
https://xkx.me/
https://github.com/malaohu/forsaken-mail

https://mailsac.com/
https://51.ruyo.net/4068.html
```

## 邮件转发

```
https://improvmx.com/
https://51.ruyo.net/3870.html

https://mailhero.io/
https://51.ruyo.net/3581.html
```

## 邮箱相关

```
https://www.cloudmersive.com/email-verification-api
https://contact.do/
https://elasticemail.com/
https://forwardemail.net/zh
https://kickbox.com/
https://www.mail-tester.com/
https://mailchimp.com/
https://www.mailerlite.com/
https://debugmail.io/
https://burnermail.io/
https://biz.mail.ru/
https://anonaddy.com/
https://www.mailinator.com/
https://biz.mail.ru/
https://www.mailboxvalidator.com/
```

## 影子邮箱

```
https://www.uu.me/
```

## 匿名邮箱

```
https://trashmail.com/
https://protonmail.com/
```

## 免费邮箱

```
https://business.yandex.ru/mail360
https://mail.yandex.com/
https://postale.io/
https://www.migadu.com/
```

## 教育邮箱

### UNC

教程如下。

```
https://51.ruyo.net/5101.html
```

### Maraine Valley Community College

```
https://enroll.morainevalley.edu/
```

### Weatherford College

```
https://www.applytexas.org/adappc/gen/profile.WBX
```

教程如下。

```
https://51.ruyo.net/4138.html
```

### Green River college

```
https://www.greenriver.edu/
```

教程如下。

```
https://51.ruyo.net/4533.html
```

### Ocean County College

```
https://www.ocean.edu/
```

教程如下。

```
https://51.ruyo.net/4755.html
```

### Imperial Valley College

```
https://www.opencccapply.net/gateway/apply?cccMisCode=031
```

教程如下。

```
https://51.ruyo.net/5074.html
```

### Shoreline Community College

```
https://www.public.ctc.edu/ApplicantWebClient/Applicant/ApplWelcome.aspx
```

教程如下。

```
https://51.ruyo.net/6317.html
```

### Southwestern College

```
https://www.swccd.edu/admissions-and-financial-aid/apply-and-register.aspx
```

教程如下。

```
https://51.ruyo.net/6515.html
```

### Southwest TN Community College

```
https://mafa1033ssbp.southwest.tn.edu/PROD/bwskalog.P_DispLoginNon
```

教程如下。

```
https://51.ruyo.net/7184.html
```

### University of California San Diego

```
https://securelb.imodules.com/s/1170/rd18/wide.aspx?sid=1170&gid=1&pgid=8&cid=46
```

教程如下。

```
https://51.ruyo.net/5142.html
```

### Ohlone College

```
https://www.opencccapply.net/gateway/apply?cccMisCode=431
```

教程如下。

```
https://51.ruyo.net/8901.html
```

### Solano Community College

```
https://www.opencccapply.net/gateway/apply?cccMisCode=281
```

教程如下。

```
https://51.ruyo.net/9029.html
```

### Golden West College

```
https://www.opencccapply.net/gateway/apply?cccMisCode=832
```

教程如下。

```
https://51.ruyo.net/9199.html
```

### Clackamas Community College

```
https://onlineapplication.clackamas.edu/CCCApp/Step1?sType=DEG
```

教程如下。

```
https://51.ruyo.net/12604.html
```

### Antelope Valley College

```
https://www.avc.edu/studentservices/adminrec/applyonline
```

教程如下。

```
https://51.ruyo.net/4291.html
```

### tp.edu.tw

需要翻墙。

```
https://www.tp.edu.tw/showLaw
```

教程如下。

```
https://51.ruyo.net/2383.html
```

### Mott Community College

```
https://appsprod.mcc.edu/onlineapp/
```

教程如下。

```
https://51.ruyo.net/14113.html
```

### 荷兰应用科技大学

```
https://www.inholland.nl/inhollandcom/enrolment/how-to-apply/
```

教程如下。

```
https://51.ruyo.net/13597.html
```

### City Colleges of Chicago

```
https://applycredit.ccc.edu/psp/ccccsprd/EMPLOYEE/SA/c/Z_SAD_OLA_MENU.Z_AD_CRED_AUTH.GBL?
```

教程如下。

```
https://51.ruyo.net/9151.html
```

### West Los Angeles College

```
https://www.opencccapply.net/cccapply-welcome?cccMisCode=749
```

教程如下。

```
https://51.ruyo.net/8185.html
```

### South Puget Sound Community College

```
https://www.public.ctc.edu/ApplicantWebClient/Applicant/ApplWelcome.aspx
```

教程如下。

```
https://51.ruyo.net/6386.html
```

### Santa Monica College

```
https://www.smc.edu/admission-aid/how-to-apply-enroll/index.php
```

教程如下。

```
https://51.ruyo.net/12489.html
```

### Flathead Valley Community College

```
https://cams1.fvcc.edu/devWEB/FVCCOnlineApp/Account/Login?ReturnUrl=%2FdevWEB%2FFVCCOnlineApp%2Fdefault.aspx
```

教程如下。

```
https://51.ruyo.net/14116.html
```

### Beloit College

```
https://www.beloit.edu/admission/
```

教程如下。

```
https://51.ruyo.net/11869.html
```

### Central Texas College

```
https://ctc4me.force.com/apply/TX_SiteLogin?startURL=%2Fapply%2FTargetX_Portal__PB
```

教程如下。

```
https://51.ruyo.net/12540.html
```

### Tallahassee Community College

```
https://forms.tcc.fl.edu/Application/Application/Intention
```

教程如下。

```
https://51.ruyo.net/8168.html
```

### Hennepin Technical College

```
https://status.minnstate.edu/eservices/
```

教程如下。

```
https://51.ruyo.net/8156.html
```

### The College of Davidson and Davie Counties

```
https://auth.cfnc.org/Identity/Account/Login?ReturnUrl=%2Fconnect%2Fauthorize%2Fcallback%3Fclient_id%3DCfncClient%26redirect_uri%3Dhttps%253A%252F%252Fwww2.cfnc.org%252Fsignin-callback%26response_type%3Dcode%26scope%3Dopenid%2520profile%2520email%2520cfncapi%2520cfncdata%26state%3Db794e3aa3c4b4ec68274510d4dfc2a5e%26code_challenge%3DUxOx7l8zPYRv-QrcM9Q8R3XO1OGC0lGrq0VrlpBAsQo%26code_challenge_method%3DS256%26response_mode%3Dquery
```

教程如下。

```
https://51.ruyo.net/13990.html
```

### Coconino Community College

```
https://selfservice.coconino.edu/prod/bwskalog.P_DispLoginNon
```

教程如下。

```
https://51.ruyo.net/12784.html
```

### Ozarks Technical Community College

```
https://apply.otc.edu/
```

教程如下。

```
https://51.ruyo.net/8289.html
```

### Lansing Community College

```
https://starnetb.lcc.edu/LCCB/bwskalog.P_DispLoginNon
```

教程如下。

```
https://51.ruyo.net/14027.html
```

### Delaware Technical Community College

```
https://banner.dtcc.edu/dtcc/bwskalog.p_disploginnew?in_id=&cpbl=&newid=
```

教程如下。

```
https://51.ruyo.net/11299.html
```

### Sierra College

```
https://www.opencccapply.net/gateway/apply?cccMisCode=271
```

教程如下。

```
https://51.ruyo.net/8641.html
```

## 发送信息

```
https://www.afreesms.com/intl/china
```

## 接收验证码

```
https://www.xinghai.party/
http://receive-sms-online.com/
https://www.receivesmsonline.net/
https://www.freeonlinephone.org/
http://receivefreesms.com/
https://receive-sms.com/
https://sms-online.co/receive-free-sms
https://getfreesmsnumber.com/
https://receiveasms.com/
https://www.receivesms.co/
http://receive-sms-online.info/
http://receive-sms-now.com/
https://smsreceivefree.com/
https://ch.freephonenum.com/
https://www.becmd.com/
https://www.materialtools.com/
https://zh.mytrashmobile.com/
http://z-sms.com/
https://www.lothelper.com/cn
http://sms.sellaite.com/
https://www.receiveasms.com/
https://www.receive-sms.com/
http://www.shejiinn.com/
https://www.materialtools.com/
http://receivesmsverification.com/
http://receiveonlinesms.biz/
https://sms-receive.net/
http://www.receive-sms-now.com/
https://receivesmsonline.in/
https://receivefreesms.net/
https://www.receivesmsonline.net/
http://sms.sellaite.com/
```

## 个人信息/信用卡信息生成

```
https://names.igopaygo.com/credit-card
https://people.debian.org/~paulliu/ROCid.html
https://www.haoweichi.com/
https://www.fakenamegenerator.com/
https://www.fakeaddressgenerator.com/
https://tw.51240.com/
https://namefake.com/
https://homepage.net/name_generator/
http://www.e4dai.com/tool/CreditCard.asp
```

## 身份证信息查询

```
http://shenfenzheng.293.net/
```

## 有趣的网站

```
http://matthew.wagerfield.com/flat-surface-shader/
https://codepen.io/tsuhre/full/BYbjyg
https://codepen.io/pissang/full/geajpX
https://codepen.io/Yakudoo/pen/rJjOJx
https://demo.marpi.pl/biomes/
https://wangyasai.github.io/Stars-Emmision/
http://wildflower.resn.co.nz/
https://www.postcrossing.com/
https://www.cia.gov/library/abbottabad-compound/index.html
http://www.saysay.net/view25584.html
https://earthexplorer.usgs.gov/
https://www.moage.cn/wordart?union=2
https://wikileaks.org/
https://file.wikileaks.org/
https://fakeupdate.net/
https://suulnnka.github.io/BullshitGenerator/index.html
https://chp.shadiao.app/
https://selfie2anime.com/
https://raining.fm/
https://www.autopiano.cn/
http://shortof.com/search/luceneapi_node/TGIF
http://lmbtfy.cn/
```

# 参考教程

## 解锁网易云灰色版权歌曲

```
https://mp.weixin.qq.com/s/GZax8nS6lz5aeI5D_0sRKA
https://mp.weixin.qq.com/s/LsI9y6mXcBWl7C4zFglAsg
```

## 各浏览器的用户代理字符串整理

```
https://blog.csdn.net/u011127019/article/details/78079833
```

## 如何安全且不限速的下载百度网盘资源

```
https://51.ruyo.net/2590.html
```

## 使用GUI You-Get下载主流视频网站资源

```
https://51.ruyo.net/8877.html
```

## 国内外图床及其对比

```
https://www.zmonster.me/2013/12/06/image-host-service.html
```

## 【千赫整理】付费VIP/无损音乐在线下载网站合集

```
https://www.52pojie.cn/thread-929831-1-1.html
```

## 国内Chrome扩展下载网站汇总

```
https://51.ruyo.net/7553.html
```

## 分享国内主流视频网站视频解析接口(支持VIP视频)

```
https://51.ruyo.net/3127.html
```

## 国内外免费接收短信验证码平台网站

```
https://51.ruyo.net/7789.html
```

## Sicmatr1x/Free-Resource

```
https://github.com/Sicmatr1x/Free-Resource
```

## 整整43个，你要的网盘搜索引擎全在这里了！

```
https://mp.weixin.qq.com/s/vA0RJCgbqvfB6ZCaWvNzjg
```

## 评测近40个Mac软件下载站，究竟哪些是真·良心网站？

```
https://mp.weixin.qq.com/s/JWxMpzcL5cVTmSBJmhm8Hw
```

## 圈内人士才知道的动漫下载站

```
https://mp.weixin.qq.com/s/PHtbxR0cg_8q12aH-x3MCQ
```

## 知网、维普、万方等学术论文免费下载

```
https://mp.weixin.qq.com/s/O_1mDJzaMNaFGfzWwpfIJA
```

