---
title: WebRTC学习笔记（1）- Win/Linux/macOS环境编译
date: 2017-03-30 12:18:03
tags:
- chromium
- ninja
- gclient
categories:
- webrtc
---

### 准备工作
WebRTC属于Chromium项目的子项目，本身可以独立编译，支持的平台包括Windows/Linux/macOS/Android/iOS等。项目本身是C++, 编译脚本也会用到Python。而项目代码托管于Google的服务器上，也就造成国内想要获取源代码，需通过科学上网的方式。所以，一些前置条件必不可少。

#### 环境准备
1. Win7 64位及以上，笔者用的Win10 64位；DX SDK； Windows SDK；
2. macOS；
3. Ubuntu，一定要是Ubuntu，笔者最先用的Mint也被迫换成Ubuntu；
<!-- more -->

#### 科学上网(VPN等)
这是获取代码的关键，整个工程下来，约几个G的容量。

#### 获取depot tools
整个工程又细分为多个子工程，不同平台拉取的东西不一样，所以需要官方提供的工具解决依赖问题。[原文在这里](http://dev.chromium.org/developers/how-tos/install-depot-tools)。

1. Git 2.2.1+
  - Win平台使用 [Msys (Git for Windows)](https://git-for-windows.github.io/);
  - Mac平台自带；
  - Ubuntu通过apt命令安装；
2. Python 2.7+
  - Win平台使用 [Python 2.7.x](https://www.python.org/downloads/windows/), 不要下载3.x版本，不兼容；
  - Mac平台自带；
  - Ubuntu若没自带可通过apt命令安装；
3. depot_tools
  - Mac/Ubuntu通过git获取
    ```bash
    $ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
    ```
    编辑 ~/.bashrc, 添加到环境变量中
    ```bash
    $ export PATH=`pwd`/depot_tools:"$PATH"    
    ```
    注意 *`pwd`* 是depot_tools所在目录的路径。
  - Windows下载[depot_tools.zip](https://storage.googleapis.com/chrome-infra/depot_tools.zip)
    然后把路径添加到系统的环境变量 **PATH** : `C:\path\to\depot_tools;%PATH%`

完成这些步骤后，新开一个命令行窗口，尝试使用 `gclient --help`，一切顺利则能看到相关提示输出。

### 获取代码并编译
1. 创建一个目录，并拉取代码:
  ```
  $ mkdir webrtc-checkout
  $ cd webrtc-checkout
  $ fetch --nohooks webrtc
  $ gclient sync
  ```
  整个过程，根据平台和网络情况的不同，需要数十分钟到几个小时不等。通过`fetch --help`可以看到更多项目。

2. 然后解决运行脚本，解决一些依赖，为编译做准备：
  ```
  $ gclient runhooks
  ```
  整个过程比上个环境需要的时间更长一些。

#### Windows平台编译
Windows 需先设置一些环境变量
```
$ set DEPOT_TOOLS_WIN_TOOLCHAIN=0
$ cd src/
$ gn gen out/debug
```

#### Linux平台编译
```
$ cd src/
$ gn gen out/debug
```

#### macOS平台编译
```
$ cd src/
$ gn gen out/debug
```

### 常见的问题
1. Windows下面如何生成vs2015工程文件?
```
$ set DEPOT_TOOLS_WIN_TOOLCHAIN=0
$ set GYP_GENERATORS=msvs
$ set GYP_MSVS_VERSION=2015
$ cd src/
$ gn gen out/debug --ide=vs2015
$ ninja -C out/debug
```

2. 如何生成X86版本的库？
```
$ gn gen out/debug_x86 --args="target_os=\"win\" target_cpu=\"x86\"" --ide=vs2015
```

3. 如何集成h264？
Webrtc默认未开启h264支持，需要gn时增加参数。而且webrtc使用ffmpeg解码h264，所以需要添加ffmpeg编译，但ffmpeg默认编译又不会带上h264的decoder，所以完整的选项如下：
```
$ gn gen out/debug_x86 --args="target_os=\"win\" target_cpu=\"x86\" is_component_build=false rtc_enable_protobuf=true rtc_use_h264=true rtc_initialize_ffmpeg=true ffmpeg_branding=\"Chrome\" rtc_include_tests=false" --ide=vs2015
```
  `rtc_include_tests=false` 表示不编译测试工程；
