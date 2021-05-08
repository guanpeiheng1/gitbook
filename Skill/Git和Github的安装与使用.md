---
title: Git和Github的安装与使用
categories: Skill
abbrlink: Git-Installation
date: 2019-11-25 17:50:29
tags:
---

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g9asa3gs51j30go08caa5.jpg)

Git是一个常用的工具，与github一同使用。

<!-- more -->

# Git

Git是提交记录的集合。

## 安装

Mac下已自带git。

Windows需自行下载。到官网下载`git for windows`，双击安装包进行安装，注意在Terminal Emulator的设置中选择Use Windows' default console window，这样就可以在cmd中使用git命令。

打开cmd，输入git，检查是否安装成功。

```
https://gitforwindows.org/
```

## 新手使用

以下为创建/克隆一个仓库并完成提交的过程，按照命令顺序执行即可。

### 连接到Github账号

只需配置一次。打开终端输入以下代码，一路回车生成密钥。

```
git config --global user.name "github的用户名"
git config --global user.email "github的注册邮箱地址"
ssh-keygen -t rsa -C "github的注册邮箱地址" 
```

密钥默认生成在用户目录下的`.ssh`文件夹内。打开id_rsa.pub，复制所有内容后，在网页上登录Github，点击头像，选择Settings-SSH and GPG keys，选择New SSH key，名称任取，内容把刚才复制的粘贴上去，保存即可。

### 本地新建仓库或克隆现有仓库

#### 本地新建仓库

即仓库初始化，只需在第一次新建仓库时执行。在终端切换到工作目录，然后执行以下命令以创建本地仓库。

```
git init
```

#### 克隆现有仓库

克隆仓库则无需对仓库进行初始化。

```
# 创建本地仓库的克隆版本
git clone /path/to/repository

# 创建远程服务器仓库的克隆版本
git clone git@github.com:[用户名]/[仓库名].git
```

### 设定远程仓库地址

只需在第一次新建仓库时执行，需要在Github网页版上先新建好仓库。对于同一个本地仓库而言，该命令只在初始化仓库时执行。

```
git remote add origin git@github.com:[用户名]/[仓库名].git
```

其余可用命令如下。

|               命令               |           含义           |
|----------------------------------|--------------------------|
| git remote add origin <url>      | 注册远程版本库           |
| git remote set-url origin <url>  | 修改远程版本库地址       |
| git remote rename origin origin1 | 重命名远程版本库         |
| git remote rm origin             | 删除远程版本库           |
| git remote update                | 更新所有注册的远程版本库 |

### 工作流处理

每次进行修改后均要执行。工作流有三个部分，其中工作目录用于存放原始文件，暂存区用于临时保存改动，HEAD指向最后一次提交的结果。

#### 提交到暂存区

添加所有文件的命令如下。

```
git add .
```

其余可用命令如下。

|           命令           |                                含义                                |
|--------------------------|--------------------------------------------------------------------|
| git add [文件名]         | 添加特定文件                                                       |
| git add -i               | 交互式添加文件                                                     |
| git add -u               | 将（被版本库追踪的）本地文件的变更（修改、删除）全部记录到暂存区中 |
| git add -A               | 将工作区中的所有改动及新增文件添加到暂存区                         |
| git add -p [文件名]      | 选择文件中需要commit的修改（修改文件后git会进行记录，运行本命令时会显示所有改动并询问哪些改动需要被提交）                                           |
| git rm -r [文件名]       | 删除文件                                                           |
| git rm --cached [文件名] | 直接从暂存区删除文件                                               |
| git clean -fd            | 清理未跟踪的文件和目录                                             |
| git clean -nd            | 测试上面命令会删除哪些文件                                         |

#### 提交到HEAD

每执行一次本命令，当前修改即被打包为一个提交记录，且前一个提交记录成为当前提交记录的父节点，当前分支头HEAD则指向当前分支。

```
git commit -m "..."
```

提交前后示意如下，其中main为分支，星号表示当前处于该分支。注意commit只会移动当前分支，而不影响其它分支。

```
# 执行前
C0───C1
      │
      main*

# 执行后
C0───C1───C2
           │
           main*
```

该命令参数含义如下。

|      参数      |                  含义                  |
|----------------|----------------------------------------|
| -m "message"   | 提交说明                               |
| -a             | 提交所有修改过的文件                   |
| --allow-empty  | 允许空白提交                           |
| --amend        | 对上一次的提交进行修补，不生成新的提交 |
| --reset-author | 同步提交者信息                         |
| -c [名称]      | 使用[名称]的提交说明                   |

#### 由HEAD推送到远程仓库

```
# 推送到master分支
git push -u origin master
```

## 分支管理

分支指向特定的提交记录。

### 新建与切换分支

```
# 在本地新建bugFix分支，<ref>为分支指向的位置，可省略，默认指向HEAD
git branch bugFix <ref>

# 切换到本地bugFix分支
git checkout bugFix
```

以上两条命令等同于以下一条命令。

```
# 在本地新建bugFix分支并切换到本分支
git checkout -b bugFix
```

执行以上命令前后提交记录如下。

```
# 执行前
C0───C1
      │
      main*

# 执行后
C0───C1
      │
      main
      bugFix*
```

### 合并分支

```
# 将bugFix分支合并到当前分支
git merge bugFix
```

执行以上命令前后提交记录如下。

```
# 执行前
           bugFix
           │
C0───C1───C2
      │
      └───C3
           │
           main*

# 执行后
           bugFix
           │
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*
```

