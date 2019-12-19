---
title: 《最适合后端的 React Admin》第 1 章：搭建环境和运行 Demo
date: 2018-04-02 20:47:45
description: "本系列更多的作用是如何使用和引导现代前端开发的思路"
categories: [React]
tags: [React]
---


## 声明

- **如果你是 macOS 或者 Linux 系统，请牢记在各个过程中使用 sudo 开头，不然可能会出现各种问题。**
- **该项目部分章节运行于 macOS，部分章节写于 Windows，对于跨平台开发是完全没问题的。**
- 以下软件环境适用于全平台：React、Angular、Vue
- 支持 macOS、Windows、Linux 各平台
- 电脑必须有 [Git](https://git-scm.com/) 环境，并且懂得 [Git](https://git-scm.com/) 相关基础知识
- IDE 选择：
    - 如果你有 [IntelliJ IDEA](https://www.jetbrains.com/idea/) 的经验建议使用 [WebStorm](https://www.jetbrains.com/webstorm/)（机子内存不能低于 8G，并且要有 SSD 硬盘）
    - 如果你喜欢轻量编辑器，推荐使用 [Visual Studio Code](https://code.visualstudio.com/)
    - 如果都不了解，可以稍后看：`《最适合后端的 React Admin》第 2 章：IDE 选择、如何 Debug`

## 前端开发所需软件环境

- [Node.js](https://nodejs.org/en/)  
    - 当前（201804）最新稳定版本：8.11.1 LTS（201810 更新：当前最新版本 8.12.0 LTS）
    - 如果你此时在是未来时间看这篇文章，并且使用 Node.js 最新版出现问题，请下载我保存在百度云的历史版本，这样有助于你学习。并且希望你出现版本兼容问题可以进行反馈。
    - 下载：
        - 官网下载：<https://nodejs.org/en/download/>
        - 安装过程都是下一步，无需设置。
- [Yarn](https://yarnpkg.com/lang/en/docs/install/#windows-stable)
    - Yarn 发音 `[jɑn]`
    - 当前（201804）最新稳定版本：1.6.0（201810 更新：当前最新版本 1.10.1）
    - 官网下载：<https://yarnpkg.com/lang/en/docs/install/>
- 以上两个工具基本是现在前端界公认的基础软件，好比如 Java 界的：
    - JDK == Node.js
    - Maven == Yarn / npm
- 补充一下必备的基础知识点：
    - Yarn 和 npm 的区别：
        - [npm和yarn的区别，我们该如何选择？](https://zhuanlan.zhihu.com/p/27449990)
        - [Yarn vs npm: 你需要知道的一切](http://web.jobbole.com/88459/)
        - [Npm vs Yarn 之备忘详单](https://jeffjade.com/2017/12/30/135-npm-vs-yarn-detial-memo/)
    - 推荐设置 Yarn 源为淘宝国内源，设置命令：`yarn config set registry  http://registry.npm.taobao.org`
    - 用 npm 也是可以的，只是 yarn 更加现代，简单使用其实没啥太大区别。
    - 如果你要用 npm，建议再安装：[nrm](https://segmentfault.com/a/1190000000473869)
      - 如果你的 node 环境安装的比较早，建议重装。或者升级：`npm i -g npm`


## 运行 Demo 项目

#### fork and clone 项目

- 访问 Github 项目地址：<https://github.com/satan31415/umi_admin>
    - fork 项目到自己的名下，方便后面自己进行修改。
- git clone 项目，假设项目本地路径为：`/home/satan/react-code/umi_admin`

#### 安装 Demo 项目所需依赖包

- 进入项目：`cd /home/satan/react-code/umi_admin`
- 用 Yarn 安装依赖包：`sudo yarn install`，安装成功后会有提示。用 npm 也可以：`sudo npm install`
  - 第一次运行，时间会比较久。不同地区的网络情况不太一样，不排除出现长时间的下载或者安装失败，建议失败后可以多试几次。如果还有问题可以及时联系我。
- 在 macOS 和 Linux 下，如果要删除 `node_modules` 文件夹，最好在终端中用命令行删除，不然文件太多，很容易卡死 IDE。
- 如果需要升级依赖包，比如升级 umi 可以：`npm i umi`

#### 启动项目，查看 Demo 效果

- 启动命令：`yarn start`
- 打开浏览器访问：<http://localhost:8001>
  - 默认端口修改位置：`/home/satan/react-code/umi_admin/.env`

## 联系我

- 博客：<http://sayshy.com>
- 邮件：`satan31415#qq.com`
- Github：`https://github.com/satan31415`
