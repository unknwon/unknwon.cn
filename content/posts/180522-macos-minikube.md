---
title: 代理环境下在 macOS 上安装 Minikube 小记 
categories: ["杂记"]
tags: ["macos", "minikube"]
date: 2018-05-22
lastmod: 2020-04-16
---

因为实习要求需要使用 k8s，所以打算在上岗前本地安装一下 Minikube，本想着复制粘贴就能搞定。。因为代理的问题折腾了好一会，趁着还热乎赶紧备份下。

## 版本信息

- macOS 10.13.4
- Docker for Mac 18.03.1-ce
- VirtualBox 5.2.12
- Minikube 0.25.2

\***因为当前最新版 Minikube 0.27.0 在 macOS 下可能会遇到启动集群（`Starting cluster components...`）一直挂起，所以我粗暴地直接回退到 0.25.2**

## 安装组件

1. [安装 kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
2. [安装 Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)

## 干跑集群

在给集群配置代理之前（当然，宿主运行着代理），干跑也是可以初始化成功的。

```sh
# 在此过程中，首次运行会下载两个文件，关键看代理速度
$ minikube start
Starting local Kubernetes v1.9.4 cluster...
Starting VM...
Getting VM IP address...
Moving files into cluster...
Setting up certs...
Connecting to cluster...
Setting up kubeconfig...
Starting cluster components...
Kubectl is now configured to use the cluster.
Loading cached images from config file.
```

启动完成后，你会在 VirtualBox 看到一个名叫 **minikube** 的虚拟机：

![image](https://user-images.githubusercontent.com/2946214/40321296-28631016-5d61-11e8-885d-644da968da35.png)

好了，可以停止集群了。

```sh
$ minikube stop
Stopping local Kubernetes cluster...
Machine stopped.
```

## 配置代理

我宿主的代理是运行在 `http://127.0.0.1:1087`，重新启动集群前需要注意以下问题：

1. 代理的 HTTP 监听必须是在所有网卡上，即监听地址 `0.0.0.0` 而不是 `127.0.0.1` 或者 `localhost`，否则虚拟机内是连不上代理的
2. ~~设置集群代理时不可使用 `localhost` 而是虚拟机对应的宿主 IP 地址，例如 `192.168.99.1`~~
3. 一般而言，虚拟机的 IP 为 `192.168.99.100`，也需要忽略代理
4. 集群自身的 IP 需要被设置为忽略代理，否则宿主就连不上集群了
5. 集群的代理需要在启动时通过 `-docker-env` 特别指定，宿主的环境变量对集群内是不生效的

综上所述...

![image](https://user-images.githubusercontent.com/2946214/40321879-17757f1c-5d63-11e8-89f3-74508ab16c9a.png)

```sh
# 此两行本身就应该已经添加到你的 .bashrc 或者 .zshrc 了
$ export http_proxy="http://127.0.0.1:1087"
$ export https_proxy="http://127.0.0.1:1087"

# For Kubernetes
$ export no_proxy=192.168.99.100
$ minikube start --docker-env HTTP_PROXY=$http_proxy --docker-env HTTPS_PROXY=$https_proxy
...

# 启动集群后，还要再特别忽略集群节点的代理
$ export no_proxy=$no_proxy,$(minikube ip)
$ export NO_PROXY=$no_proxy,$(minikube ip)
```

忽略代理主要是针对宿主在访问集群时绕过代理，否则像这类私有 IP 地址段代理是永远不可能访问到的。。然后就会导致宿主一直无法获取到集群的状态。

## 参考链接

- https://github.com/kubernetes/minikube/issues/2148
- https://github.com/kubernetes/minikube/issues/530#issuecomment-346558249