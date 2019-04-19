---
title: 如何发布一个简单的npm包
date: 2019-4-18 20:00:00
tags:
  - npm
---

### 注册 npm 账号

首先去[npm 官网](https://www.npmjs.com/)上[注册](https://www.npmjs.com/signup)npm 账号,Username、Password、Email 发布的
时候会使用到

{% asset_img signup.png %}

### 创建自己的项目

首先创建文件夹 npmtest，并进入目录

<!--more-->

```
mkdir npmtest && cd mpmtest
```

生成 package.json 文件，运行命令

```
npm init
```

如果你想创建的是指定域的包，那么运行

```
npm init --scope=@yourscopename
```

这里我选择了第二种方式

{% asset_img init.png %}

配置完毕后，开始写我们的自己的代码

npmtest/index.js

```
export const isEmpty = obj => {
  return obj == null || obj == undefined || obj == '';
};

export const sliceByIndex = (str, index) => {
  return str.slice(0, index);
};
```

### 发布

> 使用 nrm 切换我们的源为官方 npm 源，nrm 的使用详见另一篇文章[使用 nrm 切换 npm 源](../../10/使用nrm切换npm源)

{% asset_img nrm.png %}

> 使用我们注册的账号、密码、邮箱进行登录

{% asset_img login.png %}

> 使用 npm publish 发布

报错

{% asset_img error1.png %}

这应该是发布私有包权限的问题，这里我们改为发布 public 公有的包

```
npm publish --access public
```

{% asset_img error2.png %}

这是因为没有验证邮箱,验证邮箱之后再发布就可以成功了

{% asset_img succes.png %}

此时我们的包已经发布成功了,此时就能去 [npm 官网](https://www.npmjs.com/)搜索我们的包并进行使用了

### 更新 npm 包

当我们对我们的项目内容进行更改后，需要重新发布，此时记得修改 pakage.json 的版本号，再发布。如初始版本为 1.0.0，修改后如
下：

{% asset_img version.png %}

此时我们可以手动修改版本号，也可以运行下面的命令自动修改

```
npm version patch  #自动修改package.json文件版本号+1

npm publish
```

更多命令, 如初始版本为 1.0.0，执行相关类型命令后，对应的语意为：

```
npm version patch  // 1.0.1 表示小的bug修复
npm version minor // 1.1.0 表示新增一些小功能
npm version major // 2.0.0 表示大的版本或大升级
npm version preminor // 1.1.0-0 后面多了个0，表示预发布
```

### 使用我们的 npm 包

```
npm i @purplej/npmtest
```

```
import {sliceByIndex} from '@purplej/npmtest'

console.log(sliceByIndex('dedssss',3))
```

### 撤销 npm 包

由于我们这是一个测试的 npm 包，所以需要删除,那么我们如何撤销我们 npm 包呢，使用如下命令即可

```
npm --force unpublish @purplej/npmtest //删除要用force强制删除
```

{% asset_img delete.png %}

有人说超过 24 小时就不能删除了(也有说 72 小时的)，由于此次的目的是学习如何发布一个简单的 npm 包，这一步就没有进行验证了
