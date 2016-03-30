---
title: '操练 Ansible 之准备环境'
date: 2016-03-30 23:12:59
tags: [Ansible, Vagrant]
categories: DevOps
---

工欲善其事，必先利其器。在正式开始操练 Ansible 之前，我们得准备好相应的环境，安装一些软件：
* [Vagrant](https://www.vagrantup.com/downloads.html)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Ansible](http://docs.ansible.com/ansible/intro_installation.html)

## 1. 安装虚拟机

安装好 VirtualBox 之后，我们需要在里面安装一台 Linux 的虚拟机，为了能够很方便的创建和配置虚拟机，我们可以让 Vagrant 来帮助我们。

当这两样都安装好之后，开始使用 Vagrant 安装虚拟机。一般来说，有两种方式：在线安装和离线安装。

### 在线安装

Vagrant 社区提供了很多主流的 Linux 发行版对应的 [Box]（https://atlas.hashicorp.com/boxes/search）。以 Ubuntu 为例，我们可以在前面的这个仓库中搜索到 [Ubuntu/trusty64](https://atlas.hashicorp.com/ubuntu/boxes/trusty64)，然后我们在命令行中执行以下命令即可开始在线安装。

```bash
$ vagrant init ubuntu/trusty64
$ vagrant up --provider virtualbox
```

除此之外，也有一些第三方玩家提供一些 Box，例如：[Vagrantbox.es](http://www.vagrantbox.es/)，不过使用起来得手动添加 Box 的地址，参考下面的命令：
<!--more-->

```bash
$ vagrant box add {title} {url}
$ vagrant init {title}
$ vagrant up
```

一般来说，一个 box 都有好几百兆，如果网络好的话，可以选这两种方式。

### 离线安装

当然，生在红旗下，长在新中国的我们，不是每个人都有这么好的网络条件，那么这个时候离线安装似乎对我们来说更加有利。相信稍有经验的同学已经意识到在线安装的第二种方式稍加变形即可。假设我本地（~/Downloads/test.box）已经有一个 box 文件了，那么我可以下面的方式直接添加本地的 box。

```bash
$ vagrant box add testBox ~/Downloads/test.box
$ vagrant init testBox
$ vagrant up
```

在执行完添加步骤之后，可以通过以下方式查看已经存在的 Box 列表：

```bash
$ vagrant box list
```

如果你的网络条件不是很好，建议采用离线安装的方式。

## 2. 配置虚拟机
<!-- TODO -->
