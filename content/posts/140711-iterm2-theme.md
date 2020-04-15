---
title: iTerm2 主题配置
categories: ["杂记"]
tags: ["iterm2"]
date: 2014-07-11
lastmod: 2020-04-16
---

至于 iTerm2 是什么，可以参考 [我在用的 mac 软件(1)--终端环境之 iTerm2](http://foocoder.com/blog/wo-zai-yong-de-macruan-jian.html/)，我也是看这篇入门的。当初刚开始用的时候网上还能找到如何配置 iTerm2 的主题，可惜现在找不到了。虽然步骤简单，但是每次要重新弄的时候没有参考依据太挺蛋疼，顺便给各位有同样需求的人指下路。

以经典的谷歌 [Material Design](http://material-ui.com/) 主题为例，这里我找到两个色系：

- 完全遵循官方色调的：[material-design-colors.itermcolors](https://github.com/Unknwon/blogposts_ZH/files/605320/0008_material-design-colors.itermcolors.zip)

- 深色调修改版：[material-dark.itermcolors](https://github.com/Unknwon/blogposts_ZH/files/605321/0008_material-dark.itermcolors.zip)


接下来是导入主题文件，进入以下界面：

![0008_import](https://cloud.githubusercontent.com/assets/2946214/20509327/e41472e4-b035-11e6-90f8-f4a3e13d17a4.png)

选择下载后的解压文件即可。

然后就是终端的配色，编辑 `~/.bash_profile` 文件，在文件顶部加入以下内容（如果使用的是 ZSH 则不需要这步操作）：

```sh
# iterm2 color configuration
export CLICOLOR=1
export LSCOLORS=GxFxCxDxBxegedabagaced
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
```

然后：

```sh
source ~/.bash_profile
```

很好，大工告成！

![0008_iterm_screenshot](https://cloud.githubusercontent.com/assets/2946214/20509331/ea035fb2-b035-11e6-8787-cebb0666ad82.png)