注意，若需将bugFix分支移动到main，则运行以下命令。由于main继承自bugFix，因此Git会将bugFix移动到main处。

```
git checkout bugFix
git merge main
```

执行以上命令前后提交记录如下。

```
# 执行前
           bugFix
           │
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*

# 执行后
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main
                 bugFix*
```

其余可用命令如下。

|                   命令                   |      含义      |
|------------------------------------------|----------------|
| `git diff <source_branch> <target_branch>` | 预览差异       |
| `git add <filename>`                       | 标记为合并成功 |

### 变基

变基是另一种合并分支的方法，实际上就是取出一系列的提交记录，复制它们，然后在另外一个地方逐个的放下去。

```
# 把bugFix分支里的工作直接移到main分支
git rebase main
```

执行以上命令前后提交记录如下。注意，分支应指向最新的提交记录，运行rebase时，Git默认将从两者的公共节点出发，断开本分支与公共节点的连接，并指向指定分支以使其成为父节点。

```
# 执行前
           bugFix*
           │
C0───C1───C3
      │
      └───C2
           │
           main

# 执行后
# bugFix分支断开与公共节点C1的连接，并将其指向main处即C2
C0───C1─ ─C3
      │
      └───C2───C3'
           │    │
        main    bugFix*
```

注意，若需将main分支移动到bugFix，则运行以下命令。由于bugFix继承自main，因此Git会将main移动到bugFix处。

```
git checkout main
git rebase bugFix
```

执行以上命令前后提交记录如下。

```
# 执行前
C0───C1─ ─C3
      │
      └───C2───C3'
           │    │
       main*    bugFix

# 执行后
C0───C1─ ─C3
      │
      └───C2───C3'
                │
                main*
                bugFix
```

其余可用命令如下。

|                     命令                     |                                含义                                |
|----------------------------------------------|--------------------------------------------------------------------|
| git rebase -i <分支名>                       | 将当前分支移动到分支名所在位置，并交互式调整该分支名以下的提交记录 |
| `git rebase --onto <newbase> <since> <till>` | 把`<since>`（不含）到`<till>`（包含）之间的所有提交嫁接到`<newbase>`上       |
| git rebase --continue                         | 变基过程暂停后继续变基操作                                         |
| git rebase --skip                             | 跳过某次提交                                                       |
| git rebase --abort                            | 终止变基操作切换到变基前的分支                                     |

### 指定分支指向

```
# 将main分支强制指向HEAD
git branch -f main HEAD
```

### 查看分支

```
git branch -a
```

输出示例如下。其中*为当前分支，前面有remotes/origin/的为远程分支，没有路径的为本地分支。

```
  dev
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
```

### 删除分支

```
# 删除本地dev分支
git branch -d dev
```

## 提交树移动

### HEAD

HEAD指向最后一次提交的结果，一般指向分支名。可通过以下命令查看HEAD的指向。

```
# 查看HEAD指向
cat .git/HEAD

# 若HEAD指向一个引用，则使用此命令
git symbolic-ref HEAD
```

分离的HEAD即为让HEAD指向某个具体的提交记录而非分支名。通过哈希值可指定提交记录，命令如下。哈希值不必全部填写，填写至Git能唯一识别该提交记录时的长度即可。

```
# 让HEAD指向提交记录C1而非分支
git checkout [C1的哈希值]
```

### 相对引用

相对引用符号含义如下。相对引用可连续使用。

|   符号   |               含义               |
|----------|----------------------------------|
| ^        | 向上移动一个提交记录，可连续使用 |
| `~<num>` | 向上移动多个提交记录，如~3       |
| `^<num>` | 移动到第`<num>`个父节点，如^2^2  |

以以下命令为例。

```
git checkout main^
git checkout HEAD^

# 以上两条命令等同于以下一条命令
git checkout main~2
```

执行以上命令前后提交记录如下。

```
# 执行前
C0───C1───C2
           │
           main*

# 执行第一行命令后
C0───C1───C2
      │    │
   HEAD    main

# 执行第二行命令后
C0───C1───C2
 │         │
 HEAD      main
```

对于`^<num>`，需注意此处的`<num>`与`~<num>`的不同，示例如下。

```
git checkout main^2
```

假设C3为C4的第一个父节点，C2为C4的第二个父节点。执行以上命令前后提交记录如下。

```
# 执行前
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*

# 执行后
           HEAD
           │
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*
```

### 撤销变更

#### reset命令

git reset通过把分支记录回退几个提交记录来实现撤销改动。该命令对本地分支起作用。

```
git reset HEAD~1
```

执行以上命令前后提交记录如下。

```
# 执行前
C0───C1───C2
           │
           main*

# 执行后
C0───C1─ ─C2
      │
      main*
```

其余可用命令如下。`[<commit>]`可以省略，默认为HEAD。

|                  命令                  |                                            含义                                           |
|----------------------------------------|-------------------------------------------------------------------------------------------|
| `git reset [-q][<commit>][-] <paths>` | 不会重置引用和工作区，而是用commit下的文件替换暂存区文件，相当于撤销`git add <paths>`的操作 |
|   git reset[--soft\|--mixed\|--hard\|--merge\|--keep][-q][<commit>]|     会重置引用，但会根据不同的参数从而影响工作区或者暂存区            |

各参数含义如下。

|   参数  |                    含义                    |
|---------|--------------------------------------------|
| --hard  | 工作区、引用、暂存区全部替换为commit       |
| --soft  | 只更改引用，工作区和暂存区不影响           |
| --mixed | 默认参数，只更改引用和暂存区，不影响工作区 |

