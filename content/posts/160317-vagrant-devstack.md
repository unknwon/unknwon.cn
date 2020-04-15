---
title: 使用 Vagrant 安装 DevStack 小记
categories: ["杂记"]
tags: ["vagrant", "devstack"]
date: 2016-03-17
lastmod: 2020-04-16
---

今天遇到需要使用 OpenStack 的 Keystone 来管理用户的需求，于是就准备装一个本地的方便调试，过程不可谓不蛋疼（因为我比较懒，所以我直接记录成功的步骤）。同时证明了选择使用 Vagrant 来安装的绝对正确性！！！

我先用 Vagrant 装了一个 Ubuntu 14.04 的 Box，因为不懂哪个好，所以选了个下载次数最多的（先安装 VirtualBox）：

```sh
$ vagrant init ubuntu/trusty64; vagrant up --provider virtualbox
```

下载需要点时间，接着需要改 VM 的配置，需要指定一个 IP，并且默认的安装时会内存不够直接嗝屁：

```sh
$ vim Vagrantfile
```

```ruby
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  ...

  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
    vb.memory = "4096"
    vb.cpus = 2
  end
```

重载配置然后进入 VM：

```sh
$ vagrant reload && vagrant ssh
```

首先，随意目录克隆 DevStack 仓库：

```sh
$ git clone https://git.openstack.org/openstack-dev/devstack
```

因为 DevStack 不允许用 root 用户运行，而且不是 root 的这个用户还必须可以 sudo，所以一般都会说创建一个叫 `stack` 的用户。然而 Vagrant 进入就是 `vagrant` 用户，所以完整不用考虑这个问题。直接开干！

```sh
$ cd devstack
$ ./stack.sh
```

期间会要求你输入管理员的密码之类的。

然后就是漫长的等待，看见无数的字符在飞速滚动，我竟然有一种手抖装了百度全家桶的错觉。

言归正传，中途可能会出现一个非常傻逼的错误：

```sh
cp: cannot create regular file `/etc/glance/policy.json': Permission
```

没错，这个时候你会非常机智的使用 `sudo ./stack.sh`，然而，脚本会更傻逼地说，不好意思，你不能用 root 运行（那你倒是他妈的用 `sudo` 啊！）。

卧槽！！！！怎么办？

- 解决方案 A：手动帮它复制：`sudo cp -p /opt/stack/glance/etc/policy.json /etc/glance/policy.json`
- 解决方案 B：删除已存在的相关文件：`sudo rm -rf /etc/`

完了继续等待，大概半个小时后，一切安装完毕，访问 `http://192.168.33.10`。
