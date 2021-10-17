---
title: 高效工作的朴素方法论
categories: ["杂记"]
tags: ["sourcegraph", "remote work", "methodology", "productivity"]
date: 2021-10-13
lastmod: 2021-10-13
---

## 强迫症起源

很久之前我就想要写一篇文章记录我的工作方式，并在分享的同时对自己的工作方式进行审视，看看有什么能改进的地方。

我是那种总是要为下载软件、订阅服务和购买硬件寻找理由的人，举个简单的例子。我的 macOS 同时安装了 [GoLand](https://www.jetbrains.com/go/) 和 [Visual Studio Code](https://code.visualstudio.com/) 用于 [Go 语言](https://golang.org/)开发，但是何时使用什么软件是有规矩的。

简单来说：
- 如果是开发正经的项目，我会使用 GoLand
- 如果是进行临时编辑，或是开发临时项目，我会使用 VSCode

但凡它们其中一个能干这两件事，另一个就不会存活在我的电脑上。

没错，我对于我的开发环境、工作流程和工作产出就是有很严重的[强迫症](https://www.mayoclinic.org/diseases-conditions/obsessive-compulsive-disorder/symptoms-causes/syc-20354432)。

## 出来玩最重要的是出来

我自诩为 [GTD](https://en.wikipedia.org/wiki/Getting_Things_Done) 方法论的实践者，因此在我 [TickTick](https://ticktick.com/) 的今日视图（Today）里会有许多计划或推迟到今天的任务，这其中就包含了一个每日重复的任务，叫梳理今日任务（Plan for the day）。

### 花三分钟安排一天

每天开始，我都会先梳理一遍今日视图里的所有任务：

1. 有些任务会依赖于现实世界的因素，我一般会先定一个大致的日期，然后到那一天再进行判断是否需要再次延后。
1. 有些任务则完全没有可能在今天完成，那么也需要被延后。一般来讲，我主要专注于今天任务的安排，对于是否可能造成未来某个日期的任务堆积并不是很在意，因为可以等到那天再做梳理。

梳理任务的关键在于对于自己实际的产出效率和可用时间上的正确评判，很显然我不是超人，每天也只有 24 个小时。

### 预热大脑

首先，泡一杯咖啡。

我是一位业余咖啡选手，我女朋友甚至因为我爱喝雀巢三合一而嘲笑我喝的并不是咖啡而是饮料。

![That's so mean](https://media.giphy.com/media/frAYDKLzkRoWf08SGw/giphy.gif)

好吧，无可辩驳。

然后，我会按照以下步骤进行大脑预热：

1. **Slack DMs:** 私聊消息（Direct messages）享有最高的优先级，因为一般都是较为紧急、私人或重要的事项。
1. **Slack Threads:** 我正在跟进或者有提及我的讨论一般来讲会更需要我的关注、输入和反馈。
1. **GitHub PRs with review requested:** 作为一名软件工程师，我深知被反馈环节卡住的无语感，尤其是团队成员还分布在各个不同的时区。当每次反馈周期可以长达 24 小时的时候，尽早提供我的反馈对于提升团队的整体运作效率时非常关键的。
1. **Slack starred channels:** 处理所有星标频道（Starred channels）的未读消息，其中有些消息我可以作出快速回复，有些消息需要更多时间的思考，则会加入我的当日待办事项。星标频道是与我日常工作最息息相关的一些内容，我也会经常查看确保没有消息遗漏。
1. **Slack reminders:** 处理稍后提醒（Reminders）并对每个提醒事项作出决策：
    - 添加到我的当日待办事项，即打开一个浏览器标签
    - 如果我觉得此时的当日待办事项已经够多了，就延后到今天更晚的时候再做决定
    - 如果我觉得今天肯定没有时间或还不是一个执行的好时机，则会延后到之后的某个日期
1. **Emails:** 在大脑预热阶段，我只会处理那些我今天肯定没有时间执行的邮件并延后到之后的某个日期。剩余的邮件则自动成为我的当日待办事项的一部分，并根据邮件到达时间确定优先级（越早到达的邮件优先级越高）

此时此刻，我一般就会有许多浏览器标签了，这是我当日待办事项的三种形式之一，其余两种分别为 TickTick 中的今日视图和电子邮件收件箱。


I've learned my lesson that it is not always possible to be able to get everything done in my task queue, and it is still OK to end my day to avoid creating a stressful mindset without a good reason.

## Dealing with notifications

I notice people often have anxiety from notifications, and push notifications could drive me crazy, too. Therefore, I aggressively configured notifications in the way that would minimize the number of push notifications, and relying on the long-polling mode for consuming notifications.

### Sane setup

The general principle for my sane setup of notifications is to turn them off, which means no push notifications nor app badges (i.e. red dots), including all desktop or mobile apps (Yes, emails, messages, everything). Why? Because this ensures I only pull notifications when I'm ready to pull, not because they're coming.

Exceptions are rare, only Slack and WeChat DMs (I mute every group chat in WeChat), even then, I don't allow any red dot to appear on my screen, it’s just that I get notified when a DM is coming, temporarily.

### Slack

I wish there is an option to allow push notifications only for DM, but I can live with it for now.

![Slack notification](/img/211013/slack-notification.png)

For normal messages, I have set up four categories for my Slack channels:

1. **Starred:** As previously mentioned, these are the channels that I would constantly check for new messages.
1. **Priority:** Channels that are not directly related to my day-to-day work, but I have interests on following updates, including `#dev-databases`, `#dev-experience`, `#lang-go`, etc.
1. **Unmuted:** When I get bored, I go check new messages in these channels. 🙂
1. **Muted:** I do not follow what's going on these channels, but often times I need to post messages (because Slack requires join a channel in order to post a message), including `#legal`, `#finance`, etc.

### GitHub

I only receive emails for threads I'm participating, and other notifications will reside on the web. I have also completely disabled notifications from GitHub Actions until I'm able to configure it on the per-repo basis.

![GitHub notification](/img/211013/github-notification.png)

Notifications sitting on the web are not ignored, I triage _every single one of them_. Yes, there are thousands of them every month.

One important thing is that I route work-related notifications to my work email, not to my personal email.

![GitHub work notification](/img/211013/github-work-notification.png)

### Gmail

Gmail filters help me highlight, prioritize, and delete emails.

![Gmail filters](/img/211013/gmail-filters.png)

## Don't drop the ball

It is super often that I encounter a message but I do not have time to think or act on it, and I do not like the feeling of I missed something, nor trying to remember countless things in my brain.

I make extensive use of **Slack reminders**, this is one of my favorite features of Slack (the other one is **Threads**).

![Slack reminders](/img/211013/slack-reminders.png)

If the message happens to be an email, I snooze it.

![Email snooze](/img/211013/email-snooze.png)

You may get frustrated like I was wanting to customize the time on this drop down, read [How to Find and Change the Times for Your Gmail Snooze Settings](https://www.groovypost.com/howto/find-and-change-the-times-for-your-gmail-snooze-settings/).

## Rolling in the virtual workplace

I have a dedicated Chrome profile as the virtual workplace for work-related communications, which means:

- I run Slack in the browser
- I do not have Slack or work email installed/connected on my laptop nor my phone

Why not the Slack desktop app? Because it does not allow me to isolate my work workspace from other communicate workspaces, which is really really really 👇

![Sucks](https://media.giphy.com/media/2zcmB56pYiTVEUSDa4/giphy.gif)

Another major benefit of running Slack in the browser is that I can take advantage of [Chrome tab groups](https://blog.google/products/chrome/manage-tabs-with-google-chrome/) for organizing work (by opening multiple tabs for different Slack messages/threads).

![Chrome tab groups](/img/211013/chrome-tab-groups.png)

I also practice (kind of) Tomato timer, using a dead simple, beautiful, and free app for macOS named [Flow](https://flowapp.info/). By no means as a proof of my work (we at Sourcegraph evaluate results not number of hours), but a way to track my time spent to measure my own sense of productivity.

## Favor asynchronous communication

I believe interruption is the biggest killer for productivity in software engineering, and why does asynchronous communication help? I recommend reading [Maker's Schedule, Manager's Schedule](http://www.paulgraham.com/makersschedule.html).

There are many other benefits of asynchronous communication, read more on [Asynchronous communication at Sourcegraph](https://handbook.sourcegraph.com/company/asynchronous-communication).

## Take time off

Software engineering is a creative type of work, and the human brain is the most complex and sophisticated instrument in the world. Working longer hours does not guarantee to be more creative, every mechanician knows parts need to be checked and maintained periodically.

Can you ever imagine your airline company tells you that the aircraft is going to fly faster because it hasn't been checked and maintained for a very long time?

[Sourcegraph is very generous on Paid time off (PTO)](https://handbook.sourcegraph.com/people-ops/paid-time-off-and-working-hours) to help everyone be energized, motivated, and productive.