#### revert命令

git revert用于撤销更改并实现共享。

```
git revert HEAD
```

执行以上命令前后提交记录如下。其中C2'的内容与C1一致。

```
# 执行前
C0───C1───C2
           │
           main*

# 执行后
C0───C1───C2───C2'
                │
                main*
```

## 提交记录管理

### 提取特定提交记录

将提交记录复制到当前分支。

```
git cherry-pick C2 C4
```

执行以上命令前后提交记录如下。

```
# 执行前
                     side
                     │
C0───C1───C2───C3───C4
      │
      └───C5
           │
           main*

# 执行后
                     side
                     │
C0───C1───C2───C3───C4
      │
      └───C5───C2'───C4'
                      │
                      main*
```

### 修改先前提交记录

可先通过rebase命令将要修改的提交记录移到最前，完成修改并commit后，再用rebase命令调整为原来的顺序。

也可通过cherry-pick命令将要修改的提交记录单独复制出来，完成修改并commit后，再用cherry-pick命令复制该提交记录以后的提交记录。

### 标签

即tag，也称为里程碑，用于标记某个特定的提交记录。该tag不会随着新的提交移动，永久指向该提交记录。可以通过checkout命令切换到指定的tag。

```
git tag V1 C1
```

执行以上命令前后提交记录如下。

```
# 执行前
C0───C1───C2
           │
           main*

# 执行后
C0───C1───C2
      │    │
      v1   main*
```

通过以下命令可查找离指定位置最近的标签，其中`<ref>`为任何能被Git识别成提交记录的引用，不指定则默认为HEAD。--dirty参数可选，表示如果工作区有文件修改，将会显示出来。

```
git describe <ref> [--dirty]
```

输出如下。其中tag表示离ref最近的标签，numCommits表示这个ref与tag相差有多少个提交记录，hash表示给定的ref所表示的提交记录哈希值的前几位。

注意，当ref提交记录上有某个标签时，则只输出标签名称。

```
<tag>_<numCommits>_g<hash>
```

其余可用命令如下。

|                    命令                    |                                     含义                                     |
|--------------------------------------------|------------------------------------------------------------------------------|
| git tag                                    | 显示当前版本库标签列表                                                       |
| `git tag <tagname> [<commit>]`             | 创建轻量级标签                                                               |
| `git tag -a <tagname> [<commit>]`          | 创建带说明的标签                                                             |
| `git tag -m <msg> <tagname> [<commit>]`    | 创建带说明的里程碑，通过-m参数提供里程碑创建说明                             |
| `git tag -s <tagname> [<commit>] `         | 创建带GnuPG签名的标签                                                        |
| `git tag -u <key-id> <tagname> [<commit>]` | 创建带GnuPG签名的标签，通过-m参数提供签名ID                                  |
| git tag -n1                                | 在显示标签的时候同时显示说明，使用`-n<num>`参数，显示最多`<num>`行标签的说明 |
| git tag -l shaofan*                        | 只显示名称和通配符相符的标签                                                 |
| git log --oneline --decorate               | --decorate参数可以看到提交对应的标签及其他引用                               |
| git tag -d tagname                         | 删除本地标签                                                                 |

## 远程仓库管理

### 克隆远程仓库

clone命令在本地仓库增加`<remote name>/<branch name>`分支，其中Git默认将远程仓库命名为origin。

```
# --depth参数可选
# --depth=1即只下载最近一次的版本
git clone [--depth=1] git@github.com:[用户名]/[仓库名].git
```

执行以上命令前后提交记录如下。

```
# 执行前
## 远程端
C0───C1
      │
      main*

# 执行后
## 远程端
C0───C1
      │
      main*
## 本地端
C0───C1
      │
      main*
      origin/main
```

注意，对远程分支进行commit时，Git会进入分离HEAD状态，不会更新origin/main分支。origin/main分支只有在远程仓库中相应的分支更新了以后才会更新。

```
git checkout origin/main
git commit
```

### 从远程仓库获取提交

```
# <place>可以为<destination>或<source>:<destination>
# <source>:<destination>为refspecs，表示Git可识别的位置
# <source>可留空
# <remote>和<place>可省略
git fetch <remote> <place>
```

命令示例如下。

```
# 下载所有的提交记录到本地各个远程分支
# 即origin/<branchname>
git fetch

# 将远程main分支内容下载到本地origin/main分支
git fetch origin main

# 将远程foo父节点对应的分支下载到本地bar分支
# 注意不是本地origin/bar分支，因为bar分支可能在远程仓库中不存在
# 若本地bar分支不存在则自动创建
git fetch origin foo~1:bar

# 将远程空分支下载到本地bar分支
# 由于没有分支，事实上创建了本地bar分支
git fetch origin :bar
```

执行以上命令前后提交记录如下。注意本地的main分支不会被更新。

```
# 执行前
## 远程端
C0───C1───C2───C3
                │
                main*
## 本地端
C0───C1
      │
      main*
      origin/main

# 执行后
## 远程端
C0───C1───C2───C3
                │
                main*
## 本地端
C0───C1───C2───C3
      │         │
      main*     origin/main
```

若希望更新本地分支，可使用fetch命令后使用以下命令之一。

```
git cherry-pick origin/main
# 或
git rebase origin/main
# 或
git merge origin/main
```

也可以直接使用pull命令。使用pull命令则无需再使用fetch命令。git pull等同于git fetch加上git merge，其参数使用与前面完全一致。

