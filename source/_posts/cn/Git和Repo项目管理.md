---
layout: post
title: 'Git和Repo项目管理'
date: 2022-07-03
author: Futari
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: Git

---

#### Git提交问题

######  git commit 时可以加 -f代表强制提交，因为对于某些设置的文件是可忽略的，如果要强行提交的话，可以使用上面的命令。

```bash
git commit -f #强制commit
```

###### 忽略提交的文件：比如某些重要的配置文件、或者是测试文件不想被提交的情况。

Ps：只有.git目录以及他的下层目录才能代表这是一个git的仓库，git目录以及他的下层目录才是可以被识别的，不然凭空的话只会报下面的错误（.git这是一个隐藏文件夹）

```bash
$ git log
fatal: not a git repository (or any of the parent directories): .git
```

忽略提交的文件在.idea的IDEA项目工程文件夹中或者是仓库文件夹中，名为<font color = 'red' >.gitignore</font>,格式如下;

（当然不同的地方管理范围不同，idea可以管理IDEA中的提交功能，而仓库中的忽略文件更精确）

![LIHOP.png](https://s1.328888.xyz/2022/07/13/LIHOP.png)

### Repo快速理解

Repo的基本操作先不说了，Repo其实就是一个python写的Git命令脚本，去管理Git的多个库，通过拉取项目管理文件也就是mainfest.xml来获取项目，mainfest.xml维护了源代码结构

> 查看Repo（不同的Repo脚本支持的操作也不一样，可以自己从网上下载，或者自己项目组编写）源代码可以看到，通过获取命令字符，去匹配相对应的函数进行操作，本质上还是到某某仓库去拉取代码，只不过用Repo去建立了多个仓库之间的联系

##### Repo基于Gerrit的原理（我观察是通过Group组的权限管理来控制拉取的）

比如init会去相对应的{project}去同步代码，比如这里的**pub**

![Lg3Gw.png](https://s1.328888.xyz/2022/07/14/Lg3Gw.png)

