---
title: MacBook 14 M1 Pro 上手两个月使用体验
categories: ["杂记"]
tags: ["macbook", "m1"]
date: 2021-12-29
lastmod: 2021-12-29
---

M1 芯片的 MacBook 在 2020 年刚出来的时候，我根本没怎么关注。因为我觉得新的芯片刚出来生态肯定是需要时间去追赶的，我也不是那么想要吃螃蟹。回头来看，这是个正确的决定。实际上，我甚至在去年年末还购置了英特尔芯片的顶配 MacBook Pro 16 来升级[我的办公设备](https://unknwon.cn/about/)。

This year, for some reasons, maybe because I got a [Mi Box](https://www.mi.com/global/mibox/) and watched too many YouTube videos talking about how performant M1-chips are, made me watch the Apple event live, which released stunning new models of MacBook Pro 14-inch and 16-inch. I was super excited for no reason, to be honest, but still pre-ordered the 14-inch base model and [got it on the first day of shipping](https://twitter.com/jc_unknwon/status/1452872773290717192).

I do not plan to have this new MacBook Pro 14 M1 Pro to be a replacement of my existing 16-inch model. I'm still a bit conservative so I want to get a taste of M1-chip first, and to see what's the impact to my day-to-day workflow, espcially what tools are not working (which I heard a lot last year). Another upside of having a separate and smaller laptop allows me to carry around when I'm not at my desk, without plugging and unplugging my 16-inch to all the wires every time.

## Is it better?

Comparing MacBooks between a base and a top-tier model sounds unfair, but I'm once again being reminded that Apple is a hardware company (who [makes shitty software](https://twitter.com/jc_unknwon/status/1457216550390272000) 🙂).

The worst part of my experience with the 16-inch model I have is actually not the noisy of the CPU fan (which is the second-worst), but when it gets hot, the Touch ID sensor somehow stops working from time to time.

Desipte the new 14-inch model is slightly heavier than the previous 16-inch model, the overall size feels like a toy size to me, and perfect to my use case to be my portable device. And of course, the new display is _amazing_ (disclaimer: everyone seems saying this, I'd like not fall behind).

The notch, huh, yes. I will not be surprised if new laptops made by others are soon following this funny design (hint: the notch on iPhone). Well, technically it is not a problem for me, but it is a problem for menu bar items. Due to the notch, menu bar items are just disappeared if they can't fit in! The available space for menu bar items is so small that I finally decided to buy [Bartender 4](https://www.macbartender.com/Bartender4/) (Did Apple have secret deal with the Bartender developers? Wondering if their sales are dramatically increasing, LOL).

The battery life is impressive. The power adapter was like a separate component in laboratory but the MacBook can't really live without it in real life. I am the kind of person that if I know I'm going to back home late, I turn on battery save mode for my iPhone even when it's charged to 100%. In contrast, I have never worried about the new 14-inch model running out of battery. That's about how impressive it is.

One thing I do not like so far is the position of the function key, placing it on the left-bottom corner bothers me a lot, and I misused it as the control key very often.

## Reevaluating the browser

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