---
title: react + webpack项目搭建
date: 2019-05-05 19:45:12
tags:
  - webpack
  - react
---

### 创建项目并初始化

```
mkdir webpack-react //新建项目文件夹
cd webpack-react
npm init -y //生成默认package.json文件

```

### 安装 react 相关库

```
npm i react react-dom --save
```

根目录下新建 src 目录，用于存放源代码，src 目录下新建文件 index.html、index.js

<!--more-->

index.html 作为我们的模板文件

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

index.js 作为我们的入口文件

```
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import App from './App';

class App extends Component{
  render(){
    return <div>app</div>
  }
}

ReactDOM.render(<App />, document.getElementById('root'));
```

### 本地安装 webpack 相关插件

```
npm i webpack webpack-cli webpack-dev-server webpack-merge --save-dev
```

- webpack-dev-server 用来启动一个本地服务并动态编译的
- webpack-merge 用于区分 dev 和 prod 环境，它可以合并配置文件

在根目录下新建文件 webpack.common.js、webpack.dev.js、webpack.prod.js

webpack.common.js 公共配置文件

```
const path = require('path');
const webpack = require('webpack');
module.exports={

}
```

webpack.dev.js dev 环境下的配置文件

```
const common = require('./webpack.common');
const merge = require('webpack-merge');
const webpack = require('webpack');
const path = require('path');

module.exports = merge(common, {
  plugins: [
    new webpack.DefinePlugin({
      NODE_ENV: JSON.stringify(process.env.NODE_ENV)
    })
  ],
  mode: 'development'
});

```

webpack.dev.js prod 环境下的配置文件

```
const common = require('./webpack.common');
const merge = require('webpack-merge');
const webpack = require('webpack');

module.exports = merge(common, {
  plugins: [
    new webpack.DefinePlugin({
      NODE_ENV: JSON.stringify(process.env.NODE_ENV)
    })
  ],
  mode: 'production'
});
```

### 配置入口和出口

修改 webpack.common.js

```
module.exports = {
  entry: { index: path.resolve(__dirname, 'src/index.js') },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].[hash].js',
    publicPath: '/',
    chunkFilename: '[name].[hash].chunk.js'
  }
}
```

#### 安装 babel 相关插件

babel-loader、@babel/core、@babel/preset-react、@babel/preset-env 等为必须

```
npm i babel-loader @babel/core @babel/preset-react @babel/preset-env --save-dev
```

- babel-loader 处理 ES6 或者 React 的 loader

- @babel/core babel 核心模块

- @babel/preset-react 解析 jsx 语法

- @babel/preset-env 解析 es6 语法

（note：如果 babel-loader 是 8.0.0 之后的版本，需要安装指定版本的 @babel/core 、@babel/preset-react 与
@babel/preset-env,如果 babel-loader 是 8.0.0 之后的版本，则安装 babel-core、babel-preset-react、babel-preset-env。babel7
开始包名以'@'符号开头）

在根目录下新建.babelrc 文件，并对其进行如下配置

```
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

其他 babel 插件，根据需要安装

- @babel/plugin-proposal-class-properties 处理箭头函数

修改.babelrc 配置

```
{
  "plugins":[
    ...
    "@babel/plugin-proposal-class-properties"
    ...
  ]
}
```

- @babel/plugin-proposal-decorators 处理装饰器

```
{
  "plugins":[
    ...
    ["@babel/plugin-proposal-decorators", { "legacy": true }]
    ...
  ]
}
```

- @babel/plugin-transform-runtime 处理 ES7 的 async/await

此插件可解决使用 ES7 的 async/await 时报如下错的问题

```
Uncaught ReferenceError: regeneratorRuntime is not defined
```

修改.babelrc 配置

```
{
  "plugins": [
    ...
    "@babel/plugin-transform-runtime",
    ...
  ]
}
```

### 配置 loader

> js、jsx 相关

```
// webapck.common.js

