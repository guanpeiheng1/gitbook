# 资料

## 开源项目合集

```
https://github.com/1c7/chinese-independent-developer
```

## Github学习

```
https://try.github.io/
https://github.com/phodal/github
https://github.phodal.com/




// Github Gist
代码随时存放，可以设为secret或public
https://gist.github.com/
Lepton: Github Gist管理工具
https://github.com/hackjutsu/Lepton
```

# 安装

Mac系统下已自带git，Windows需自行下载。

## Mac用户

打开终端，输入`git`，正常情况下会弹出git的帮助窗口。

## Windows用户

到官网下载`git for windows`，双击安装包进行安装，注意在`Terminal Emulator`的设置中选择`Use Windows' default console window`，这样就可以在cmd中使用git命令。

打开cmd，输入git，检查是否安装成功。

```
https://gitforwindows.org/
```

# Github使用

## 新建分支

在仓库中点击Branch，在搜索框中输入要新建的分支的名字，系统会自动弹出Create branch的窗口，点击即可。

## 设置默认分支

在仓库中点击Settings，在侧栏选择Branches，选择新的默认分支即可。

# Git使用

## 连接到Github账号

打开终端输入以下代码，一路回车生成密钥。

```
git config --global user.name "github的用户名"
git config --global user.email "github的注册邮箱地址"
ssh-keygen -t rsa -C "github的注册邮箱地址" 
```

密钥默认生成在用户目录下的`.ssh`文件夹内。打开`id_rsa.pub`，复制所有内容后，在网页上登录Github，点击头像，选择`Settings`，然后点击`SSH and GPG keys`，选择`New SSH key`，名称任取，内容把刚才复制的粘贴上去，保存。

## 基本操作

以下为创建一个仓库并完成提交的过程，按照命令顺序执行即可。

### 创建新仓库

在终端切换到工作目录，然后执行以下命令以创建本地仓库。

```
git init
```

### 设定远程仓库地址

需要在Github网页版上先新建好仓库。对于同一个本地仓库而言，该命令只在初始化仓库时执行。

```
git remote add origin git@github.com:[用户名]/[仓库名].git
```

其余命令如下。

```
// 注册远程版本库
git remote add origin <url>

// 修改远程版本库地址
git remote set-url origin <url>

// 重命名远程版本库
git remote rename origin origin1

// 删除远程版本库
git remote rm origin

// 更新所有注册的远程版本库
git remote update
```

### 查看文件状态

```
git status

// 显示精简的状态信息
git status -s
```

### 工作流处理

工作流有三个部分，其中工作目录用于存放原始文件，暂存区用于临时保存改动，HEAD指向最后一次提交的结果。

#### 由工作目录提交到暂存区

添加所有文件的命令如下。

```
git add .
```

其余可用的命令如下。

```
// 添加特定文件
git add [文件名]

// 交互式添加文件
git add -i

// 将（被版本库追踪的）本地文件的变更（修改、删除）全部记录到暂存区中
git add -u

// 将工作区中的所有改动及新增文件添加到暂存区
git add -A

// 删除文件
git rm -r [文件名]

// 直接从暂存区删除文件
git rm --cached [文件名]

// 清理未跟踪的文件和目录
git clean -fd

// 测试上面命令会删除哪些文件
git clean -nd
```

#### 由暂存区提交到HEAD

```
git commit -m "..."
```

该命令的其它参数如下。

```
-m "message" 提交说明
-a all changed file
--allow-empty 允许空白提交
--amend 对上一次的提交进行修补，不生成新的提交
--reset-author 同步提交者信息
-c xx 使用xx的提交说明
```

#### 由HEAD推送到远程仓库

```
// 推送到master分支
git push -u origin master
```

## 分支管理

### 查看分支

```
git branch -a

// 输出如下，其中*为当前分支，前面有remotes/origin/的为远程分支，没有路径的为本地分支
  dev
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
```

### 新建分支

```
// 在本地新建dev分支并切换到本分支
git checkout -b dev origin/dev
```

### 切换分支

```
// 切换到本地dev分支
git checkout dev
```

### 删除分支

```
// 删除本地dev分支
git branch -d dev
```

### 推送分支

```
// 推送dev分支到远程仓库
git push origin dev
```

### 合并分支

```
// 将dev合并到当前分支
git merge dev

// 预览差异
git diff <source_branch> <target_branch>

// 标记为合并成功
git add <filename>
```

