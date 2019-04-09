---
title: React项目中使用ESlint
date: 2019-04-09 19:45:12
tags:
  - react
  - ESlint
---

> 安装 eslint

```
npm i eslint --save-dev
```

> 新建 eslint 的配置文件 .eslintrc.js

```
module.exports = {

}
```

<!--more-->

> 限定使用范围。在.eslintrc.js 文件中配置 root

```
{
    "root": true
}
```

默认情况下，ESLint 会在所有父级目录里寻找配置文件，一直到根目录。为了将 ESLint 限制到一个特定的项目，我们就需要在该目录
中添加上面的配置项。ESLint 一旦发现配置文件中有 "root": true，它就会停止在父级目录中寻找。

> 安装 eslint-plugin-react，用于识别 react 中的一些语法检验

[eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react)

```
npm install eslint-plugin-react
```

增加配置 extends

```
{
  "extends": [
    "eslint:recommended",
    "plugin:react/recommended"
  ]
}
```

增加配置 plugins

```
{
  "plugins": [
    "react"
  ]
}
```

> 安装 babel-eslint

[babel-eslint](https://github.com/babel/babel-eslint)，用于识别 es6 的新语法

```
npm install babel-eslint --save-dev
```

在 .eslintrc.js 配置文件中添加如下配置项代码：

```
parser: 'babel-eslint' // eslint的解析器
```

> 添加 rules 配置项

ESLint 附带有大量的规则。你可以使用注释或配置文件修改你项目中要使用的规则。要改变一个规则设置，你必须将规则 ID 设置为下
列值之一：

- "off" 或 0 - 关闭规则
- "warn" 或 1 - 开启规则，使用警告级别的错误：warn (不会导致程序退出)
- "error" 或 2 - 开启规则，使用错误级别的错误：error (当被触发的时候，程序会退出)

rules 配置详见 [ESlint 官网 rule](https://cn.eslint.org/docs/rules/)

> .eslint.js 部分配置

```
module.exports={
  root: true,
  parser: 'babel-eslint', // 指定eslint的解析器
  parserOptions: { // 设置解析器选项
    ecmaFeatures: {
      jsx: true,
      modules: true
    }
  },
  env: { // 环境
    browser: true,
    node: true,
    es6: true
  },
  extends: ['eslint:recommended', 'plugin:react/recommended'],// 指定eslint规范
  plugins: ['react'],
  rules: {
    ...
    quotes: ['error', 'single'],
    semi: ['error', 'always'],
    ...
    // react配置
    'react/prop-types': 0,
    'react/no-unescaped-entities': 0
  }
};
```

> eslint 配置项解释

```
root 限定配置文件的使用范围
parser 指定eslint的解析器
parserOptions 设置解析器选项
extends 指定eslint规范
plugins 引用第三方的插件，使用 plugins 关键字来存放插件名字的列表
env   指定环境，使用 env 关键字指定你想启用的环境，并设置它们为 true
rules 启用额外的规则或覆盖默认的规则
globals 声明在代码中的自定义全局变量
```
