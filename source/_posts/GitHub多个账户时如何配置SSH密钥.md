---
title: GitHub多个账户时如何配置SSH密钥
date: 2021-10-09 22:20:51
tags: 博客,github,ssh key,git
---

# GitHub多个账户时如何配置SSH密钥

在工作中，可能会有企业帐号，个人帐号等github帐号需要同时配置ssh key，以便能进行认证并成功拉取代码。本文将介绍如何进行相关配置。

如何生成ssh key 见github指南：<https://docs.github.com/cn/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent>

```
生成指定名称ssh key命令：
ssh-keygen -t rsa -f ~/.ssh/id_rsa.key2 -C "email"
```

在 ~/.ssh 目录下新建一个config文件，添加如下内容（其中Host和HostName填写git服务器的域名，IdentityFile指定私钥的路径）

```
# github1
Host github1.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa

# github2
Host github2.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa.key2
```

当然，在本地生成好之后要把ssh加到github帐号SSH配置中，这里不再赘述。

测试命令：

```
测试key1：ssh -T git@github1.com
测试key2：ssh -T git@github2.com
测试命令成功时会返回ssh key对应的帐号信息
```

在真实项目中使用：
```
例：git@github.com:watermelon-ping/watermelon-ping.github.io.git
github1中的项目真实使用：git@github1.com:watermelon-ping/watermelon-ping.github.io.git
github2中的项目真实使用：git@github2.com:watermelon-ping/watermelon-ping.github.io.git
```

有用的命令：
```
git remote set-url origin remote-url
```

综述：

通过域名别名实现对github的多账户控制