## 仓库管理

### 克隆仓库

克隆仓库则无需对仓库进行初始化。

```
// 创建本地仓库的克隆版本
git clone /path/to/repository

// 创建远程服务器仓库的克隆版本
git clone username@host:/path/to/repository
```

### 更新本地仓库

```
git pull
```

## 文件管理与改动

### 用HEAD中的内容替换本地改动

```
git checkout -- <fliename>
```

### 丢弃本地改动并从服务器获取最新版本

```
git fetch origin
git reset --hard origin/master
```

### 将本地仓库连接到远程仓库

```
git remote add origin <server>
```

### 浅克隆

只下载最近一次的版本（默认会下载项目的完整历史版本）。

```
git clone --depth=1  https://github.com/[Github用户名]/[Github仓库名].git
```

### 查找文件

```
git grep "content"
```

### 现在.git目录所在位置

```
git rev-parse --git-dir
```

### 显示工作区根目录

```
git rev-parse --show-toplevel
```

### 设置git缓存区

```
// 增加git缓存区大小
git config --global http.postBuffer 2000000000

// 压缩配置
git config --global core.compression -1 

// 修改配置文件
export GIT_TRACE_PACKET=1
export GIT_TRACE=1
export GIT_CURL_VERBOSE=1
```

### 查看版本库中的文件列表

```
git ls-files --with-tree=HEAD
```

### 拣选

```
// 显示领先提交（未被推送到上游跟踪分支中）
git cherry

// 需提供一个提交ID作为参数，然后在当前HEAD上重放此提交，形成不管是内容还是提交说明都一样的提交
git cherry-pick
```

### 重置

```
// 不会重置引用和工作区，而是用commit下的文件替换暂存区文件，相当于撤销git add <paths>的操作，其中commit可以省略，默认为HEAD
git reset[-q][＜commit＞][--]＜paths＞

// 会重置引用，但会根据不同的参数从而影响工作区或者暂存区，其中commit也可以省略，默认为HEAD
// --hard 工作区，引用、暂存区全部替换为commit
// --soft 只更改引用，工作区和暂存区不影响
// --mixed（默认） 只更改引用和暂存区，不影响工作区
git reset[--soft|--mixed|--hard|--merge|--keep][-q][＜commit＞]
```

### 恢复进度

```
// 保存工作进度，会分别对暂存区和工作区进行保存
git stash

// 显示进度列表
git stash list

// 恢复最新一条保存进度
// 如果提供＜stash＞参数（来自于git stash list显示的列表），则从该＜stash＞中恢复，恢复完毕也将从进度列表中删除＜stash＞
// 选项--index除了恢复工作区的文件外，还尝试恢复暂存区
git stash pop[--index][＜stash＞]

// 保存进度时候指定说明
// 使用参数--patch会显示工作区和HEAD的差异
// 使用-k或--keep-index参数，在保存进度后不会将暂存区重置
git stash save "message..."

// 删除一个存储的进度，默认删除最新的进度
git stash drop[＜stash＞]

// 除了不删除恢复的进度之外，其余和git stash drop命令一样
git stash apply[--index][＜stash＞]

// 删除所有存储的进度
git stash clear

// 基于进度创建分支
git stash branch＜branchname＞＜stash＞
```

### 文件忽略

git文件忽略需要在根目录新建.gitignore文件，作用范围是其所处的目录及其子目录。并且无法使用add添加，只对未加入跟踪的文件有效。

文件内容示例如下。

```
# 这是注释行--被忽略
**.a # 忽略所有以.a为扩展名的文件。
!lib.a # 但是lib.a文件或目录不要忽略,即使前面设置了对*.a的忽略
/TODO # 只忽略此目录下的TODO文件,子目录的TODO文件不忽略
build/ # 忽略所有build/目录下的文件
doc/*.txt # 忽略文件如doc/notes.txt,但是文件如doc/server/arch.txt不被忽略
```

可通过以下命令查看被忽略的文件。

```
git status --ignored -s
```

### 变基

```
// git rebase --onto C E^ F 相当于把E到F之间的所有提交嫁接到C上
git rebase--onto＜newbase＞＜since＞＜till＞

// 变基过程暂停后继续变基操作
git rebase--continue

// 跳过某次提交
git rebase--skip

// 终止变基操作切换到变基前的分支
git rebase--abort
```

