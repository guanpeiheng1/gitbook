# Node环境的搭建与使用

## 安装

### 通过nvm安装（推荐）

仓库如下。

```text
https://github.com/nvm-sh/nvm
```

在终端输入以下命令安装`nvm`。

```text
// bash用户
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash

// zsh用户
curl -o- [https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh](https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh) | bash [[ -s $HOME/.nvm/nvm.sh ]] && . $HOME/.nvm/nvm.sh
```

安装完成后在终端输入下列命令以安装`node`。

```text
// 安装长期支持版本（建议）
nvm install --lts

// 安装最新版本
nvm install node
```

### 通过官方安装包安装

打开下面官方网站，下载安装包进行安装。

```text
https://nodejs.org/en/
```

## 使用

### 更换下载源

#### 直接更换为淘宝源

在终端执行以下命令。

```text
npm config set -g registry https://registry.npm.taobao.org
```

#### 用nrm切换下载源

在终端执行以下命令安装`nrm`，并查看下载镜像源。

```text
npm install -g nrm
nrm ls
```

用以下命令切换淘宝源。

```text
nrm use taobao
```

### 更新主程序

#### 对于nvm安装

```text
nvm install lts/* --reinstall-packages-from=default --latest-npm
```

#### 对于独立程序

```text
npm install -g npm
npm install -g node
```

### 更新下载包

在终端执行以下命令安装`ncu`并更新依赖包到最新版本。

```text
npm install -g npm-check-updates
ncu -u
```

### 查看安装过的模块

```text
// 查看当前项目的依赖模块
npm ls --depth 0

// 查看全局依赖模块命令
npm ls -g --depth 0
```

### 删除项目所有依赖

```text
npm cache verify
// 或npm cache clean --force
```

### 删除项目所有依赖

```text
npm uninstall *
```

## 卸载

### 卸载nvm

在终端输入以下命令，移除nvm。

```text
cd ~
rm -rf .nvm
```

移除掉`~/.profile`，`~/.bash_profile`， `~/.zshrc`，`~/.bashrc`文件中关于nvm的配置后输入以下命令，让配置文件生效。

```text
source ~/.zshrc
source ~/.bash_profile
```

### 卸载node和npm

在终端执行以下命令以卸载。

```text
sudo npm uninstall npm -g
sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*
sudo rm -rf /usr/local/include/node /Users/$USER/.npm
sudo rm /usr/local/bin/node
sudo rm /usr/local/share/man/man1/node.1
sudo rm /usr/local/lib/dtrace/node.d
```

输入以下命令以确认卸载是否已完成。

```text
node
npm
```

## 常见问题

### 提示package.json不存在

package.json不存在时，可运行以下命令自动创建。

```text
npm init
```

package.json存在时，运行以下命令以将package.json的模块安装到node-modules文件夹下。

```text
npm install
// 或 npm install –save-dev
```

打开package.json，在最后大括号前添加以下语句，保存并退出。该语句可修复`warning no description / no repository field`的错误。

```text
"private": true
```

### 提示找不到&lt;CoreFoundation/CoreFoundation.h&gt;或其它库文件

在终端输入以下命令即可。

```text
csrutil disable
sudo mount -uw /
sudo ln -s "$(xcrun --show-sdk-path)/usr/include" /usr/include
export SDKROOT="$(xcrun --show-sdk-path)"
echo "export SDKROOT=\"\$(xcrun --show-sdk-path)\"" >> ~/.bash_profile
sudo DevToolsSecurity -enable
```

