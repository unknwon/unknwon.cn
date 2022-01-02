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

说到新版 MacBook，刘海是逃不开的问题，我相信很快就会有大批厂商跟风这种愚蠢的设计（参考 iPhone 的刘海）。不过严格来说这设计本身对我没什么影响，但是很多菜单栏的软件会受到影响，从而对我造成困扰。当菜单栏空间不够的时候，放不下的图标就会直接消失，根本没有别的问题访问它们。鉴于这个问题，我最终下定决心成为 [Bartender 4](https://www.macbartender.com/Bartender4/) 的付费用户（我有理由怀疑 Apple 和 Bartender 的开发者达成了某种秘密协议）。

话说回来，新款 MacBook 的电池使用寿命非常牛逼！自盘古开天地，充电器和笔记本对我来说就不能分离，不带着充电器和没带笔记本大差不差。而我呢，又是那种知道晚回家就会把手机电充满并且在 100% 电量的时候就开启省电模式的人。可见我对笔记本的电池使用寿命是多么的没有安全感，但是我在使用新款 MacBook 的过程中，从来就没有担心过电量不够的问题，真的非常牛逼！

另一个我不喜欢的是功能键的摆放位置，我所使用过的其它键盘布局左下角都是 Ctrl 键，因此我在使用这款 MacBook 的过程中非常容易按错。

## 重新评估浏览器

With a brand new laptop and a clean system, it is the perfect time to reevaluate what software and tools I could set up for. The biggest debate in my head is the primary browser. I was very tempted to use Safari as the only browser, but once again choosed Chrome.

Why?

The one last blocker for me is that Safari does not support multiple user profiles.

I rely on [different user profiles to separate work and personal life](https://unknwon.io/posts/211009_boring_methodology_to_be_productive/#rolling-in-the-virtual-workplace), so this is really critical.

I did think about using Safari for non-work purpose and use the Chrome profile that I set up for work. The problem with this solution that links are opened in the default browser, and Chrome knows which profile to open the link (i.e. the last active one). Thus, if my default browser is Safari but I need to open a link for work, I always have to copy-and-paste.

The situation might change with upcoming SOC 2 compliance for Sourcergaph, which requires using company-provided laptop for accessing sensitive company resources, and need to install some security agents. In that regard, I am probably going to remove all work-related stuff off my personal devices.

## Stay cool, stay quiet

It would be such a waste to not do any serious work on this powerful machine, so I followed [the docs for setting up local development environment for Sourcegraph](https://docs.sourcegraph.com/dev/setup/deprecated_quickstart), and did some extra [M1-specific work](https://docs.sourcegraph.com/dev/setup/how-to/m1_mac_local_dev).

For well-known reasons, I have to use proxy for accessing almost all sites that are required to complete my work, including any Google services (google.com, GCP), GitHub, Slack and so on. Having a modified system proxy creates some setup problems.

I use [ClashX](https://github.com/yichengchen/clashX) and it does not respect `/etc/hosts` file thus trying to proxy the traffic of `https://sourcegraph.test:3443` which is in fact my local Sourcegraph instance behind the [Caddy web server](https://caddyserver.com/). Luckily, I found [a blog post that shows how to bypass certain domain names for ClashX](http://blog.kelvinsail.com/2020/06/16/ClashX-%E9%85%8D%E7%BD%AE/). Also [change the ports of ClashX](https://github.com/yichengchen/clashX#change-the-ports-of-clashx) so I can access my local Prometheus instance (they happen to use the same port). Lastly, configure proxy for Git, NPM and terminal commands (I still don't understand why terminal commands ignore system proxy and need to be configured separately, although I have been doing this for years).

The macOS Monetary seems working good, nothing tedious found so far, and Apple-made keyboards are always great. The keyboard on this new MacBook Pro makes typing even more enjoyable, the feedback, the sound, and the surface touch.

The 14-inch MacBook stays cool at all time, and haven't heard the CPU fan once yet.

After two months, I conclude that I can do all of my work with the M1-chip.

## Last but not the least

Finally, I have the legitimate reason to buy more seats of software like CleanShot X, TablePlus, etc. that I love.

One more thing, I really hope the [Universal Control](https://www.apple.com/macos/monterey/#:~:text=to%20work%20using-,Universal%C2%A0Control,-and%20Shortcuts.%20Stay) to be released soon.