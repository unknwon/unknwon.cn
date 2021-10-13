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

## Kicking off the day

Like any [GTD](https://en.wikipedia.org/wiki/Getting_Things_Done) user, I have many tasks either planned or postponed to my Today's view in [TickTick](https://ticktick.com/). There is one special task that repeats every single day, which is the "Plan for the day".

### Plan for the day

It reminds me to go through and triage the entire list of tasks for today:

1. Some tasks are dependent on real world circumstances, I often tentatively put a date and check again on that date. If today is still not good time to tackle, I would tentatively pick another date again.
1. Postpone tasks that I definitely will not have time to get done to some later dates. It is always OK to temporarily overload later dates, because there is no perfect plan anyway. Won't borther making it in the first place, just focus on what's the most important.

That's it. It is not a complex process, the key is to be honest with myself, I'm not superman.

### Warming up the brain

Make a cup of coffee.

I'm not an expert in terms of drinking coffee, actually my girlfriend even makes fun of me for drinking beverages instead of real coffoe, because I'm a big fan of [Nescafé 3 in 1 Instant Coffee Sticks](https://www.amazon.com/Nescaf%C3%A9-Instant-Coffee-Sticks-ORIGINAL/dp/B000W5BS9K).

![That's so mean](https://media.giphy.com/media/frAYDKLzkRoWf08SGw/giphy.gif)

Anyway.

I then process following items to warm up my brain, let me explain each of them:

1. **Slack DMs:** Direct messages have the highest priority in my daily processing queue because generally they are about something very urgent, personal and/or important.
1. **Slack Threads:** Discussions I'm following and/or have been mentioned are more likely needing my attention, input, feedback and/or followup ASAP.
1. **GitHub PRs with review requested:** As a software engineer, I know the feeling of being blocked or waiting for feedback, espcially when teammates are across different timezones, a single feedback roundtrip could take up to 24 hours. It is better to share my feedback early because not everyone (in fact no one) ends a day at the same time as I do.
1. **Slack starred channels:** Triage unread messages in all starred channels to make sure I respond to simple things ASAP that could unblock my teammates, or add tasks that needs more thinking to my task queue for the day (i.e. open a browser tab for it). Starred channels are the ones most relevant to my day-to-day work, which needs me to constantly keep an eye on.
1. **Slack reminders:** Triage reminders to make a decision for each of them:
    - Add to my task queue, i.e. open a browser tab for it
    - Postpone to later today, often because I feel my task queue is risking to reach my capacity
    - Postpone to a later date, either because:
        - I definitely will not have time today
        - Not a good time to tackle depending on circumstances
1. **Emails:** As part of the brain warm up, I only take care of emails that need to be postponed to later dates. Other emails simply become part of my task queue, prioritized by the time of arrival to my inbox (earliest arrival has the highest priority).

Upon finishing the brain warm up, I will have a bunch of open browser tabs, as one of three forms of my task queue, the other two forms are:
- Planned tasks in my TickTick
- Email inbox

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
