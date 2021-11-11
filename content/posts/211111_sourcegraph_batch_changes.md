---
title: 使用 Sourcegraph 进行多仓库批量变更
categories: ["Sourcegraph"]
tags: ["sourcegraph", "batch changes"]
date: 2021-11-11
lastmod: 2021-11-11
---

## 任务说明

我上次用来把所有个人仓库的 CI [从 Travis CI 迁移到 GitHub Actions](https://github.com/go-macaron/macaron/pull/189) 的时候还叫 “Sourcegraph Campaigns”，现在已经升级改名成为 [Sourcegraph Batch Changes](https://docs.sourcegraph.com/batch_changes)。如果你之前没有听说过这个东西的话，用我的话来说，就是用于创建和管理对海量仓库的内容变更，可以是十几个仓库，也可以是上千个仓库。

这次我想要做的事情是让所有 [Flamego](https://github.com/flamego) 仓库的 CI 同时运行包括 Go 1.16 和 Go 1.17 版本的测试（当前创建这些仓库的时候 Go 1.17 还没发布）。

## 准备工作

[Batch Changes 的文档](https://docs.sourcegraph.com/batch_changes)写的非常详尽，我就懒得在这里重复了，主要指出几个关键性的链接，即我需要：

1. [一个正常运行的 Sourcegraph 实例](https://docs.sourcegraph.com/#quick-install)
1. [安装 Sourcegraph CLI](https://docs.sourcegraph.com/batch_changes/quickstart#install-the-sourcegraph-cli)

## 编写变更说明

Batch Changes 使用一个 YAML 格式的文件来说明如何实现规模化、自动化地创建内容变更，下面这个文件是根据我的需求编写好的版本：

```yaml
# File: add-go-1.17-to-flamego-ci.batch.yaml

name: add-go-1.17-to-flamego-ci
description: This batch change adds Go 1.17 to CI for all Flamego repositories

# 查找所有需要进行 CI 变更的 Flamego 仓库
on:
  - repositoriesMatchingQuery: "repo:github.com/flamego/ file:go.yml go-version: [ 1.16.x ]"

# 在每个仓库中执行以下步骤
steps:
  - run: "sed -i 's/go-version: \\[ 1.16.x \\]/go-version: \\[ 1.16.x, 1.17.x \\]/' .github/workflows/go.yml"
    container: alpine:3

# 描述 GitHub pull request 的分支、标题和内容
changesetTemplate:
  title: 'ci: add Go 1.17'
  body: Updates CI to include Go 1.17
  branch: batch-changes/add-go-1.17 # 变更将会被推送到这个分支
  commit:
    message: Add Go 1.17 to CI
  published: false
```

这个文件的大体意思就是先通过 `repo:github.com/flamego/ file:go.yml go-version: [ 1.16.x ]` 这个搜索查询锁定仓库列表，然后在每个仓库的 `.github/workflows/go.yml` 文件中把所有 `go-version: [ 1.16.x ]` 的字符串替换为  `go-version: [ 1.16.x, 1.17.x ]`。

## 预览、应用并发布

执行以下命令就可以生成预览：

```sh
$ src batch preview -f add-go-1.17-to-flamego-ci.batch.yaml
```

![src batch preview](/img/211111/src-batch-preview.gif)

如果该命令执行成功，还会直接在终端打印 Web 界面预览的 URL，非常方便！

![src batch preview on web](/img/211111/src-batch-preview-web.png)

然后我可以点击每个部分查看生成的变更是否符合预期：

![src batch preview on web with diff](/img/211111/src-batch-preview-web-diff.png)

看起来都没问题的话，就可以单击应用（Apply）：

![apply src batch preview](/img/211111/src-batch-preview-apply.png)

出于谨慎，我先尝试发布其中一个变更，看看会发生什么：

![publish changesets](/img/211111/publish-changesets.png)

果然，挂了。 😂

![publish changesets failed](/img/211111/publish-changesets-failed.png)

好吧，我需要移除说明文件里的最后一行 `published: false`，然后重新再来一遍。

不过又因为别的原因挂了 😇 排查之后发现是我的 GitHub Personal Access Token 没有获得操作 `workflow` 的权限，所以没有办法修改与 CI 相关的文件，也就是我正好需要修改的文件。在我把权限给到之后，貌似可以用了。

![GitHub pull request](/img/211111/github-pr.png)

焦急等待检查通过：

![GitHub pull request checks passed](/img/211111/github-pr-check-passed.png)

## 是时候表演真正的技术了

在我把所有的变更都发布了之后，对在 Batch Changes UI 和 GitHub.com 上的 Pull Request 列表进行了对比：

![side-by-side pull request comparison](/img/211111/prs-side-by-side.png)

其中一个 CI 还挂了...

![pull request check failed](/img/211111/pr-check-failed.png)

不过总体来讲是成功的！

![some pull requests merged](/img/211111/some-prs-merged.png)

Batch Changes 向仓库提交的就是最普通的 Pull Request，所以我可以直接向这个分支手动推送一些修复：

![push fixes to the pull request](/img/211111/push-tests-fix.png)

任务完成。

![closed batch changes](/img/211111/closed.png)

## 最后几句话

_并没有最后几句话_
