---
title: git rebase 用法
date: 2021-12-02 17:16:28
tags: [JavaScript,面试]
categories: 技术类-前端
---

[参考自此文章](https://zhuanlan.zhihu.com/p/34197548)

以前提交代码一直是用命令行提交的

```js

// 一把梭
git pull
git add .
git commit -m 'xxxxxx'
git push
```

在多人开发项目下，这样提交的话会有如下效果:

![显示效果](https://upload-images.jianshu.io/upload_images/13931286-3d42d72b334fc7c7.png?imageMogr2/auto-orient/strip|imageView2/2/w/594/format/webp)

![显示效果](https://upload-images.jianshu.io/upload_images/13931286-d0fb965ce204e6f4.png?imageMogr2/auto-orient/strip|imageView2/2/w/222/format/webp)

当很多人都把自己的分支合到主分支的时候，这样会显得主线很乱而且还会有自动生成的提交信息

**所以提倡用`git rebase`**

## 使用 rebase 和 merge 的基本原则

1. 下游分支更新上游分支内容的时候使用 rebase
2. 上游分支合并下游分支内容的时候使用 merge
3. 更新当前分支的内容时一定要使用 --rebase 参数

例如现有上游分支 master，基于 master 分支拉出来一个开发分支 dev，在 dev 上开发了一段时间后要把 master 分支提交的新内容更新到 dev 分支，此时切换到 dev 分支，使用 `git rebase master`

等 dev 分支开发完成了之后，要合并到上游分支 master 上的时候，切换到 master 分支，使用 `git merge dev`

## 实际开发中遇到操作

当你和其他人在同一个分支开发时，在你提交的时候发现有人已经提交了一些东西上去了，你可以这样操作：

1. `git pull --rebase`

2. `git push`

你也可以使用vscode上下载的git插件来操作：

![vscode操作](https://upload-images.jianshu.io/upload_images/13931286-ecca9bb5bd96dc14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后再push上去。
