---
title: 使用 HTTPS 部署 Gogs
categories: ["杂记"]
tags: ["caddy", "gogs"]
date: 2015-11-14
lastmod: 2020-04-16
---

根据部署 Gogs 的方式，目前配上 HTTPS 的方法有两种：

1. 由反向代理服务器统一处理，例如：NGINX、Caddy
2. 裸奔 Gogs

HTTPS 的证书也有两种：真的和假装是真的。

那么，我们先来说下如何得到 HTTPS 证书。

### 获得 HTTPS 证书

1. 付费购买商业证书（土豪，请给 Gogs 项目捐助个千八百万吧 😊）
2. 向 [Let's Encrypt](https://letsencrypt.org/) 或 [StartSSL](https://www.startssl.com/) 申请免费证书
3. 自签发证书

前两种证书的获得方式基本可以谷歌到（其实我是真的懒），目前有其它工具可以生成自签发证书，这里主要说下如何用 Gogs 直接生成，改善生活。

想要 Gogs 支持生成自签发证书，必须使用 `cert` 构建标签构建（从官方下载的二进制都已经支持）。

好的，那么下面就是你需要执行的命令：

```sh
$ ./gogs cert -ca=true -duration=8760h0m0s -host=myhost.example.com
```

别忘了把域名换成你自己的（什么，你没域名？没域名你要个毛 HTTPS 啊！😵）

执行成功之后，就会在当前目录下得到两个文件：`cert.pem` 和 `key.pem`。

### 使用 HTTPS 证书

刚才已经提到根据部署方式的不同，有两种不同的配置方法。

如果你使用反向代理来部署 Gogs，那么 Gogs 这边除了说明你的 `ROOT_URL` 是带上 HTTPS，例如 `https://try.gogs.io`，其它

啥都别改！

啥都别改！

啥都别改！

先说 NGINX，示例配置如下：

```nginx
server {
    listen 443 ssl;
    server_name try.gogs.io;
    ssl_certificate path/to/cert.pem;
    ssl_certificate_key path/to/key.pem;

    location / {
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_pass http://localhost:3000$request_uri;
    }
}

# 以下部分表示重定向 HTTP 请求到 HTTPS
server {
    listen 80;
    server_name try.gogs.io;
    return 301 https://$host$request_uri;
}
```

看见没有！Gogs 还是使用 HTTP 正常启动！HTTPS 的处理全部交给 NGINX 就尼玛对了！！！！

虽然我说的比较简陋，但我想意思应该到位了。。。

好了，再来看看 Caddy 的话要怎么配置达到相同效果：

```caddy
https://try.gogs.io {
    proxy / localhost:3001
    tls path/to/cert.pem path/to/key.pem
}

# 以下部分表示重定向 HTTP 请求到 HTTPS
http://try.gogs.io {
    redir https://try.gogs.io{uri} 301
}
```

从简便程度来说，Caddy 真是甩 NGINX 一条天目山路！

当然，裸奔 Gogs 也是一种态度。在自定义配置中修改以下项：

```ini
[server]
PROTOCOL = https
ROOT_URL = https://try.gogs.io/
CERT_FILE = path/to/cert.pem
KEY_FILE = path/to/key.pem
```

(哈哈，要说行数，还是 Gogs 自带的配置最少呢 😂 连高亮都有！)

好了，如果你用的是真的证书，一个绿色的小锁会显示在 Chrome 的 URL 栏旁边；自签发的就会有个警告页面，然后是红色的坑爹状。