### 反转提交

将HEAD的提交反向再提交一次。

```
git rever HEAD
```

### 里程碑

```
// 显示当前版本库里程碑列表
git tag

// 创建轻量级里程碑
git tag＜tagname＞[＜commit＞]

// 创建带说明的里程碑（直接通过-m参数提供里程碑创建说明）
git tag-a＜tagname＞[＜commit＞]
git tag-m＜msg＞＜tagname＞[＜commit＞]

// 创建带GnuPG签名的里程碑（直接通过-m参数提供里程碑创建说明）
git tag-s＜tagname＞[＜commit＞]
git tag-u＜key-id＞＜tagname＞[＜commit＞]

// 在显示里程碑的时候同时显示说明，使用-n＜num＞参数，显示最多＜num＞行里程碑的说明
git tag -n1

// 只显示名称和通配符相符的里程碑
git tag-l shaofan*

// 在查看日志时使用参数--decorate可以看到提交对应的里程碑及其他引用
git log --oneline --decorate

// 使用命令git describe将提交显示为一个易记的名称。这个易记的名称来自于建立在该提交上的里程碑，若该提交没有里程碑则使用该提交历史版本上的里程碑并加上可理解的寻址信息，若提交没有对应的里程碑，但是在其祖先版本上建有里程碑，则使用类似＜tag＞-＜num＞-g＜commit＞的格式显示，其中＜tag＞是最接近的祖先提交的里程碑名字，＜num＞是该里程碑和提交之间的距离，＜commit＞是该提交的精简提交ID
// --dirty参数如果工作区有文件修改，将会显示出来
git describe

// 删除本地里程碑
git tag -d tagname

// 删除远程里程碑
git push origin :tagname
```

## 补丁

```
// 最近三次提交生成补丁文件
git format-patch -s HEAD~3..HEAD

// 应用补丁
cat *.patch | git am
```

## 日志管理

### 查看日志

```
git log
```

可加以下参数。

```
--pretty=fuller
--pretty=oneline
```

### 查看操作日志

```
// 显示master分支最近五次操作日志
git reflog show master | head -5

// 显示HEAD分支最近一次操作日志
git reflog -1
```

## 主程序

### 更新

```
git update-git-for-windows
```

### 彩色git输出

```
git config color.ui true
```

### git配置命令

```
git config
```

# Git大文件上传

git对于超过100MB的文件默认不能传，出现`GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.`错误。

``` 
// 解决方法：
https://git-lfs.github.com/

// 切换到项目目录后运行
git lfs install
git lfs track "*.psd"
git add .gitattributes
// 然后正常add，commit，push
```

# .gitingore

告诉Git哪些文件不需要添加到版本管理中。

```
https://www.jianshu.com/p/699ed86028c2
```

文件内容：

```
# dependencies  npm包文件
/node_modules

# production  打包文件
/build

# misc 
.DS_Store

npm-debug.log*
```

# Git下载加速

## 通过Gitee

打开以下链接并完成注册和登录。然后创建仓库，在新建仓库页选择`导入已有仓库`，复制需要下载的git链接，点击创建，然后下载即可。

```
https://gitee.com/
```

## 通过网站代下载

可通过以下网站进行代下载。

```
https://g.widora.cn/
https://www.toolwa.com/github/
https://git.best/
https://g.ioiox.com/
https://shrill-pond-3e81.hunsh.workers.dev/
https://gh.api.99988866.xyz/
https://github.zhlh6.cn/
https://gitclone.com/
http://gitd.cc/
https://githubd.com/#/
https://d.serctl.com/
http://www.toolzl.com/tools/githubjiasu.html
https://github.zhlh6.cn/
```

## 通过Chrome插件

下载链接如下。

```
https://ltribe.lanzoui.com/i3iAGi1xxdi
https://chrome.google.com/webstore/detail/github%E5%8A%A0%E9%80%9F/mfnkflidjnladnkldfonnaicljppahpg?utm_source=chrome-ntp-icon
```

## 通过CDN

通过以下链接即可。

```
https://cdn.jsdelivr.net/gh/[Github用户名]/[GitHub仓库名]@[版本号，没版本号可以不填]/file
```

## 通过自建服务器

```
https://github.com/hunshcn/gh-proxy
```

## 通过FastGit

```
https://doc.fastgit.org/zh-cn/
```

## 通过Microsoft Azure Notebooks

