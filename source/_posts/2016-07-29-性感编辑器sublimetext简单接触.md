---
title: 性感编辑器Sublimetext简单接触
date: 2016-07-29 10:09:45
tags: [工具]
description: 介绍Sublimetext的安装与常用插件
photos:
- http://o84rkcsji.bkt.clouddn.com/flurry_sublime_text_icon_v2_by_erikhellman-d5x3jqn.png
---

<!-- more -->

# Sublimetext的安装

Sublimetext的安装非常方便，只需要下载后一路下一步即可。

{% youku 280 400 %}
XMTY2NTMyMzgzMg
{% endyouku %}


# Sublimetext插件的安装

## PackageControl的安装

PackageControl是sublimetext官方的插件管理工具，通过pc我们可以非常方便的安装、管理其他插件。

因为PackageControl服务器身处国外，因为一些众所周知的原因，会出现访问困难，大家可以想办法科学上网解决问题。

这里贴出的是sublimetext安装PackageControl的代码：

{% codeblock lang:python %}
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
{% endcodeblock %}

## 常用插件的安装

{% youku 280 400 %}
XMTY2NTE4MzUwNA
{% endyouku %}

## 常用插件列表

- emmet：前端`必备`插件，曾经名叫zencoding，能够帮助我们快速书写html中的结构，[语法详情](http://docs.emmet.io/abbreviations/syntax/)
- ColorPicker：在当前文件中打开调色板工具，并且将选中的颜色快速上屏，这个插件总是打开系统**自带**的调色板工具，所以在Mac上效果很赞。快捷键为`⌘+⇧+c`，Windows为`ctrl+shift+c`，注意这个插件会和下面介绍的 `Convert​To​UTF8` 插件快捷键冲突，如果安装了 Convert​To​UTF8 会导致该插件快捷键失效。解决方法是更改 Convert​To​UTF8 的快捷键设置。

- Convert​To​UTF8：这个的用处是将文件的编码格式改为 UTF8 ，主要用于打开一些国内编辑器生成的GB2312文件或者弯弯们的BIG5码文件，插件使用的注意事项请查看[这里](https://github.com/seanliang/ConvertToUTF8/blob/master/README.zh_CN.md)。

- AutoFileName：可以扫描当前目录，实现文件名的自动补全，有了它 sublimetext越来越像一个正经的IDE了。

- BracketHighlighter：成对出现的括号给予高亮显示，对于写代码的人来说，找到另外半个括号的位置真的很重要。

- Color Highlighter：文件中的颜色代码高亮显示，不仅高亮还能实时演示出代码对应的颜色，你们说方不方便哇。

- SideBarEnhancements：增强侧边栏的功能，实现新建文件、改名、移动等等功能。

- SublimeCodeIntel：极大的增强自带语法提示功能。