```
# <place>可以为<destination>或<source>:<destination>
# <source>:<destination>为refspecs，表示Git可识别的位置
# <source>可留空
# <remote>和<place>可省略
git pull <remote> <place>
```

命令示例如下。

```
# 将远程main分支内容下载到本地origin/main分支，并完成分支合并
git pull origin main

# 将远程main分支内容下载到本地foo分支，并完成分支合并
# 若本地foo分支不存在则自动创建
git pull origin main:foo
```

执行以上命令前后提交记录如下。

```
# 执行前
## 远程端
C0───C1───C3
           │
           main*
## 本地端
           main*
           │
C0───C1───C2
      │    
      origin/main

# 执行后
## 远程端
C0───C1───C3
           │
           main*
## 本地端
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
           │     │
 origin/main     main*
```

### 提交本地变更

push命令将变更上传到指定的远程仓库，并在远程仓库上合并新提交记录。若分支在远程仓库中不存在，Git将会自动创建。

```
# <place>可以为<destination>或<source>:<destination>
# <source>:<destination>为refspecs，表示Git可识别的位置
# <remote>和<place>可省略
# 不带任何参数时的行为与Git的一个名为push.default的配置有关
# 不带任何参数时，一般为推送HEAD所在分支
git push <remote> <place>
```

命令示例如下。

```
# 将本地main分支推送到远程origin/main分支
git push origin main

# 将本地foo父节点对应的分支推送到远程origin/main分支
git push origin foo^:main

# 将本地空分支推送到远程origin/foo分支
# 由于没有分支，事实上删除了远程origin/foo分支
git push origin :foo
```

执行以上命令前后提交记录如下。

```
# 执行前
## 远程端
C0───C1
      │
      main*
## 本地端
           main*
           │
C0───C1───C2
      │    
origin/main

# 执行后
## 远程端
C0───C1───C2
           │
           main*
## 本地端
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
           │     │
 origin/main     main*
```

### 历史偏离处理

假设当前提交记录如下。此时执行git push将失败，因为C3基于C1，但远程仓库中已经变成了C2，Git无法确定是需要先合并分支还是直接回滚到C1。

```
## 远程端
C0───C1───C2
           │
           main*

## 本地端
           main*
           │
C0───C1───C3
      │    
origin/main
```

此时可先更新本地的远程分支，然后通过rebase命令将C3移到最新的提交记录下，最后再提交即可。

```
git fetch
git rebase origin/main
git push

# git fetch和git rebase可合并为以下命令
git pull --rebase
```

执行以上命令后提交记录如下。

```
## 远程端
C0───C1───C2───C3
                │
                main*
## 本地端
      ┌───C3
      │
C0───C1───C2───C3'
                │ 
                main*
                origin/main
```

也可用merge代替rebase。

```
git fetch
git merge origin/main
git push

# git fetch和git merge可合并为以下命令
git pull
```

执行以上命令后提交记录如下。

```
## 远程端
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*

## 本地端
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*
                 origin/main
```

### 远程追踪分支

以上例子中，本地的main分支与远程的origin/main分支关联。可通过以下命令修改远程追踪分支。

```
# 新建一个新分支totallyNotMain，该分支跟踪远程分支origin/main
git checkout -b totallyNotMain origin/main

# 将现有分支totallyNotMain设置为跟踪远程分支origin/main
# 若当前即在totallyNotMain分支，可在以下命令中省略totallyNotMain
git branch -u o/main totallyNotMain
```

执行以上命令后提交记录如下。

```
## 远程端
C0───C1───C2
           │
           main*

## 本地端
C0───C1
      │ 
      main
      totallyNotMain*
      origin/main
```

若执行git pull，则变为如下。注意main分支未被更新，git push同理。

```
## 远程端
C0───C1───C2
           │
           main*

## 本地端
C0───C1───C2
      │    │
   main    totallyNotMain*
           origin/main
```

## 文件管理与改动

### 查看文件状态

```
git status

# 显示精简的状态信息
git status -s
```

### 文件忽略

git文件忽略需要在根目录新建.gitignore文件，作用范围是其所处的目录及其子目录，无法使用add添加。只对未加入跟踪的文件有效。

文件内容示例如下。

```
# 忽略所有以.a为扩展名的文件
**.a

# 但lib.a文件或目录不要忽略
!lib.a

# 只忽略此目录下的TODO文件，子目录的TODO文件不忽略
/TODO

# 忽略所有build/目录下的文件
build/

# 忽略文件如doc/notes.txt，但是文件如doc/server/arch.txt不被忽略
doc/*.txt
```

常用配置如下。

```
# dependencies  npm包文件
/node_modules

# production  打包文件
/build

# misc 
.DS_Store

npm-debug.log*
```

可通过以下命令查看被忽略的文件。

```
git status --ignored -s
```

### 目录显示

```
# 显示现在.git目录所在位置
git rev-parse --git-dir

# 显示工作区根目录
git rev-parse --show-toplevel
```

### 大文件上传

git对于超过100MB的文件默认不能上传，出现`GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.`错误。

可通过安装git-lfs解决，切换到项目目录并执行以下命令即可。注意无法对安装该工具之前的提交应用该工具。完成安装后按照正常的仓库提交流程进行即可。

```
git lfs install
git lfs track "*.psd" // psd为需要上传的大文件后缀
git add .gitattributes
```

### 用HEAD中的内容替换本地改动

```
git checkout -- <fliename>
```

### 丢弃本地改动并从服务器获取最新版本

