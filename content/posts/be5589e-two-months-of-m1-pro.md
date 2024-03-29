---
title: MacBook 14 M1 Pro 上手两个月使用体验
categories: ["杂记"]
tags: ["macbook", "m1"]
date: 2021-12-29
lastmod: 2021-12-29
---

M1 芯片的 MacBook 在 2020 年刚出来的时候，我根本没怎么关注。因为我觉得新的芯片刚出来生态肯定是需要时间去追赶的，我也不是那么想要吃螃蟹。回头来看，这是个正确的决定。实际上，我甚至在去年年末还购置了英特尔芯片的顶配 MacBook Pro 16 来升级[我的办公设备](https://unknwon.cn/about/)。

今年可能是因为买了[小米盒子海外版](https://www.mi.com/global/mibox/)然后在 YouTube 上看了太多关于 M1 芯片的评测视频，让我想要观看今年苹果秋季发布会的直播，包括了两款 MacBook Pro 14 和 16 英寸的发布。老实说，我也不知道为什么我被搞点有点激动，但是依然选择预购了 14 英寸的版本，并在[发货的第一天就开箱了](https://www.bilibili.com/video/BV1kF411e75n)。

我并不打算把这个 14 英寸的版本替换目前使用的 16 英寸作为我的主力机器。我对目前 M1 生态的支持程度还有抱有一些莫名的怀疑，所以买来主要是作为尝鲜使用，体验一下看看对我的日常工作流有什么影响。拥有另一台 MacBook 的好处是可以不再需要不停地插拔 16 英寸的各种接头，带着 14 英寸到处跑就好了。

## 真的好用？

在 MacBook 的乞丐版和顶配版之间做比较感觉有点不道德，但是我再次被作为硬件公司的苹果公司惊艳到了（但苹果依旧是专门[制造垃圾软件的公司](https://twitter.com/jc_unknwon/status/1457216550390272000) 🙂）。

16 英寸的 CPU 风扇一直是被人诟病的问题，但对我来说这不是最糟的（第二遭的），而是当机器过热之后 Touch ID 就开始时不时的失灵。

尽管 14 英寸的机器比 16 英寸的版本还稍重一些，但是在整体尺寸上对我来说感觉像一个玩具，完美契合我的移动办公需求。当然了，新的显示屏也非常惊艳（呵呵，我懂个屁，不过大家都这么说我不说显得有些格格不入）。

说到新版 MacBook，刘海是逃不开的问题，我相信很快就会有大批厂商跟风这种愚蠢的设计（参考 iPhone 的刘海）。不过严格来说这设计本身对我没什么影响，但是很多菜单栏的软件会受到影响，从而对我造成困扰。当菜单栏空间不够的时候，放不下的图标就会直接消失，根本没有别的方法访问它们。鉴于这个问题，我最终下定决心成为 [Bartender 4](https://www.macbartender.com/Bartender4/) 的付费用户（我有理由怀疑 Apple 和 Bartender 的开发者达成了某种秘密协议）。

话说回来，新款 MacBook 的电池使用寿命非常牛逼！自盘古开天地，充电器和笔记本对我来说就不能分离，不带着充电器和没带笔记本大差不差。而我呢，又是那种知道晚回家就会把手机电充满并且在 100% 电量的时候就开启省电模式的人。可见我对笔记本的电池使用寿命是多么的没有安全感，但是我在使用新款 MacBook 的过程中，从来就没有担心过电量不够的问题，真的非常牛逼！

另一个我不喜欢的是功能键的摆放位置，我所使用过的其它键盘布局左下角都是 Ctrl 键，因此我在使用这款 MacBook 的过程中非常容易按错。

## 重新评估浏览器

拿到一个全新的 MacBook 和一个纯净的系统，正是思考更换软件和工具的最佳时机。我最大的困扰是浏览器的选择，我非常想要尝试将 Safari 作为我的主力浏览器，不过还是再次选择了 Chrome。

为什么呢？

因为 Safari 不支持多用户。

我非常依赖[浏览器的多用户来隔离个人和工作空间](https://unknwon.cn/2021/211013-boring-methodology-to-be-productive/)，这对我来说是必不可少的功能。

我的确有想过把 Safari 作为个人用途，然后继续使用 Chrome 完成工作相关的内容。这样做的问题是在其它软件中打开链接时总会使用默认的浏览器，而不会像仅使用 Chrome 时，可以根据最后活跃的用户来决定使用哪个用户来打开当前的链接。如果默认浏览器是 Safari 的话，在我需要打开工作空间的链接时就总是需要复制粘贴，非常麻烦。

不过情况可能会在不久的未来发生改变。Sourcergaph 即将开始 SOC 2 的审查流程，即要求所有敏感工作都必须在公司设备上完成，并安装指定的安全软件。考虑到这种情况，我应该会将所有工作相关的内容都从我的个人设备上移除。

## 安静的美男子

手持这么强劲的设备却不做任何重量级的工作的话就显得太暴殄天物了，所以我[按照文档搭建了 Sourcegraph 的本地开发环境](https://docs.sourcegraph.com/dev/setup/deprecated_quickstart)，然后补充了一些 [M1 相关的额外步骤](https://docs.sourcegraph.com/dev/setup/how-to/m1_mac_local_dev)。

由于众所周知的原因，我必须使用代理才能访问工作相关的所有站点，包括所有谷歌服务、GitHub、Slack 等等，并且使用代理软件还造成了一点点小问题。

我使用的软件是 [ClashX](https://github.com/yichengchen/clashX) 但是它不看 `/etc/hosts` 并会尝试代理到 `https://sourcegraph.test:3443` 的浏览，而后者其实又是我架设在 [Caddy](https://caddyserver.com/) 后的本地 Sourcegraph 实例。不过还好，我找到了一篇[如何让 ClashX 忽视指定域名的博客](http://blog.kelvinsail.com/2020/06/16/ClashX-%E9%85%8D%E7%BD%AE/)。除此之外，我还要[修改 ClashX 的默认端口](https://github.com/yichengchen/clashX#change-the-ports-of-clashx)，因为它和我本地的 Prometheus 实例端口冲突了。最后，还要为 Git、NPM 等等终端命令配置代理（尽管我已经这么做了很多年，但我始终不明白为什么终端命令可以绕过系统代理）。

新系统 Monetary 目测工作正常，Apple 的新键盘手感也不错，让我更加想要使用它打字。

新款 MacBook 的机身一直保持低温，并且还未曾听到 CPU 风扇启动过。

经过两个月的使用，我觉得可以说 M1 芯片的机器完全能够胜任我所需要做的工作。

## 结语

我终于有合理的借口为 CleanShot X、TablePlus 等软件购买更多的席位了，哈哈。

再提一嘴，我希望 [Universal Control](https://www.apple.com/macos/monterey/#:~:text=to%20work%20using-,Universal%C2%A0Control,-and%20Shortcuts.%20Stay) 早日发布！