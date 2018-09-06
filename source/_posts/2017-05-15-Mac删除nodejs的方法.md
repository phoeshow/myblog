---
title: Mac删除nodejs的方法
tags:
  - nodejs
  - 教程
categories:
  - 技术文章
date: 2017-05-15 22:15:49
description:
---

> 原文地址： [How to Uninstall Node.js from Mac OSX--by Scott Robinson](https://getpocket.com/a/read/1168807267)

今日在一台新电脑上部署工作环境的时候发现一个 nodejs 被安装的乱七八糟，很多配置也改的很奇怪，权限不明，一些全局安装的 cli 工具无法使用，所以准备要重新安装一下。
众所周知在 Mac 上安装软件和删除软件都非常方便，就是把图标往回收站里一扔完事，可是回想起 nodejs 的安装过程就发现并不是这么简单的问题，因为根本就没有图标可供删除。
通过一番 google 终于找到了合适的方法。对于不同方法安装的 nodejs 也要分类讨论一下。

_注意_,这里我们的目标是同时删除 nodejs 和 npm 。

## 手动安装
如果是通过二进制包编译或者直接从官网下载安装包双击安装的 nodejs ，那删除起来就比较费劲了，毕竟作为一个运行时，很多东西会被安装在比较底层的非用户目录中，我们需要通过命令行的方式分别手动删除才行，这种情况下想要删除 nodejs 和 npm 我们需要按照下面的列表依次删除文件与目录：

- 删除 `/usr/local/lib` 目录中的 `node` 与 `node_modules` 目录。
- 删除 `/usr/local/include` 目录中的 `node` 与 `node_modules` 目录。
- 删除 `/usr/local/bin` 目录中的 `node` , `node-debug` , `node-gyp` 目录。
- 删除个人用户目录下的 `.npmrc` 文件（ **注意** 这个是 npm 的设置文件，如果你计划以后还要重新安装 nodejs 的话可以不删，不过因为我这里很多权限和软连接已经乱掉了，所以索性都删除干净）
- 删除个人用户目录下的 `.npm` 文件目录
- 删除个人用户目录下的 `.node-gyp` 目录
- 删除个人用户目录下的 `.node_repl_history` 目录
- 删除 `/usr/local/share/man/man1/` 中所有与node和npm有关的文件 `node*` ， `npm*`
- 删除 `/usr/local/lib/dtrace/` 中的 `node.d`
- 删除 `/opt/local/bin/` 中的 `node` 目录
- 删除 `/opt/local/include/` 目录中的 `node` 目录
- 删除 `/opt/local/lib/` 目录中的 `node_modules` 目录
- 删除 `/usr/local/share/doc/` 中的 `node` 目录
- 删除 `/usr/local/share/systemtap/tapset/` 目录中的 `node.stp`

以上目录我们依次删除就好，命令是 `rm -rf <path>` ，删除命令毕竟是比较危险的，请格外小心不要输入错误。

_这里请注意，因为不同人对于 node 的使用程度不同，上面列出来的文件和目录不一定所有人都有。_


## 通过 Homebrew 安装
如果原本的 node 是通过 Homebrew 安装的那就再简单不过了，我们只要通过反向命令 `brew uninstall node` 就可以正常删除，因为 Homebrew 是一种沙盒模式，会自动记录安装依赖时安装过的文件。

## NVM（Node Version Manager）安装
NVM 是非常知名的Node版本管理器，可以很方便的在一台电脑上部署多个不同版本的 nodejs ，通过 NVM 来添加与删除 nodejs 都是非常方便的。我们要删除某一个版本的 node 的时候只需要运行命令 `nvm uninstall <version>` 即可，比如：

```bash
$ nvm uninstall v7.7.4
```

## 未知安装方法
如果这台电脑来自别人，你也不知道之前究竟通过何种方式安装的 node ，那么我们只能靠猜了，但是也不能瞎猜，这里需要用到命令行指令 `which` 。

```bash
$ which node
/Users/scott/.nvm/versions/node/v4.1.2/bin/node
```
例如这样的一条命令可以让我们猜测出node是通过 nvm 安装的，然后我们就可以对症下药按照上面介绍的方式删除了。
