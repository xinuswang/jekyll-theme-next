---
title: 部署Qt应用程序
date: 2017-05-15 17:49:46
tags:
- deploy
categories:
- Qt
---

应用程序开发完成后，需要打包成一个可以执行的包交付测试。常见对应Window、macOS、Linux三个常见桌面操作环境，会采用不同的打包方式。而其中Linux的发行版本众多，制作DEB或者RPM等不同的包也有区别。
<!-- more -->

### Windows平台
windeployqt是Qt官方提供的工具，其位于Qt安装路径下面的Bin目录下，可以手动就入PATH方便命令行中使用。windeployqt主要的作用是自动把程序所依赖的动态库、插件、其他资源等拷贝到程序的当前目录下。
拷贝编译得到的myapp.exe拷贝到某个单独的目录中，然后在该目录下执行此命令：
```bat
D:\MyApp>windeployqt myapp.exe
```
如果是Qt Qucik程序，则还需要把qml的目录带上，这个路径一般也在Qt安装目录下的
```bat
D:\MyApp>windeployqt myapp.exe --qmldir QML_PATH
```

### macOS平台
macOS平台也有类似的工具macdeployqt。当编译好myapp.app文件后，执行此命令可以解决依赖问题，如果增加 -dmg参数，则可以直接打包成dmg格式：
```bash
$ macdeployqt myapp.app -dmg
```

### Ubuntu平台
Linux平台上，Qt并没有提供相关的工具来解决依赖，但是我们可以直接deb、rpm等包格式，通过Ubuntu的仓库或者Fedora仓库解决Qt的依赖问题。

### 程序图标
参考Qt帮助文档"Setting the Application Icon"。

### 结语