```
git fetch origin
git reset --hard origin/master
```

### 查找文件

```
git grep "content"
```

### 查看版本库中的文件列表

```
git ls-files --with-tree=HEAD
```

### 拣选

```
# 显示领先提交（未被推送到上游跟踪分支中）
git cherry
```

### 恢复进度

|                   命令                  |                                                                                              含义                                                                                              |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| git stash                               | 保存工作进度，会分别对暂存区和工作区进行保存                                                                                                                                                   |
| git stash list                          | 显示进度列表                                                                                                                                                                                   |
| `git stash pop [--index] [<stash>]`     | 恢复最新一条保存进度，如果提供`<stash>`参数（来自于git stash list显示的列表）则从该`<stash>`中恢复，恢复完毕也将从进度列表中删除`<stash>`。选项--index除了恢复工作区的文件外，还尝试恢复暂存区 |
| git stash save "message..."             | 保存进度时候指定说明。使用参数--patch会显示工作区和HEAD的差异，使用-k或--keep-index参数在保存进度后不会将暂存区重置                                                                            |
| `git stash drop [<stash>] `             | 删除一个存储的进度，默认删除最新的进度                                                                                                                                                         |
| `git stash apply [--index] [<stash>]`   | 除了不删除恢复的进度之外，其余和git stash drop命令一样                                                                                                                                         |
| git stash clear                         | 删除所有存储的进度                                                                                                                                                                             |
| `git stash branch <branchname> <stash>` | 基于进度创建分支                                                                                                                                                                               |

### 反转提交

```
# 将HEAD的提交反向再提交一次
git rever HEAD
```

### 补丁

```
# 最近三次提交生成补丁文件
git format-patch -s HEAD~3..HEAD

# 应用补丁
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
# 显示master分支最近五次操作日志
git reflog show master | head -5

# 显示HEAD分支最近一次操作日志
git reflog -1
```

## 主程序管理

### 更新

```
git update-git-for-windows
```

### 彩色git输出

```
git config color.ui true
```

### git配置

```
git config
```

### 设置缓存区

```
# 增加git缓存区大小
git config --global http.postBuffer 2000000000

# 压缩配置
git config --global core.compression -1 

# 修改配置文件
export GIT_TRACE_PACKET=1
export GIT_TRACE=1
export GIT_CURL_VERBOSE=1
```

## 下载加速

### 通过Gitee

打开以下链接并完成注册和登录。然后创建仓库，在新建仓库页选择`导入已有仓库`，复制需要下载的git链接，点击创建，然后下载即可。

```
https://gitee.com/
```

### 通过镜像站

可通过以下网站代替github.com进行访问。

```
https://github.iapk.cc
https://hub.fastgit.org
https://github.bajins.com
https://github.com.cnpmjs.org
https://github.wuyanzheshui.workers.dev
```

也可使用Fast Github，游猴脚本如下。

```
https://greasyfork.org/zh-CN/scripts/397419
```

### 通过代下载

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

### 通过插件

#### Chrome插件

可通过`Github加速`插件，下载链接如下。

```
https://fhefh2015.github.io/Fast-GitHub/
https://ltribe.lanzoui.com/i3iAGi1xxdi
https://chrome.google.com/webstore/detail/github%E5%8A%A0%E9%80%9F/mfnkflidjnladnkldfonnaicljppahpg
```

#### 游猴插件

可通过`Github增强`插件。

```
https://greasyfork.org/scripts/412245
```

### 通过CDN

通过以下链接即可。

```
https://cdn.jsdelivr.net/gh/[Github用户名]/[GitHub仓库名]@[版本号，没版本号可以不填]/file
```

也可将raw.githubusercontent.com替换为raw.staticdn.net。

### 通过自建服务器

#### 部署到Cloudflare Worker

打开以下链接并登录，新建一个Worker。

```
https://workers.cloudflare.com
```

复制以下代码到左侧代码框后进行部署，即可通过Worker所分配的网站进入部署好的Github加速站点。