module.exports={
 ...
 module:{
   rules:[
     ...
     {
       test: /\.(js|jsx)$/,
       use: 'babel-loader',
       exclude: /node_modules/
     },
     ...
   ]
 }
 ...
}
```

> 样式相关

```
// webapck.common.js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const devMode = process.env.NODE_ENV === 'developement';
module.exports={
  ...
  module:{
    rules:[
      ...
      {
        test: /\.(css)$/,
        use: [
          devMode ? 'style-loader' : MiniCssExtractPlugin.loader, //style-loader把css文件变成style标签插入到head中
          {
            loader: 'css-loader' //用来处理css中的url路径
          },
          {
            loader: 'postcss-loader'
          }
        ],
        exclude: /node_modules/
      },
      ...
   ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      // JS中的CSS -> 单独的文件中
      filename: '[id].[contenthash:12].css',
      chunkFilename: '[id].[contenthash:12].css'
    }),
  ]
  ...
}

// postcss.config.js
module.exports = {
  plugin: [require('autoprefix')()]
};

```

### 配置 html-webpack-plugin

```
module.exports = {
  plugins:[
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, 'src/index.html'),
      inject: 'body',
      hash: true, //防止缓存
      minify: {
        removeAttributeQuotes: true //压缩  去掉引号
      }
    }),
  ]
}
```

### 配置 clean-webpack-plugin

```
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  plugins:[
    new CleanWebpackPlugin(),
  ]
}
```

note: 最新版本的 clean-webpack-plugin，不能直接 const CleanWebpackPlugin 引入

### 配置 webpack-dev-server

```
// webpack.dev.js

module.exports = {
  devServer: {
    contentBase: path.resolve(__dirname, 'src'),
    port: '8088',
    host: '0.0.0.0',
    compress: true, //服务器返回浏览器的时候是否启动gzip压缩
    hot: true,
    historyApiFallback: true // 该选项的作用所有的404都连接到index.html
  },
}
```

### 运行

修改 pakage.json 文件

```
{
  "scripts": {
    "dev": "NODE_ENV=development webpack-dev-server --config webpack.dev.js",
    "build": "webpack --config webpack.prod.js"
  },
}
```

此时即可通过 npm run dev 打开一个 8088 的本地服务、进行本地开发了,使用 npm run build 进行打包

### 模块拆分

splitChunks.chunks 可能的值：initial、async、 all。

默认为 async，表示只会提取异步加载模块的公共代码，initial 表示只会提取初始入口模块的公共代码，all 表示同时提取前两者的代
码。

### antd 的按需加载

安装 babel-plugin-import 插件

```

npm i babel-plugin-import --save-dev

```

.babelrc 文件中加入如下配置

```

{ ... "plugins": [ ... [ "import", { "libraryName": "antd", "style": true // style: true | 'css' (其中 style: true
会加载 less 文件) } ] ... ] }

```

使用

```

import {Button} from 'antd'

<Button type="primary">按钮</Button>

```

### echarts 按需加载

- 方法 1

添加一个 echarts 配置文件

echart.js

```

// 加载 echarts，注意引入文件的路径 import echarts from 'echarts/lib/echarts'

// 再引入你需要使用的图表类型，标题，提示信息等

import 'echarts/lib/chart/bar' import 'echarts/lib/component/legend'

// 引入折线图 require('echarts/lib/chart/line'); // 引入提示框和标题组件 require('echarts/lib/component/title');

export default echarts

```

在需要的组件中引入你定义的文件即可

```

import echarts from '你的配置文件地址路径/echarts'

let myChart = echarts.init(document.getElementById('linechart')); myChart.setOption({ ... })

```

- 方法 2

安装 babel-plugin-equire

```

npm install babel-plugin-equire -save-dev

```

修改.babelrc 文件

```

{ "plugins": [ ... "equire" ... ] }

```

echart.js

```

// eslint-disable-next-line const echarts = equire([ // 写上你需要的 'bar', 'legend', 'title' ]) export default echarts

```

### 相关报错

> 报错 bezierEasingMixin

```

// https://github.com/ant-design/ant-motion/issues/44 .bezierEasingMixin(); ^ Inline JavaScript is not enabled. Is it
set in your options?

```

如下图

{% asset_img less-error.png %}

解决方案

开启 JavaScript 就可以了

```

{ loader: "less-loader", options: { javascriptEnabled: true } }

```

> 使用 eslint 时报如下警告

```

Warning: React version not specified in eslint-plugin-react settings. See
https://github.com/yannickcr/eslint-plugin-react#configuration .

```

解决方案:在.eslintrc.js 中添加如下代码

```
module.exports = {
  ...
  settings: { react: { version: 'detect' } },
  ...
}
```

[详细配置](https://github.com/yannickcr/eslint-plugin-react#configuration)
