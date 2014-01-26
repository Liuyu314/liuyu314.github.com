---
layout: post
title: "Eclipse如何使用Git"
description: ""
category: "Git"
tags: [Git]
---
{% include JB/setup %}

版本控制（本文将以Git为例）对于开发人员的意义不言而喻，它使得我们管理代码时更加轻松，也方便了共通开发的团队。也许你在终端使用过Git（一句一句的输入命令），而Eclipse为我们同样提供了Git功能（Git插件），使得在集成环境下，我们仍然能够方便的使用版本控制。我之前写过一篇文章介绍Git——[git 简单用法](]http://liuyu314.github.io/unix/linux/2013/03/17/git-usage/)，本文将简要介绍Eclipse如何使用Git工具。

首先要先为Eclipse安装Git，Eclipse已经为我们安装好了Git，如果你的还没有安装，可以按照下面的步骤进行：点击Help -> Install New Software -> Add

<p><img src="/images/blogImgs/EGIT1.png" width='650' height='488'></p>

网址即： http://download.eclipse.org/egit/updates/ ，也可以点击Help中的Eclipse Marketplace查找GIT进行下载。

OK，假设你已经安装完毕。选择Preferences -> Team -> Git -> Configuration进行配置，点击New Entry，添加key和value，key添加user.name，value添加你的用户名即可。与上同理，再添加user.email。

配置完成后，随便创建一个新项目。右键选择工程项目，选择Team->Share Project。注意，网上有些文章说是在File中查找，但是我的Eclipse的File中并没有该选项。接下来要选择图中Use or create repository in parent folder of project。再勾选项目的复选框。可能你会发现无法勾选，那就点击下面的Create Repository为项目建立Git目录，即.git文件夹。此时再勾选项目即可。

<p><img src="/images/blogImgs/EGIT2.png"></p>

我们再次右键工程项目选中Team，就可以看到如下信息：

<p><img src="/images/blogImgs/EGIT3.png" width='650' height='395'></p>

这里包含很多Git的功能选项，如果你熟悉Git的用法，相信很多你都不会陌生，比如commit，push，pull等等。用法与终端下的Git相似，首先是将文件添加至暂存区进行追踪。commit将这些文件添加至Git仓库中。当我们要查看提交情况时，选择show in history即可。

至于merge、reset这些Git的基本功能，这里就不过多的介绍了。

如果希望能够把代码提交到Github上，需要SSH密匙，[这篇文章](http://tomyail.com/blog/935)对此有所介绍。

公欲善其事，必先利其器。作为一名程序员，版本控制是非常有用的工具，也是每个程序员都应该掌握的一门技术。
