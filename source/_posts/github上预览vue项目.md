---
title: github上预览vue项目   #文章标签
date: 2019-04-27 15:22:02            #文章创建时间
categories: 前端                      #分类
tags: [前端,vue,github]              #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 利用 github 在线预览 vue 项目 # 文章摘要
# photos:                             #文章标题图片
#     - "http://oz2tkq0zj.bkt.clouddn.com/17-11-9/52323298.jpg"
---

# github上预览vue项目

## build 打包 vue 项目

先在vue项目中运行build将项目打包

```bash
$ npm run build
```

**注意：** 打包之前要先看下项目的config目录下的index.js文件里的build输出目录是否正确

```js
/* index.js */

'use strict'
const path = require('path')
const ip = require('ip')

module.exports = {
  dev: {
    // ...省略代码
  },

  build: {
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),

    // Paths
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    // 注意此处的代码，原本的 assetsPublicPath: '/' 是这样的
    // 要将些处的路径改为：'./'，否则打包出来的引用路径不正确，页面无法正确加载显示 
    assetsPublicPath: './',
    // ...省略代码
  }
}
```

## 创建 gh-pages 分支并上传 dist 代码

1. 将打包好的代码 `dist` 目录单独剪切出来。
2. 进入 `dist` 目录并初始化 `git` 。
3. 创建 `gh-pages` 分支。
4. 将 `dist` 目录下的代码添加到暂存区。
5. 添加 `git` 的更新说明。
6. 强制 `push` 到远程仓库。
```bash
# 初始化 git
git init 

# 创建 gh-pages 分支
git checkout -b gh-pages

# 将 dist 目录下的代码添加到暂存区
git add -A

# 添加 git 的更新说明
git commit -m "更新说明"

# 强制 push 到远程仓库
git push 仓库地址 gh-pages -f
```
进入 github 项目中查看 gh-pages 分支是否更新成功

## 查看项目的预览地址

gh-pages 分支添加成功后，点击项目右上角的 Settings 设置里的下面 GitHub Pages 此选项中就可以看到项目的预览地址：
```
GitHub Pages
------------------------------------------------------------------------
GitHub Pages is designed to host your personal, organization, or project pages from a GitHub repository.

√ Your site is published at https://xxxxxxx
```
Your site is published at 后面的链接就是项目的预览地址