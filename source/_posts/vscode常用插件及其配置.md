---
title: vscode常用插件及其配置
date: 2019-6-11 19:00:00
tags:
  - vscode
---

### emmet

> vscode 内置 emmet 功能，可以用在 html、jsx、css、sass、less 等文件上。但是默认没有开启。

command+shift+p 打开命令面板,选择>Preferences: Open Settings (JSON),在 settings.json 中添加如下行覆盖默认配置即可：

```
{
  "emmet.triggerExpansionOnTab": true,
}
```

> 查看默认配置

command+shift+p 打开命令面板,选择>Preferences: Open Raw Default Settings【首选项，打开默认配置(JSON)】, 即可打开
defaultSettings.json

<!--more-->

### Chinese (Simplified) Language Pack for Visual Studio Code

中文（简体）语言包,将界面转换为中文

### Atuo Rename Tag

适用于 JSX、Vue、HTML，修改标签名时，自动帮你完成头部和尾部闭合标签的同步修改,如下图

{% asset_img rename.png %}

### Auto Close Tag

适用于 JSX、Vue、HTML，在打开标签并且键入 </ 的时候，能自动补全要闭合的标签；

### beautify

格式化代码工具，美化 javascript，JSON，CSS，Sass，和 HTML 等代码。

### Sass

### Reactjs code snippets

### Prettier - Code formatter

### Color Highlight

### ESLint

### node-readme

### Settings Sync

用于同步你的 VSCode 用户配置,一台电脑配置好之后，其它的几台电脑都不用配置

> 1 在 vscode 中搜索扩展 Settings Sync，并安装

> 2 设置 Github Person Access Token

首先进入[https://github.com/settings/tokens](https://github.com/settings/tokens)，点击 generate new token syn
{% asset_img generate.png %}

填写 token 的描述并勾选 gist 选项，点击 Generate token，生成 access token(note: 找个地方保存 access token，后期会用到)

{% asset_img token.png %}

> 3 上传配置到 Github Gist

- 打开 vscode，command+shift+p,输入 sync，选择 Sync: Update / Upload Settings(上传设置)

- 输入第二步中保存的 access token

- 上传成功后控制台会打印出上传信息,保存 Github Token(也就是第二步中的 access token) 和 Github Gist，Github Token 相当于
  密码， Github Gist 是你保存在 Github Gist 的 id，以后下载配置都要使用到这两个信息

{% asset_img gist.png %}

> 4 配置新设备的 VSCode

安装好 Settings Sync 拓展， command + shift + p，输入 sync，点击 Sync: Download Settings(下载设置),输入 accken 和 gist
即可

> 5 重置 sync

command+p，输入>async,Sync: Reset Extension Settings(重置设置),此后上传和下载配置，需重新填写 token

### settings.json 部分配置

```
{
  // 启用后，按下 TAB 键，将展开 emmet 缩写
  "emmet.triggerExpansionOnTab": true,
  //显式弹出语法展开提示
  "emmet.showSuggestionsAsSnippets": true,
  //将语法展开提示在提示列表中置顶
  "editor.snippetSuggestions": "top",
  //emmet只显示标记语言和样式表的展开提示
  "emmet.showExpandedAbbreviation": "inMarkupAndStylesheetFilesOnly",
  // emmet能识别缩写语法的场景
  "emmet.includeLanguages": {
    "javascript": "javascriptreact",
    "vue-html": "html",
    "plaintext": "jade
  }

}
```
