---
title: node版本管理工具nvm的安装及使用
date: 2019-4-10 19:00:00
tags:
  - nvm
  - node
---

### nvm 是什么

[nvm](https://github.com/creationix/nvm/blob/master/README.md)：node 版本管理工具，一个 nvm 可以管理很多 node 版本和 npm
版本。

### 卸载已经安装到全局的 node、npm(可选步骤)

查看已经安装在全局的模块,卸载后可根据需要重装

```
npm ls -g --depth=0
```

删除全局 node_modules 目录

```
sudo rm -rf /usr/local/lib/node_modules
```

<!--more-->

删除 node

```
sudo rm /usr/local/bin/node
```

删除全局 node 模块注册的软链

```
cd  /usr/local/bin && ls -l | grep "../lib/node_modules/" | awk '{print $9}'| xargs rm
```

### 安装 nvm

> curl 安装

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
```

其中 v0.33.8 为版本，可更改为你要安装的版本，最新版本参照[github](https://github.com/nvm-sh/nvm)

> wget 安装

```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
```

安装完成后，使用 <font color=#ff0000>nvm</font> 命令查看是否安装成功，有输出则表示安装成功

若出现

```
 nvm: command not found
```

此时需要在<font color=#ff0000>.bash_profile</font> 文件中新增配置代码并保存,如果没有该文件则新增

- 进入当前用户的 home 目录

```
$ cd ~
```

- 新建.bash_profile 文件

```
$ touch .bash_profile
```

- 打开.bash_profile 文件

```
$ open .bash_profile
```

- 输入安装提示中代码并保存：

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
```

具体代码参照安装时的提示，上面代码为示例。如下图

{% asset_img nvm-install.png %}

- 运行命令,更新配置过的环境变量

```
source .bash_profile
```

此时运行 nvm -v 会出现如下界面，就代表 nvm 安装成功了

{% asset_img nvm-success.jpg %}

此时我们就可以根据自己的需要随意切换我们的 node 版本了～～～～～～～～

### nvm 的使用

- <font color=#ff0000>nvm ls</font> 列出所有安装的版本
- <font color=#ff0000>nvm install <version\></font> 安装指定版本
- <font color=#ff0000>nvm uninstall <version\></font> 删除已安装的指定版本
- <font color=#ff0000>nvm use <version\></font>切换使用指定的版本 node
- <font color=#ff0000>nvm current</font> 显示当前的版本
- <font color=#ff0000>npm ls</font> 列出所有安装的版本
- <font color=#ff0000>nvm ls-remote</font> 列出全部可以安装的版本号
