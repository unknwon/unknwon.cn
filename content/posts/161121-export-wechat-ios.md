---
title: 如何备份导出 IOS 微信聊天数据库
categories: ["杂记"]
tags: ["wechat"]
date: 2016-11-21
lastmod: 2020-04-16
---

网络上已经有一些关于探讨如何导出 IOS 版微信聊天记录的导出，比如 [WeBack](http://www.fiosoftware.com/weback/) 和 [iTools](http://jingyan.baidu.com/article/870c6fc309c1edb03ee4be4f.html) ，但这两类工具主导针对的都是小白用户，并没有给出如何获取完整数据库的方案，尽管实际原理相差无几，但想要程序更好地进行操作数据，还是所有差别，特此记录。

目前最新的 IOS 版本为 10.1.1，据观察该版本的备份文件存储格式与 IOS9 略有不同，因此部分没有及时更新的导出工具暂时无法使用。

## 创建 iPhone 备份

想要获取到应用的具体数据，就先要将数据以备份的形式先存储在电脑上，本文以 Mac 系统为例。

首先，使用数据线将手机连接上电脑，如果是首次连接，需要选择 **信任该电脑**。

然后打开 iTunes，找到正在连接的手机：

![image](https://cloud.githubusercontent.com/assets/2946214/20511361/b77acfc0-b045-11e6-80fe-54f7534ccba6.png)

在 **备份** 区域内，选择 **本电脑**，并务必取消勾选 **给 iPhone 备份加密**，否则之后我们取出的数据也是无法使用的：

![image](https://cloud.githubusercontent.com/assets/2946214/20511398/106dafc6-b046-11e6-8823-89696f8f5139.png)

接着，点击 **立即备份** 并等待备份完成：

![image](https://cloud.githubusercontent.com/assets/2946214/20511409/30258014-b046-11e6-95ff-382a5bcb32f3.png)

至此，备份工作就已经完成了。

## 获取微信数据库

为了方便地取出相应的备份数据，我们需要借助工具 [iPhone Backup Extractor](http://www.iphonebackupextractor.com/free-download/)，虽然这是一款收费软件，但是试用的功能已经完全满足我们的需求。

下载安装后，在左侧列表找到我们刚刚创建的备份文件（图标为 iTunes 样式的）：

![image](https://cloud.githubusercontent.com/assets/2946214/20511494/df4f2ea0-b046-11e6-8396-3572c1d40678.png)

单击之后需要等待加载完成，大约需要几十秒。加载完毕之后，我们就可以选择专家模式（Expert Mode）：

![image](https://cloud.githubusercontent.com/assets/2946214/20511520/05a78ae8-b047-11e6-9144-f282b921d5c4.png)

我们需要的文件为 **Application Domains/com.tencent.xin/{UUID}/DB/MM.sqlite**，将其勾选：

![image](https://cloud.githubusercontent.com/assets/2946214/20511550/4335587c-b047-11e6-91fc-268611ab3dc6.png)

在 com.tencent.xin 目录下会有多个 UUID 组成的目录，其中一个全部为 0 可以忽略。剩下就需要根据你的微信用户来选择了（如果知道算法的小伙伴请不吝赐教！），我的手机只登陆过一个微信号，所以没有这个麻烦：

![image](https://cloud.githubusercontent.com/assets/2946214/20511596/8913614a-b047-11e6-9f4e-92e0febfce71.png)

最后，就可以单击右下角的 Extract 按钮导出数据库了：

![image](https://cloud.githubusercontent.com/assets/2946214/20511619/b4ac3822-b047-11e6-8382-024006608f95.png)

大功告成！

至于数据库里面的具体关系，我也并没有全部搞清楚，~~但是会在下篇中举出已经发现的线索，足够进行用户识别和聊天记录分析。敬请期待！~~ 大家可以参阅文章 [iOS 微信的本地存储结构简析](http://daily.zhihu.com/story/8807166)。

### 相关阅读：

- [查找 iPhone、iPad 和 iPod touch 的备份](https://support.apple.com/zh-cn/HT204215)