```
'use strict'

/**
 * static files (404.html, sw.js, conf.js)
 */
const ASSET_URL = 'https://hunshcn.github.io/gh-proxy/'
// 前缀，如果自定义路由为example.com/gh/*，将PREFIX改为 '/gh/'，注意，少一个杠都会错！
const PREFIX = '/'
// git使用cnpmjs镜像、分支文件使用jsDelivr镜像的开关，0为关闭，默认开启
const Config = {
    jsdelivr: 1,
    cnpmjs: 1
}

/** @type {RequestInit} */
const PREFLIGHT_INIT = {
    status: 204,
    headers: new Headers({
        'access-control-allow-origin': '*',
        'access-control-allow-methods': 'GET,POST,PUT,PATCH,TRACE,DELETE,HEAD,OPTIONS',
        'access-control-max-age': '1728000',
    }),
}

/**
 * @param {any} body
 * @param {number} status
 * @param {Object<string, string>} headers
 */
function makeRes(body, status = 200, headers = {}) {
    headers['access-control-allow-origin'] = '*'
    return new Response(body, {status, headers})
}


/**
 * @param {string} urlStr
 */
function newUrl(urlStr) {
    try {
        return new URL(urlStr)
    } catch (err) {
        return null
    }
}


addEventListener('fetch', e => {
    const ret = fetchHandler(e)
        .catch(err => makeRes('cfworker error:\n' + err.stack, 502))
    e.respondWith(ret)
})


/**
 * @param {FetchEvent} e
 */
async function fetchHandler(e) {
    const req = e.request
    const urlStr = req.url
    const urlObj = new URL(urlStr)
    let path = urlObj.searchParams.get('q')
    if (path) {
        return Response.redirect('https://' + urlObj.host + PREFIX + path, 301)
    }
    // cfworker 会把路径中的 `//` 合并成 `/`
    path = urlObj.href.substr(urlObj.origin.length + PREFIX.length).replace(/^https?:\/+/, 'https://')
    const exp1 = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:releases|archive)\/.*$/i
    const exp2 = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:blob)\/.*$/i
    const exp3 = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:info|git-).*$/i
    const exp4 = /^(?:https?:\/\/)?raw\.githubusercontent\.com\/.+?\/.+?\/.+?\/.+$/i
    if (path.search(exp1) === 0 || !Config.cnpmjs && (path.search(exp3) === 0 || path.search(exp4) === 0)) {
        return httpHandler(req, path)
    } else if (path.search(exp2) === 0) {
        if (Config.jsdelivr){
            const newUrl = path.replace('/blob/', '@').replace(/^(?:https?:\/\/)?github\.com/, 'https://cdn.jsdelivr.net/gh')
            return Response.redirect(newUrl, 302)
        }else{
            path = path.replace('/blob/', '/raw/')
            return httpHandler(req, path)
        }
    } else if (path.search(exp3) === 0) {
        const newUrl = path.replace(/^(?:https?:\/\/)?github\.com/, 'https://github.com.cnpmjs.org')
        return Response.redirect(newUrl, 302)
    } else if (path.search(exp4) === 0) {
        const newUrl = path.replace(/(?<=com\/.+?\/.+?)\/(.+?\/)/, '@$1').replace(/^(?:https?:\/\/)?raw\.githubusercontent\.com/, 'https://cdn.jsdelivr.net/gh')
        return Response.redirect(newUrl, 302)
    } else {
        return fetch(ASSET_URL + path)
    }
}


/**
 * @param {Request} req
 * @param {string} pathname
 */
function httpHandler(req, pathname) {
    const reqHdrRaw = req.headers

    // preflight
    if (req.method === 'OPTIONS' &&
        reqHdrRaw.has('access-control-request-headers')
    ) {
        return new Response(null, PREFLIGHT_INIT)
    }

    let rawLen = ''

    const reqHdrNew = new Headers(reqHdrRaw)

    let urlStr = pathname
    if (urlStr.startsWith('github')) {
        urlStr = 'https://' + urlStr
    }
    const urlObj = newUrl(urlStr)

    /** @type {RequestInit} */
    const reqInit = {
        method: req.method,
        headers: reqHdrNew,
        redirect: 'follow',
        body: req.body
    }
    return proxy(urlObj, reqInit, rawLen, 0)
}


/**
 *
 * @param {URL} urlObj
 * @param {RequestInit} reqInit
 */
async function proxy(urlObj, reqInit, rawLen) {
    const res = await fetch(urlObj.href, reqInit)
    const resHdrOld = res.headers
    const resHdrNew = new Headers(resHdrOld)

    // verify
    if (rawLen) {
        const newLen = resHdrOld.get('content-length') || ''
        const badLen = (rawLen !== newLen)

        if (badLen) {
            return makeRes(res.body, 400, {
                '--error': `bad len: ${newLen}, except: ${rawLen}`,
                'access-control-expose-headers': '--error',
            })
        }
    }
    const status = res.status
    resHdrNew.set('access-control-expose-headers', '*')
    resHdrNew.set('access-control-allow-origin', '*')

    resHdrNew.delete('content-security-policy')
    resHdrNew.delete('content-security-policy-report-only')
    resHdrNew.delete('clear-site-data')

    return new Response(res.body, {
        status,
        headers: resHdrNew,
    })
}
```

#### 部署到VPS

连接到服务器并执行以下命令部署docker，其中第一个80表示要暴露的端口。

```
docker run -d --name="gh-proxy-py" -p 0.0.0.0:80:80 --restart=always hunsh/gh-proxy-py:latest
```

通过以下命令安装依赖，注意应当使用python 3。

```
pip install flask requests
```

新建main.py并填写以下内容，其中部分选项根据注释进行修改。修改完成后保存并运行该脚本即可。

```
# -*- coding: utf-8 -*-
import re

import requests
from flask import Flask, Response, redirect, request
from requests.exceptions import (
    ChunkedEncodingError,
    ContentDecodingError, ConnectionError, StreamConsumedError)
from requests.utils import (
    stream_decode_response_unicode, iter_slices)
from urllib3.exceptions import (
    DecodeError, ReadTimeoutError, ProtocolError)

# config
# git使用cnpmjs镜像、分支文件使用jsDelivr镜像的开关，0为关闭，默认开启
jsdelivr = 1
cnpmjs = 1
size_limit = 1024 * 1024 * 1024 * 999  # 允许的文件大小，默认999GB，相当于无限制了 https://github.com/hunshcn/gh-proxy/issues/8
HOST = '127.0.0.1'  # 监听地址，建议监听本地然后由web服务器反代
PORT = 80  # 监听端口
ASSET_URL = 'https://hunshcn.github.io/gh-proxy'  # 主页

app = Flask(__name__)
CHUNK_SIZE = 1024 * 10
index_html = requests.get(ASSET_URL, timeout=10).text
exp1 = re.compile(r'^(?:https?://)?github\.com/.+?/.+?/(?:releases|archive)/.*$')
exp2 = re.compile(r'^(?:https?://)?github\.com/.+?/.+?/(?:blob)/.*$')
exp3 = re.compile(r'^(?:https?://)?github\.com/.+?/.+?/(?:info|git-).*$')
exp4 = re.compile(r'^(?:https?://)?raw\.githubusercontent\.com/.+?/.+?/.+?/.+$')


