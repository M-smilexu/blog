---
title: "在mac下安装oh my zsh"
date: 2015-03-14 15:40:20
categories: [coding, zsh]
description: "目前常用的 Linux 系统和 OS X 系统的默认 Shell 都是 bash，但是真正强大的 Shell 是深藏不露的 zsh， 这货绝对是马车中的跑车，跑车中的飞行车，史称『终极 Shell』，但是由于配置过于复杂，所以初期无人问津，很多人跑过来看看 zsh 的配置指南，什么都不说转身就走了。直到有一天，国外有个穷极无聊的程序员开发出了一个能够让你快速上手的zsh项目，叫做「oh my zsh」，Github 网址是：https://github.com/robbyrussell/oh-my-zsh 这玩意就像「X天叫你学会 C++」系列，可以让你神功速成，而且是真的。"
---

可以通过以下命令查看你的mac系统有几种shell:

    cat /etc/shells

显示如下:

    /bin/bash
    /bin/csh
    /bin/ksh
    /bin/sh
    /bin/tcsh
    /bin/zsh


可以看到mac自带了zsh, 那什么是zsh?

>目前常用的 Linux 系统和 OS X 系统的默认 Shell 都是 bash，但是真正强大的 Shell 是深藏不露的 zsh， 这货绝对是马车中的跑车，跑车中的飞行车，史称『终极 Shell』，但是由于配置过于复杂，所以初期无人问津，很多人跑过来看看 zsh 的配置指南，什么都不说转身就走了。直到有一天，国外有个穷极无聊的程序员开发出了一个能够让你快速上手的zsh项目，叫做「oh my zsh」，Github 网址是：
>[https://github.com/robbyrussell/oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)
>这玩意就像「X天叫你学会 C++」系列，可以让你神功速成，而且是真的。

接下来就讲讲怎么在mac上安装oh my zsh

### 基础安装

准备工作: 你的mac上需要装curl 或者 wget, 如果你的电脑上没有安装curl 或者 wget, 请安装后再继续。

你可以通过curl 或者 wget的命令行安装

#### curl

    curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

#### wget
    
    wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O - | sh

### 手动安装

准备工作: 你的mac上需要安装git。

1、克隆代码库

    git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh

2、备份已有的.zshrc [可选步骤]

    cp ~/.zshrc ~/.zshrc.orig

3、新建一个zsh的配置文件

可以拷贝一份已有的模板文件来创建zsh的配置文件

    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

4、修改默认shell

    chsh -s /bin/zsh

5、初始化你的zsh配置

一旦你在终端中新开一个窗口, 它将自动开启加载有oh my zsh配置的zsh。

### 安装遇到问题

如果你安装完成后，在终端中新开一个窗口，它没有自动加载有oh my zsh配置的zsh，可以参考如下解决方案:

1. [修改系统的默认shell](http://blog.yuaz.net/archives/292)
2. [Change default shell from bash to zsh](http://apple.stackexchange.com/questions/88278/change-default-shell-from-bash-to-zsh)

### 更多内容

1. [zsh](http://www.zsh.org/)
2. [oh my zsh](https://github.com/robbyrussell/oh-my-zsh)
3. [终极shell](http://macshuo.com/?p=676)
