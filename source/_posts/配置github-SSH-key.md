---
title: 配置github SSH key
date: 2019-04-02 15:19:37
tags:
  - SSH key
---

> 查看.ssh\id_rsa.pub 文件是否存在，若存在直接复制即可，若不存在则需重新生成(windows 下在 C:/Users/用户名/.ssh 目录中)

```
$ cd ~/.ssh
```

> 生成 SSH key

```
ssh-keygen -t rsa -C "邮件地址"
```

连续回车后，找到.ssh\id_rsa.pub 文件，打开并复制里面的内容

<!--more-->

> 进入自己的 github，点击 Settings

{% asset_img avatar-settings.png %}

> 选择 SSH and GPG keys---->新增 SSH key--->把刚刚复制的内容填写到 key 中，title 随便填，保存即可

{% asset_img ssh.png %}

> 测试是否成功

```
$  ssh -T git@github.com

Warning: Permanently added the RSA host key for IP address '192.30.255.112' to the list of known hosts.
Hi purplejiang! You've successfully authenticated, but GitHub does not provide shell access.
```

此时 SSH 已经配置成功，虽然有个 warning，但是不影响使用。但是有个警告对于有强迫症的人来说实在不爽，下面我们就来消除这个
warning

Warning 告诉我们要为 IP 为'192.30.255.112'的主机（RSA 连接的）持久添加到 hosts 文件中，那么我们就打开 hosts 文件添加一行
即可消除警告

```
mac中hosts文件地址：/private/etc/host

192.30.255.112 github.com
```

> 最后配置

```
$ git config --global user.name "liuxianan" // 你的github用户名
$ git config --global user.email  "xxx@qq.com" //填写你的github注册邮箱
```
