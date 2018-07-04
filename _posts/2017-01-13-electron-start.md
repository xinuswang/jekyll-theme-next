---
title: Electron入坑笔记
date: 2017-01-13 17:27:24
tags:
- electron
categories:
- webapp
---

### 前置条件
#### 1. [Node && NPM](http://nodejs.org/)

前往官方下载, 然后修改为国内的淘宝镜像[https://npm.taobao.org/](https://npm.taobao.org/)。
```bash
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
$ cnpm install [name]
```
**后面的Shell命令中，用cnpm替代npm**
<!-- more -->

#### 2. [Electron](http://electron.atom.io/)
通过npm安装。
```bash
$ npm install electron -g
```
在项目中安装。
```bash
$ npm install --save-dev electron
```
<!-- more -->
#### 3. Electron installer
主要用户打包安装包，通过npm安装。
```bash
$ npm install electron-packager -g
```

#### 4. Bower && Bootstrap
安装Bower包管理工具，用来管理前端的js、CSS。通过 Bower 可以安装并管理 Bootstrap 的Less、CSS、JavaScript和字体文件。
```bash
$ npm install -g bower
```
安装Bootstrap。
```bash
$ bower install bootstrap  
```

### Demo && Examples
electron-quick-start
```
$ git clone https://github.com/electron/electron-quick-start
$ cd electron-quick-start
$ npm install
$ npm start
```

electron-api-demos
```bash
$ git clone https://github.com/electron/electron-api-demos
$ cd electron-api-demos
$ npm install
$ npm start
```
