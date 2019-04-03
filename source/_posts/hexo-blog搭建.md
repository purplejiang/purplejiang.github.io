---
title: hexo blog搭建
date: 2019-04-02 10:45:12
tags:
---

#### 安装 hexo

```
$ npm i -g hexo
```

若安装速度过慢，可使用淘宝镜像源--registry=https://registry.npm.taobao.org

### hexo 搭建 blog

```
$ hexo init <name>
```

此时会完成 hexo 项目的初始化、并下载安装项目的依赖。name 可选，若无，则会在当前文件夹下初始化项目，如有则在 name 文件夹
下初始化项目

<!--more-->

{% asset_img mulu.jpg %}

在 public 目录下生成相关 html 文件

```
$ hexo g   //全称：hexo generate
```

启动本地预览服务

```
$ hexo s  //全称：hexo server
```

此时打开 http://localhost:4000 即可看到默认的 blog 项目(此时为 hexo 的默认主题)

{% asset_img init-blog.jpg %}

### 创建新 blog

```
$ hexo new 'hexo blog搭建'

INFO  Created: ~/JT/xwx/github/hexo/source/_posts/hexo-blog搭建.md
```

此时在/source/\_posts 文件下生成了 hexo-blog 搭建.md 文件，直接在该文件中编辑 blog 即可

它与 hexo new page 'my'的区别：

```
$ hexo new page "my"
INFO  Created: ~/JT/xwx/github/hexo/source/my/index.md
```

后者会生成在/public/my/index.html，不会作为文章出现在 blog 的目录中

### 主题更换

如果不喜欢 hexo 默认的主题，那么我们可以根据自己的喜好进行替换。hexo 的官方网站上有很多[主题](https://hexo.io/themes/)可
以进行选择，下面我们以 [Gradient](https://github.com/RandomAdversary/Gradient) 主题为例进行自己的主题替换

1. 首先 clone 该主题到本地 themes 目录下

```
$ git clone https://github.com/RandomAdversary/Gradient.git themes/gradient
```

此时根目录的 themes 文件下就会多一个名为 gradient 的文件夹，如图

{% asset_img theme-mulu.jpg %}

2. 然后修改根目录下的\_config.yml 文件

```
_config.yml

theme: gradient
```

3. 运行

```
$ hexo clean //清理public的内容
$ hexo g //重新生成
$ hexo s //发布
```

此时再打开 http://localhost:4000/ 就能看到我们的主题已经替换成功了

{% asset_img theme-gradient.jpg %}

### 手动控制 blog 摘要显示的内容

在 blog 文件需要的位置加上代码： <!--more--> 即可

![](/images/more.png)

此时效果如下：

![](/images/more-result.png)

### hexo blog 中添加图片、音乐、视频

1. 添加图片

- 外部图片

```
![“图片描述”](“图片地址”)
```

- 本地图片，source 目录下新建文件夹 images（也可以是其他名字），把图片存放在该目录下，引入

```
![“图片描述”](/images/图片名字.图片的后缀名)
```

- 当我们的 blog 只有少量图片，那最简单的方法就是将它们放在 source/images 文件夹中，当图片过多时，这种方式就不便于图片的
  管理，此时可以采用下面方式：

```
修改_config.yml文件

post_asset_folder: true
```

当资源文件管理功能打开后，Hexo 将会在你每一次通过 hexo new [layout] <title\> 命令创建新文章时自动创建一个同名文件夹。可
以通过常规的 markdown 语法和相对路径来引用它们，但是通过这种方式来引用图片和其它资源可能会导致它们在存档页或者主页上显示
不正确。为解决这个问题可采用下面的引用方式

```
{% asset_img example.jpg This is an example image %}
```

note: 详见[官网资源文件夹](https://hexo.io/zh-cn/docs/asset-folders)一章

2. 添加音乐

```
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86
	src="http://music.163.com/outchain/player?type=2&id=25706282&auto=0&height=66">
</iframe>
```

3. 添加视频

```
<iframe
	height=498 width=510
	src="http://player.youku.com/embed/XNjcyMDU4Njg0"
	frameborder=0 allowfullscreen>
</iframe>
```

### 发布上传 github

> 这一步的前提条件是已进行 SSH key 的配置，若还未进行配置，可参照[配置 github-SSH-key](../配置github-SSH-key)

> 安装 hexo-deployer-git,不然发布时会报错

{% asset_img d-error.png %}

```
npm install hexo-deployer-git --save
```

> 修改\_config.yml

```
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: master
```

> 生成并发布

```
$ hexo g
$ hexo d
```

此时即可通过https://username.github.io/查看了

由于发布的时候只会将生成的 public 文件夹下的静态文件上传到 github 的 master 分支，当我们切换电脑又该怎么写 blog 呢，所以
我们新建一个分支 hexo 用户维护我们的 blog 的源代码，这样我们可以很方便的在另一台电脑上进行编辑了。当切换电脑时，只需
clone hexo 分支上的代码，执行 npm i 安装项目依赖即可，安装完成后就可以进行 blog 的编辑、生成、发布了，注意当我们发布新的
blog 时，需更新一份源代码到 hexo 分支

### yilia 主题

> 使用步骤

1.下载

```
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

2.修改根目录的\_config.yml 文件

```
theme: yilia
```

> yilia 主题所有文章模块确实问题解决方案（content.json 404）

1.根目录下运行

```
npm i hexo-generator-json-content --save
```

2.在根目录的\_config.yml 文件中增加如下文件

```
jsonContent:
  meta: false
  pages: false
  posts:
    title: true
    date: true
    path: true
    text: false
    raw: false
    content: false
    slug: false
    updated: false
    comments: false
    link: false
    permalink: false
    excerpt: false
    categories: false
    tags: true
```

> 配置头像

修改 themes/yillia/\_config.ymal 文件

```
avatar: /images/avatar.jpg
```