打开以下链接并登录微软账号。新建一个notebook环境并预装Git，然后使用git或者wget命令将Github资源下载到微软服务器，再从微软下载。

```
https://notebooks.azure.com/
```

## 通过代理

```
// 代理设置
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'

// 取消代理
git config --global --unset https.proxy 'socks5://127.0.0.1:1080'
git config --global --unset http.proxy 'socks5://127.0.0.1:1080'
```

## 通过修改hosts

在国内使用`git clone`命令经常会很慢，这是由于域名被限制，修改hosts文件即可解决。

### 寻找正确的IP地址

```
// 国内效果不好
全局翻墙环境下，ping一下域名即可。

或：
通过以下两个地址中的任意一个查找域名`global-ssl.fastly.Net`和`github.com`的公网地址，选择一个稳定且延迟较低的IP并记录。
http://tool.chinaz.com/dns/
https://www.ipaddress.com/
```

### 修改hosts文件

#### 通过直接修改

将以下代码添加到`hosts`文件，其中xxx部分替换为上面所查到的IP地址。

```
// Windows下hosts文件位于C:\Windows\System32\drivers\etc\hosts
// Mac/Linux下hosts文件位于/etc/hosts

xxx.xxx.xxx.xxx github.com
xxx.xxx.xxx.xxx github.global.ssl.fastly.net  
```

重启浏览器，或在终端（命令行）输入以下命令`刷新DNS缓存`。

```
// Linux/Mac
sudo /etc/init.d/networking restart

// Windows
ipconfig /flushdns
```

#### 通过SwitchHosts!

注意需要把backup配置文件也打开。

```
https://github.com/oldj/SwitchHosts
```

# Github Action

## 准备工作

打开Github，点击Settings-Seveloper settings-Personal access tokens，选择`Generate new token`，勾选`repo`，`admin:repo_hook`，`workflow`后点击`Generate token`即可。

完成Github Action的设置后，可通过随意commit或点击`Star`以激活功能。

## 天翼云盘自动签到

打开以下链接并Fork项目。

```
https://github.com/malaohu/Cloud189Checkin-Actions
```

点击Settings-Secrets，添加以下Secrets。

```
名称 / USER
内容 / 账号（可用空格分隔多个账号）

名称 / PWD
内容 / 密码（可用空格分隔多个账号）
```

保存后点击Actions，并开启功能。然后打开README.md，随意编辑后提交更改即可。

# Github仓库收集

## SingleFile

将网页保存为HTML文件。

```
https://github.com/gildas-lormeau/SingleFile
```

## Handright

模拟中文手写的轻量级Python库。

```
https://github.com/Gsllchb/Handright
```

## CC-attack

```
https://github.com/Leeon123/CC-attack
```

## manim

```
https://github.com/3b1b/manim
```

## wkhtmltopdf

将HTML渲染为PDF和各种图像格式的命令行工具。

```
https://github.com/wkhtmltopdf/wkhtmltopdf
https://wkhtmltopdf.org/
```

## RW_Password

提取收集以往泄露的密码中符合条件的强弱密码。

```
https://github.com/r35tart/RW_Password
```

## raspberry-pi-sync

在树莓派上安装Resilio Sync。

```
https://github.com/willjasen/raspberry-pi-sync
```

## WechatMomentScreenshot

朋友圈转发截图生成工具。

```
https://akarin.dev/WechatMomentScreenshot/
https://github.com/TransparentLC/WechatMomentScreenshot
```

## wkhtmltopdf

将HTML渲染为PDF和各种图像格式的命令行工具。

```
https://github.com/wkhtmltopdf/wkhtmltopdf
https://wkhtmltopdf.org/
```

## RW_Password

提取收集以往泄露的密码中符合条件的强弱密码。

```
https://github.com/r35tart/RW_Password
```

## raspberry-pi-sync

在树莓派上安装Resilio Sync。

```
https://github.com/willjasen/raspberry-pi-sync
```

## WechatMomentScreenshot

朋友圈转发截图生成工具。

```
https://akarin.dev/WechatMomentScreenshot/
https://github.com/TransparentLC/WechatMomentScreenshot
```

# 参考教程

## gitbook

```
https://git-scm.com/book/zh/v2
```

## git的基本用法

```
https://www.cnblogs.com/manjin666/p/9634324.html
```

## GIT命令汇总

```
http://shaofan.org/git-2/
```
