---
title: 中文博客搬新家
categories: ["杂记"]
tags: ["caddy", "hugo", "utterances", "阿里云", "备案"]
date: 2020-04-19
lastmod: 2020-05-04
---

从 2013 年开始就搞起了自己的中文博客，几经周折，从用小众的博客系统，到后来懒得托管直接丢在 GitHub 上把 Issue 当博客发，最终迁移到了现在的中国大陆境内站点。这里发个博客记录一下这次迁移相关的乱七八糟的故事。

## 起因

显而易见，我最近几年尤其地懒，在内容上的产出可以说是相当的可怜，所以肯定不是我想正儿八经写博客了才搞的。这里最关键的点我觉得还是域名的问题，因为我手上早早有了 unknwon.io 和 unknwon.dev 这两个域名，去年我心血来潮想要去查一下 unknwon.cn 是个什么情况。一查发现，嘿，11 月域名过期，好的，那我就等到十一月，然后发现之前的持有者并没有续费，但是因为阿里云的保护机制，过期之后还要等个一段时间才会把域名释放出来，所以我就定个 TODO 每周查一次。功夫不负有心人，终于在一月份的时候被我注册成功了。哈哈哈哈哈。

![](https://media0.giphy.com/media/12msOFU8oL1eww/giphy.gif?cid=5a38a5a2d8002caef50972fc1826c1d716a1db6ee8a535fd&rid=giphy.gif)

既然这个域名到手了，那用来做为中国大陆的主力站点也就成为了顺理成章的事情了。

## 筹备

那为什么拖到四月份才把这个博客搞起来呢？~~废话那肯定是因为懒啊，前面不是说了吗？~~ 我觉得主要有以下几个原因：
1. 受疫情影响，原计划在老家呆五天的我被迫呆了一个月之久，反正也出不去门，然后对工作充满了无限的激情，完全把域名这个事情给忘记了。
2. 备案要拍照，所以回杭州之后也没弄... 直到，前些天理了个头发。

整个备案流程还是挺出乎意料地快，用阿里云的手机端，第一天打电话初审通过之后管局短信认证，第二天就告知备案成功了。想想几年前还要去特定地点拍照和邮寄材料就头皮发麻（当然，我也压根没去）。现在真的是方便了呀。除此之外，阿里云自动帮我的 ECS 续了两天的费用... 还挺有意思的哈。

人生第一次域名备案就此完成。

接下来就要开始思考几个经典的问题：用什么工具来搭博客（静态的还是动态的），用什么主题（好看的还是特别好看的）。这里面还涉及到几个比较关键的细节，包括代码高亮、评论插件和 Markdown 语法等的支持程度。经过煎架和傲飞的轮番推荐，最后决定使用 [Hugo](https://gohugo.io) + [Caddy 2](https://caddyserver.com) 整一套，理由如下：
1. https://unknwon.io 也是用的 Hugo 和 Caddy 1 配套搭建的
2. 没有 2

## 搭建

### 服务器

考虑到博客反正也没什么流量，选用了纵观阿里云 ECS 最经济实惠的突发性实例 `ecs.t5-lc2m1.nano`，包含了强劲的 1 vCPU 和 512 MB 内存。带宽嘛，呵呵，提这个做什么。

### 博客主题

选用了 [LeaveIt](https://github.com/liuzc/LeaveIt)，大体上都非常酷，感谢作者的开源，不过感觉有些年份了，所以不得不亲自上手魔改了几下（整个站点都包含在了 GitHub 的 [unknwon/unknwon.cn](https://github.com/unknwon/unknwon.cn) 仓库里）：
1. 备案的域名指向没有跟上时代的步伐，作为强迫症，这是不可能忍的。
2. 代码高亮用的 Google code-prettify，总感觉是个上古的工具了，再怎么着也要用 Highlight.JS 啊。不过现在 Hugo 用的 goldmark 已经自带代码高亮功能，这些已经没有必要。
3. 评论系统仅支持 Disqus，我需要用煎架推荐的 [Utterances](https://utteranc.es)。
4. 虽然支持一键切换 Dark mode，但在傲飞的建议下，改成了随系统主题自动切换的 CSS 语法（顺便在强迫症的促使下，用 VSCode 的 prettier 格式化了一下所有 SCSS 文件）。
5. 页面失焦的时候标题自动改为 "I miss you!"，艹了，虽然这个功能很可爱，但是我还他妈的怎么知道我之前看的页面是哪个？不得浪费我两次来回点击的 Roundtrip？删了！

### 反向代理及 HTTPS

自 2015 年以来 Web 服务器我只用过 Caddy。虽然 Caddy 2 的文档（截止 2020 年 4 月 19 日）如同狗屎，对于实际部署没什么帮助，但是还是硬着头皮上了，配置如下：

```caddyfile
{
    http_port 80
    https_port 443
}

unknwon.cn {
        file_server * {
                root /root/apps/unknwon.cn/public
        }
}
```

除此之外还有一个小插曲。服务器无法获取 Let's Encrypt 证书，OCSP 服务器连接超时。本来呢，如果服务器在境外，获取 Let's Encrypt 的 SSL 证书是很无感的一件事情，但是国内的营运商就是他妈的有毛病，没事封人家 IP（你以为这样就能骗别人去买你的 SSL 证书了？）。

这还不简单，傲飞写了可以部署到 Cloudflare worker 上的一个反代脚本：

```js
addEventListener("fetch", event => {
    event.respondWith(handleRequest(event.request))
})

/**
 * Respond to the request
 * @param {Request} request
 */
async function handleRequest(request) {
    let url = new URL(request.url);

    url.host = "http://ocsp.int-x3.letsencrypt.org";

    let headers = new Headers(request.headers);
    headers.set("Host", url.host);
    headers.delete("Connection");
    headers.delete("Proxy-Connection");
    headers.delete("Keep-Alive");
    headers.delete("Proxy-Authenticate");
    headers.delete("Proxy-Authorization");
    headers.delete("Transfer-Encoding");

    return await fetch(url.href, {method: request.method, headers: headers, body: request.body});
}
```

~~嗯，到这里也就差不多了~~ 看看右边的滚动条，亲。

事实上，问题根本就不在这里，因为这个地址是 Let's Encrypt 的目录服务器主动返回的（MLGB 的 Caddy 2 文档也不说 `acme_ca` 的值应该是什么样，净写一堆没用的描述，我研究了半天才发现不是 OCSP 的地址，甚至跟这都没关系）。当然，也不是毫无收获，注册了 unknwon.workers.dev 的二级域名，以后还可以用的。

好吧，再经过一顿搜索（加上对 Caddy 2 文档的一顿骂街），终于在 Caddy 的论坛找到一丝线索，一个遇到和我同样问题的暴躁老哥，并[指出了问题的核心解决方案](https://caddy.community/t/caddy-stuck-at-activating-privacy-features/7390/7?u=unknwon)，就是改 `/etc/hosts/`：

```txt
23.215.130.83   ocsp.int-x3.letsencrypt.org
```

不过这个 IP 是会变的，下次不灵的时候，就用境外服务器 `ping ocsp.int-x3.letsencrypt.org` 然后就能拿到需要的 IP 地址。

### 评论系统

用的煎架亲自下场推荐的 [Utterances](https://utteranc.es)，别问为什么，就是好用就是帅，除了不能随系统自动切换 Dark mode 以外，无可挑剔！主要特点就是基于 GitHub Issue 作评论系统，虽然同类产品已经有非常多，但是这家是不需要自己注册 GitHub App，只要短短几行代码就能完成评论系统的部署！更何况，本身这个博客就是一个 Git 仓库，把评论放在 Issue 里简直是天作之合。至于评论通知，相当于复用了 GitHub 通知，不需要自己去站点靠缘分发现新的评论或回复。

```js
<script src="https://utteranc.es/client.js"
    repo="unknwon/unknwon.cn"
    issue-term="pathname"
    label="utteranc"
    theme="github-light"
    crossorigin="anonymous"
    async>
</script>
```

### 其它

因为站点还比较小，用 CDN 存图片还不是我的首选，所以对图片加载进行一些假仁假义的优化还是有必要的，用了 [Imgbot](https://github.com/marketplace/imgbot) 这个工具，定期发 PR 提醒你~~过于肥胖~~图片体积可以优化。

## 后续

经过一周断断续续地折腾，站点总算成型。后面还要搞一下自动从 GitHub 拉取更新，不然每次更新都要 SSH 到服务器 `git pull && hugo --destination=/root/apps/unknwon.cn/public` 显得很业余，也不好拿出来吹。

---

**2020-05-04 更新**：通过使用 [appleboy/ssh-action](https://github.com/appleboy/ssh-action)，成功地实现一推送到 `master` 分支就自动 SSH 到服务器进行部署。

GitHub Action 的配置如下（[deploy.yml](https://github.com/unknwon/unknwon.cn/blob/master/.github/workflows/deploy.yml)）：

```yml
name: Deploy

on:
  push:
    branches: [ master ]

jobs:
  hugo:
    name: Hugo
    runs-on: ubuntu-latest
    steps:
    - name: Compile content on the server
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.HOST }}
        username: ${{ secrets.USERNAME }}
        password: ${{ secrets.PASSWORD }}
        script_stop: true
        script: |
          cd apps/unknwon.cn
          git pull
          hugo --destination=./public
```