@app.route('/')
def index():
    if 'q' in request.args:
        return redirect('/' + request.args.get('q'))
    return index_html


def iter_content(self, chunk_size=1, decode_unicode=False):
    """rewrite requests function, set decode_content with False"""

    def generate():
        # Special case for urllib3.
        if hasattr(self.raw, 'stream'):
            try:
                for chunk in self.raw.stream(chunk_size, decode_content=False):
                    yield chunk
            except ProtocolError as e:
                raise ChunkedEncodingError(e)
            except DecodeError as e:
                raise ContentDecodingError(e)
            except ReadTimeoutError as e:
                raise ConnectionError(e)
        else:
            # Standard file-like object.
            while True:
                chunk = self.raw.read(chunk_size)
                if not chunk:
                    break
                yield chunk

        self._content_consumed = True

    if self._content_consumed and isinstance(self._content, bool):
        raise StreamConsumedError()
    elif chunk_size is not None and not isinstance(chunk_size, int):
        raise TypeError("chunk_size must be an int, it is instead a %s." % type(chunk_size))
    # simulate reading small chunks of the content
    reused_chunks = iter_slices(self._content, chunk_size)

    stream_chunks = generate()

    chunks = reused_chunks if self._content_consumed else stream_chunks

    if decode_unicode:
        chunks = stream_decode_response_unicode(chunks, self)

    return chunks


@app.route('/<path:u>', methods=['GET', 'POST'])
def proxy(u):
    u = u if u.startswith('http') else 'https://' + u
    u = u.replace(':/g', '://g', 1)  # uwsgi会将//传递为/
    if jsdelivr and exp2.match(u):
        u = u.replace('/blob/', '@', 1).replace('github.com', 'cdn.jsdelivr.net/gh', 1)
        return redirect(u)
    elif cnpmjs and exp3.match(u):
        u = u.replace('github.com', 'github.com.cnpmjs.org', 1) + request.url.replace(request.base_url, '', 1)
        return redirect(u)
    elif jsdelivr and exp4.match(u):
        u = re.sub(r'(\.com/.*?/.+?)/(.+?/)', r'\1@\2', u, 1)
        u = u.replace('raw.githubusercontent.com', 'cdn.jsdelivr.net/gh', 1)
        return redirect(u)
    else:
        if exp2.match(u):
            u = u.replace('/blob/', '/raw/', 1)
        headers = {}
        r_headers = {}
        for i in ['Range', 'User-Agent']:
            if i in request.headers:
                r_headers[i] = request.headers.get(i)
        r_headers['Accept-Encoding'] = request.headers.get('Accept-Encoding', 'identity')
        try:
            url = u + request.url.replace(request.base_url, '', 1)
            if url.startswith('https:/') and not url.startswith('https://'):
                url = 'https://' + url[7:]
            r = requests.request(method=request.method, url=url, data=request.data, headers=r_headers, stream=True)
            for i in ['Content-Range', 'Content-Type']:
                if i in r.headers:
                    headers[i] = r.headers.get(i)
            if r.status_code == 200:
                headers = dict(r.headers)
            try:
                headers.pop('Transfer-Encoding')
            except KeyError:
                pass

            if 'Content-length' in r.headers and int(r.headers['Content-length']) > size_limit:
                return redirect(u + request.url.replace(request.base_url, '', 1))

            def generate():
                for chunk in iter_content(r, chunk_size=CHUNK_SIZE):
                    yield chunk

            return Response(generate(), headers=headers, status=r.status_code)
        except Exception as e:
            headers['content-type'] = 'text/html; charset=UTF-8'
            return Response('server error ' + str(e), status=500, headers=headers)
    # else:
    #     return Response('Illegal input', status=403, mimetype='text/html; charset=UTF-8')


if __name__ == '__main__':
    app.run(host=HOST, port=PORT)
```

### 通过FastGit

#### 基础使用

各命令替换如下。

对于`https://raw.githubusercontent.com/`，可替换为`https://raw.fastgit.org/`。

```
# 克隆
# 假设仓库为https://github.com/author/repo
git clone https://hub.fastgit.org/author/repo

# Release
# 假设下载链接为https://github.com/A/A/releases/download/1.0/1.0.tar.gz
wget https://download.fastgit.org/A/A/releases/download/1.0/1.0.tar.gz

# Codeload
# 假设下载链接为 https://hub.fastgit.org/A/A/archive/master.zip
# 或者 https://codeload.github.com/A/A/zip/master
wget https://download.fastgit.org/A/A/archive/master.zip
```

#### fgit

可通过安装fgit，在克隆项目时直接完成链接替换。下载以下项目并解压。

```
https://github.com/FastGitORG/fgit
```

完成解压后，对于Linux，终端切换到解压好的目录后执行以下命令。

```
sudo cp ./bin/fgit.sh /usr/local/bin
```

对于Windows则将fgit/scripts加入到环境变量(PATH)中。

完成后即可在终端通过以下命令克隆项目。

```
fgit https://github.com/author/repo
```

#### 油猴脚本

Fastgit油猴脚本链接如下。

```
https://greasyfork.org/zh-CN/scripts/402301-fastgit
```

### 通过Microsoft Azure Notebooks

