---
title: webpack-react
date: 2019-05-05 19:45:12
tags:
  - webpack
  - react
---

<!--more-->

> 模块拆分

splitChunks.chunks 可能的值：initial、async、 all。

默认为 async，表示只会提取异步加载模块的公共代码，initial 表示只会提取初始入口模块的公共代码，all 表示同时提取前两者的代
码。

> antd 的按需加载

安装 babel-plugin-import 插件

```
npm i babel-plugin-import --save-dev
```

.babelrc 文件中加入如下配置

```
{
  ...
  "plugins": [
    ...
    [
      "import", {
        "libraryName": "antd",
        "style": true       // style: true | 'css' (其中 style: true 会加载 less 文件)
      }
    ]
    ...
  ]
}
```

使用

```
import {Button} from 'antd'

<Button type="primary">按钮</Button>
```

> echarts 按需加载

- 方法 1

添加一个 echarts 配置文件

echart.js

```
// 加载echarts，注意引入文件的路径
import echarts from 'echarts/lib/echarts'

// 再引入你需要使用的图表类型，标题，提示信息等

import 'echarts/lib/chart/bar'
import 'echarts/lib/component/legend'

// 引入折线图
require('echarts/lib/chart/line');
// 引入提示框和标题组件
require('echarts/lib/component/title');

export default echarts

```

在需要的组件中引入你定义的文件即可

```
import echarts from '你的配置文件地址路径/echarts'

let myChart = echarts.init(document.getElementById('linechart'));
myChart.setOption({
  ...
})
```

- 方法 2

安装 babel-plugin-equire

```
npm install babel-plugin-equire -save-dev
```

修改.babelrc 文件

```
{
 "plugins": [
 	...
 	"equire"
  ...
 ]
}
```

echart.js

```
// eslint-disable-next-line
const echarts = equire([
  // 写上你需要的
  'bar',
  'legend',
  'title'
])
export default echarts

```

> 报错 bezierEasingMixin

```
// https://github.com/ant-design/ant-motion/issues/44
.bezierEasingMixin();
^
Inline JavaScript is not enabled. Is it set in your options?
```

如下图

{% asset_img less-error.png %}

解决方案

开启 JavaScript 就可以了

```
{
  loader: "less-loader",
  options: {
    javascriptEnabled: true
  }
}
```

> 使用 eslint 时报如下警告

```
Warning: React version not specified in eslint-plugin-react settings. See https://github.com/yannickcr/eslint-plugin-react#configuration .
```

解决方案:在.babelrc 中添加如下代码

```
{
  ...
  settings: {
    react: {
      version: 'detect'
    }
  },
  ...
}
```

[详细配置](https://github.com/yannickcr/eslint-plugin-react#configuration)
