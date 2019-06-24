---
title: 在mac上搭建开发环境
date: 2019-06-10 19:45:12
tags:
  - Homebrew
  - item2
---

### 1. Homebrew

Homebrew 是一款 Mac OS 平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以
实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。[官网](https://brew.sh/)

> 前置

- Xcode 命令行工具

```
xcode-select --install
```

- 支持 shell (sh 或者 bash)

> 安装 Homebrew,

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

<!--more-->

出现如下界面则代表安装成功

{% asset_img brew-success.png %}

> 使用

- 安装包

```
brew install <packageName>
```

eg: brew install node、brew install mysql

- 卸载包

```
brew uninstall <packageName>
```

- 查询

```
brew search <packageName>
```

- 查看已安装包列表

```
brew list
```

- 查看当前版本

```
brew -v
```

- 更新

```
brew update
```

> Homebrew 下载更新太慢

- 1.下载官网脚本

```
curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install >> brew_install
```

- 2.下载完毕后在该目录下修改 brew_install

这里替换成清华大学的镜像，也可以改为其他地址

```
open brew_install

#注释以下两句
#BREW_REPO = “https://github.com/Homebrew/brew“.freeze
#CORE_TAP_REPO = “https://github.com/Homebrew/homebrew-core“.freeze

#修改为以下两句,保存并退出
BREW_REPO = “https://mirrors.ustc.edu.cn/brew.git “.freeze
CORE_TAP_REPO = “https://mirrors.ustc.edu.cn/homebrew-core.git“.freeze
```

- 3.执行该文件

```
/usr/bin/ruby brew_install
```

### 2. Homebrew Cask

brew-cask 允许你使用命令行安装 OS X 应用。几乎所有常用的应用都可以通过 brew-cask（Sublime Text、VirtualBox） 安装，而且
是从应用的官网上下载，所以你要安装新的应用时，建议用 brew-cask 安装。

```
eg: brew cask install google-chrome //安装goole
```

如果你不知道应用在 brew-cask 中的 ID，可以先用 brew search 命令搜索， 比如： brew search google

### 3. iterm2

> 使用 Homebrew 安装 iterm2

```
brew cask install iterm2
```

> 替换默认背景图片

iTerm2 -> Preferences -> Profiles-> window -> Background Image

> 打开新的窗口/标签页的时候，默认情况下新窗口总是 HOME 目录，此时需要每次敲命令才能进入工作目录。如果想要这个新窗口在打
> 开的时候就自动进入工作目录，需要如下设置

iTerm2 -> Preferences -> Profiles-> General -> Working Directory -> 选择 Reuse previous seesion's directory

> 常用快捷键

- Tab 纵向分割：⌘+d Tab
- 横向分割：⌘+shift+d
- 切换 Tab 中的 pane：⌘ + [ 或者 ⌘+ opt + arrow
- 关闭 panel：⌘ + w
- 最大化 Tab 中的 pane，隐藏本 Tab 中的其他 pane：⌘+ shift +enter , 再次还原
- 新建 Tab ：⌘ + t
- Tab 切换：⌘ + arrow 或者 ⌘+shift + [
- 改变 Tab 的顺序：⌘ + shift + arrow
- 快速切换到 Tab 上：⌘ + Num
- 最大化 Tab ： ⌘ + enter 再次还原
- 窗口太多，可以使用 ⌘ + / 快速定位到光标所在位置
- 一屏显示所有窗口：⌘ + alt+ e
- 设置标记：⌘ + shift + m
- 跳转到上个标记：⌘ + shift + j
- 多个标记切换：⌘ + shift + arrow
- 进入回放：⌘ + opt + b
- 方向键控制时间 ：arrow
- 退出回放：esc
- 查找：⌘ + f

### 4. oh my zsh

默认的 Bash 是黑白的，没有色彩。而 Oh My Zsh 可以带你进入彩色时代。Oh My Zsh 同时提供一套插件和工具，可以简化命令行操作
。

> 安装,[github 地址](https://github.com/robbyrussell/oh-my-zsh)

```
# curl 安装方式
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

```
# wegt 安装方式
sh -c "$(wget -O- https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

> 卸载 oh-my-zsh

```
uninstall_oh_my_zsh
```

> 安装 zsh-syntax-highlighting

平常用的 ls、cd  等命令输入正确会绿色高亮显示，输入错误会显示其他的颜色。

```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting


vi ~/.zshrc

# plugin中加上zsh-syntax-highlighting
plugins = (
  git
  zsh-syntax-highlighting
)

# 改动生效
source ~/.zshrc
```

> 安装 zsh-autosuggestions

输入命令时，会给出建议的命令（灰色部分）,按键盘 → 补全

```
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions

vi ~/.zshrc

# plugin中加上zsh-autosuggestions
plugins = (
  git
  zsh-autosuggestions
)

# 改动生效
source ~/.zshrc
```

> shell 相关命令

查看系统当前使用的 shell

```
$ echo $SHELL
/bin/bash
```

查看系统是否安装了 zsh

```
$ cat /etc/shells

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```

切换为 zsh

```
chsh -s /bin/zsh
```

> 安装 oh my zsh 后导致已安装的 nvm 不可用

```
nvm

zsh: command not found: nvm
```

此时只需重新安装 nvm 即可，见博客[node 版本管理工具 nvm 的安装及使用](../../../04/10/node版本管理工具nvm的安装及使用)

note: 若安装后当前 Tab 下 nvm 可用，但是使用 ⌘ + t 新建的 Tab(或通过其他方式新建的 Tab)下 nvm 不可用，此时需要注意配置文件的配置，由于此时使用的终端是 zsh，故需在～/.zshrc 文件新增配置代码（终端 bash 的配置文件是～/.bash_profiles）

### 5. git

> 使用 Homebrew 安装 git

```
brew install git
```

测试是否安装正常

```
which git

/usr/local/bin/git
```

设置用户信息

```
$ git config --global user.name "liuxianan" // 你的github用户名
$ git config --global user.email  "xxx@qq.com" //填写你的github注册邮箱
```

iterm2 自带 git 插件，可使用一些简写命令进行 git 操作

- gb --------- git branch
- ga . --------- git add .
- gcmsg --------- git commit -m
- gp --------- git push
- gst --------- git status
- gd --------- git diff
- gcm --------- git checkout master
- gco --------- git checkout

> git 命令行中的中、英文切换

切换成英文

```
普通的命令行
echo "alias git='LANG=en_GB git'" >> ~/.bashrc

安装了zsh的命令行
echo "alias git='LANG=en_GB git'" >> ~/.zshrc
```

切换成中文

```
普通的命令行
echo "alias git='LANG=zh_CN git'" >> ~/.bashrc

安装了zsh的命令行
echo "alias git='LANG=zh_CN git'" >> ~/.zshrc
```

### 6. vs code

> 通过命令行打开项目

- 在 vscode 界面中 ⌘ + shift+p 打开控制面板
- 搜索【Shell 命令：在 PATH 中安装"code"命令】,并选择
- cd 到目录

```
cd test
```

- 打开

```
code .
```