打开以下链接并登录微软账号。新建一个notebook环境并预装Git，然后使用git或者wget命令将Github资源下载到微软服务器，再从微软下载。

```
https://notebooks.azure.com/
```

### 通过代理

可通过以下命令设置代理。

```
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
```

可通过以下命令取消。

```
git config --global --unset https.proxy 'socks5://127.0.0.1:1080'
git config --global --unset http.proxy 'socks5://127.0.0.1:1080'
```

### 通过hosts

#### 自动修改

可使用UsbEAm Hosts Editor修改，下载链接如下。

```
https://www.dogfight360.com/blog/475/
```

#### 手动修改

##### 查询IP

通过以下网站查询`global-ssl.fastly.Net`和`github.com`的公网地址，选择一个稳定且延迟较低的IP并记录。注意不要直接在终端ping该网址，因为经常会发生DNS污染。

```
http://tool.chinaz.com/dns/
https://www.ipaddress.com/
```

也可通过以下链接直接获取。

```
https://github.com/ineo6/hosts
```

##### 添加配置

###### 通过直接修改

将以下代码添加到`hosts`文件，其中xxx部分替换为上面所查到的IP地址。

```
// Windows下hosts文件位于C:\Windows\System32\drivers\etc\hosts
// Mac/Linux下hosts文件位于/etc/hosts

xxx.xxx.xxx.xxx github.com
xxx.xxx.xxx.xxx github.global.ssl.fastly.net  
```

重启浏览器，或在终端（命令行）输入以下命令以刷新DNS缓存即可。

```
// Linux/Mac
sudo /etc/init.d/networking restart

// Windows
ipconfig /flushdns
```

###### 通过SwitchHosts!

下载地址如下。在My Hosts中填写上面查询到的IP和域名，然后开启该配置文件即可。

```
https://github.com/oldj/SwitchHosts
```

也可通过订阅hosts规则的方式，避免手动更新域名和IP。点击+号，Type选择Remote，URL如下，Auto Refresh建议选择1 hour。

```
https://cdn.jsdelivr.net/gh/ineo6/hosts/hosts
```

# Github

## 分支管理

### 新建分支

在仓库中点击Branch，在搜索框中输入要新建的分支的名字，系统会自动弹出Create branch的窗口，点击即可。

### 设置默认分支

在仓库中点击Settings，在侧栏选择Branches，选择新的默认分支即可。

### Pull Request

即PR。部分仓库不允许直接push，需要通过Pull Request提交更新。对代码进行修改后通知仓库作者并请求作者合并自己的修改，即完成一次Pull Request。

在Github上fork项目，并进行修改。切换到需要PR的分支，点击New pull request即可。

## Github Action

### 准备工作

打开Github，点击Settings-Seveloper settings-Personal access tokens，选择`Generate new token`，勾选`repo`，`admin:repo_hook`，`workflow`后点击`Generate token`即可。

完成Github Action的设置后，可通过随意commit或点击`Star`以激活功能。

### 可用项目

#### 天翼云盘自动签到

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

## 获取Token

网页登录github账号后，点击Settings-Developer settings-Personal access tokens，点击Generate Token即可。注意生成后需马上复制该Token，否则第二次将无法复制。

## 仓库收集

### chinese-independent-developer

开源项目合集。

```
https://github.com/1c7/chinese-independent-developer
```

### SingleFile

将网页保存为HTML文件。

```
https://github.com/gildas-lormeau/SingleFile
```

### Handright

模拟中文手写的轻量级Python库。

```
https://github.com/Gsllchb/Handright
```

### CC-attack

进行DDOS攻击。

```
https://github.com/Leeon123/CC-attack
```

### manim

3b1b使用的数学绘图库。

```
https://github.com/3b1b/manim
```

### wkhtmltopdf

将HTML渲染为PDF和各种图像格式的命令行工具。

```
https://github.com/wkhtmltopdf/wkhtmltopdf
https://wkhtmltopdf.org/
```

### RW_Password

提取收集以往泄露的密码中符合条件的强弱密码。

```
https://github.com/r35tart/RW_Password
```

### raspberry-pi-sync

在树莓派上安装Resilio Sync。

```
https://github.com/willjasen/raspberry-pi-sync
```

### WechatMomentScreenshot

朋友圈转发截图生成工具。

```
https://akarin.dev/WechatMomentScreenshot/
https://github.com/TransparentLC/WechatMomentScreenshot
```

### croc

任意两台电脑互传文件。

```
https://github.com/schollz/croc
```

# Github Gist

随时编写代码并记录。可设为仅自己可见即secret，或全体可见即public。

```
https://gist.github.com/
```

可通过Lepton管理自己的Github Gist。

```
https://github.com/hackjutsu/Lepton
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

## Git Large File Storage

```
https://git-lfs.github.com/
```

## .gitignore

```
https://www.jianshu.com/p/699ed86028c2
```

## hunshcn/gh-proxy

```
https://github.com/hunshcn/gh-proxy
```

## FastGit简体中文指南

```
https://doc.fastgit.org/zh-cn/
```

## 如何提高国内访问Github的速度到2MB/s以上 | 评布客博客

```
https://code.seniortesting.club/blog/2020/How-To-Speed-Github.html
```

## Learn Git Branching

```
https://learngitbranching.js.org/?locale=zh_CN
```

## Pull Request 的命令行管理

```
http://www.ruanyifeng.com/blog/2017/07/pull_request.html
```

## 如何解决类似 curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused 的问题 #10

```
https://github.com/hawtim/blog/issues/10
```
