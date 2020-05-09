---
title: 在阿里云安装 Sourcegraph
categories: ["Sourcegraph"]
tags: ["sourcegraph", "docker", "阿里云", "github action"]
date: 2020-05-09
lastmod: 2020-05-09
---

## 前言

Sourcegraph 是一款集代码搜索、智能提示和代码变更管理于一体的企业级开发者工具，虽然针对开源项目有 https://sourcegraph.com 可以使用，但毕竟目前主站还是托管在美国中部的机房，对于国内的网络环境来说体验并不是非常流畅。况且目前如果要添加私有仓库的话，也是需要自己搭建一个实例才可以使用的。所以才有了想要自己在阿里云杭州的机房搭建一个私有实例，来加速日常的工作流。

## 同步镜像到阿里云

从道理上来说，本来是应该直接 `docker pull sourcegraph/server:insiders` 然后就能启动镜像了的，可惜啊，在国内机房直接从 Docker Hub 拉取镜像的速度实在是感人。所以只好剑走偏锋，利用 GitHub Action 做了一个简单的同步功能，将 Sourcegraph 实时构建版本的镜像每十分钟从 Docker Hub 同步到阿里云的 Docker Registry（详见 https://github.com/unknwon/docker-sync-to-aliyun/blob/master/.github/workflows/sourcegraph.yml）。在此感谢微软的财大气粗，公开仓库的 GitHub Action 无限制免费 :smile:

```yaml
name: Sourcegraph

on:
  schedule:
    - cron: '*/10 * * * *'

jobs:
  server:
    name: 'server:insiders'
    runs-on: ubuntu-latest
    steps:
    - name: Pull image from Docker Hub
      run: docker pull sourcegraph/server:insiders
    - name: Tag image
      run: docker tag sourcegraph/server:insiders registry.cn-hangzhou.aliyuncs.com/sourcegraph/server:insiders
    - name: Login to Aliyun registry
      uses: azure/docker-login@v1
      with:
        login-server: registry.cn-hangzhou.aliyuncs.com
        username: ${{ secrets.USERNAME }}
        password: ${{ secrets.PASSWORD }}  
    - name: Push image to Aliyun
      run: docker push registry.cn-hangzhou.aliyuncs.com/sourcegraph/server:insiders
```

在同步之前，需要先开通阿里云的容器镜像服务，并且创建一个镜像仓库。为了方便对应，我创建了 **sourcegraph** 命名空间和 **server** 镜像仓库，这个貌似是全局唯一的，所以大家应该就可以直接拉取并使用。

## 安装启动

考虑到 Sourcegraph 是需要直接访问 GitHub 主站（github.com）和 API 端点（api.github.com）的，所以需要先利用境外的机器 ping 一下这两个地址获取比较可靠的 IP 地址。然后将这两个 IP 地址添加到 Docker 镜像的 `/etc/hosts` 文件中，这里我已经 ping 好了，就直接和命令一起贴出来。

```bash
$ docker run -d \
    --add-host github.com:140.82.113.4 \
    --add-host api.github.com:140.82.113.6 \
    --publish 7080:7080 \
    --volume ~/.sourcegraph/config:/etc/sourcegraph \
    --volume ~/.sourcegraph/data:/var/opt/sourcegraph \
    --name sourcegraph \
    registry.cn-hangzhou.aliyuncs.com/sourcegraph/server:insiders
```

这条命令就是会在后台运行 Sourcegraph 的容器，并通过 volume 功能保存关键数据，方便后续升级。

噢，这就装完了。不过呢，还需要一个反向代理服务，我使用的是 Caddy 2，配置如下：

```caddyfile
{
    http_port 80
    https_port 443
}

sourcegraph.unknwon.cn {
    reverse_proxy * localhost:7080
}
```

配置教程这里就不详细展开了，后续可能还会写一个快速上手的教程，不过所有官方文档都展示在 https://docs.sourcegraph.com，大家可以尽情查阅。

## 版本升级

```bash
# 1. 拉取新版本的镜像
$ docker pull registry.cn-hangzhou.aliyuncs.com/sourcegraph/server:insiders
# 2. 停止当前运行的容器
$ docker stop sourcegraph
# 3. 删除已停止的容器
$ docker rm sourcegraph
# 4. 执行上个小节安装启动里的 docker run 命令
$ docker run ...
# 5. 清理过期的镜像版本
$ docker image prune --force
```

## 其它

我司招人，需要提供英文简历和进行英语视频面试，开放职位请查看 https://github.com/sourcegraph/careers。如有相中的，可以发送简历和职位信息到邮箱 joe@sourcegraph.com 进行内推后再提交求职申请应用 :blush:
