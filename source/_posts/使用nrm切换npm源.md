---
title: 使用nrm切换npm源
date: 2019-4-10 20:00:00
tags:
  - nrm
  - npm
---

### nrm 是什么

nrm 是一个 npm 源管理器，允许你快速地在 npm 源间切换

### npm 切换使用源

查看 npm 源地址

```
npm config list
```

<!--more-->

修改 registry 地址

```
npm set registry https://registry.npm.taobao.org/
```

删除 registry 地址

```
npm config rm registry
```

### nvm 安装及使用

安装

```
npm install nrm -g
```

查看可选的源列表,带\*号即为当前使用的配置

```
nrm ls
```

{% asset_img nrmls.png %}

查看当前使用的是哪个源

```
nrm current
```

切到源http://r.cnpmjs.org/， 命令： nrm use <源的别名>，即

```
nrm use cnpm
```

用 nrm add 命令添加公司私有 npm 源,起个别名

```
nrm add <别名> <源地址>
```

测试速度

```
$ nrm test npm

npm ---- 1045ms
```

删除源

```
nrm del <别名>
```
