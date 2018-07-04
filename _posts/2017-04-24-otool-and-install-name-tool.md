---
title: Dyld Error Message:Library not loaded:xxx
date: 2017-04-24 15:03:13
tags:
- otool
- dyld
- install_name_tool
categories:
- macos
---

### 前言
最近的一个项目中，网络部分有依赖openssl库，于是用`brew`安装了openssl。其安装的地址 `/usr/local/Cellar/openssl/1.0.2k/` , 同时会在 `/usr/local/opt/openssl/`生成一个软链接。

在生成的app中，已拷贝依赖的动态库:
- `MyApp.app/Contents/Frameworks/libssl.1.0.0.dylib`
- `MyApp.app/Contents/Frameworks/libcrypto.1.0.0.dylib`

但当其他电脑(非开发电脑)运行app时，会直接闪退，并看到崩溃的日志，主要如下:
```
Dyld Error Message:
  Library not loaded: /usr/local/Cellar/openssl/1.0.2k/lib/libcrypto.1.0.0.dylib
  Referenced from: /Volumes/MyApp/MyApp.app/Contents/Frameworks/libssl.1.0.0.dylib
  Reason: image not found
```
<!-- more -->
### 分析
根据上面的日志可以明确的知道，`libssl.1.0.0.dylib`依赖于`libcrypto.1.0.0.dylib`，但app在加载动态库时并没有使用同目录下的库，而是去找编译时库所在的位置。

通过`otool`工具可以查看`libssl.1.0.0.dylib`链接的依赖：
```
$ otool -L libssl.1.0.0.dylib
libssl.1.0.0.dylib:
	@executable_path/../Frameworks/libssl.1.0.0.dylib (compatibility version 1.0.0, current version 1.0.0)
	/usr/local/Cellar/openssl/1.0.2k/lib/libcrypto.1.0.0.dylib (compatibility version 1.0.0, current version 1.0.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1238.0.0)
```

### 解决方法
我们需要用到另外一个实用的工具`install_name_tool`，修改`MyApp.app/Contents/Frameworks/libssl.1.0.0.dylib`中的依赖库。
```
$ install_name_tool -change /usr/local/Cellar/openssl/1.0.2k/lib/libcrypto.1.0.0.dylib @loader_path/libcrypto.1.0.0.dylib libssl.1.0.0.dylib
```
修改依赖库后，重新通过otool查看:
```
$ otool -L libssl.1.0.0.dylib
libssl.1.0.0.dylib:
	@executable_path/../Frameworks/libssl.1.0.0.dylib (compatibility version 1.0.0, current version 1.0.0)
	@loader_path/libcrypto.1.0.0.dylib (compatibility version 1.0.0, current version 1.0.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1238.0.0)
```
可以看到，`MyApp.app/Contents/Frameworks/libssl.1.0.0.dylib`中对`libcrypto.1.0.0.dylib`的依赖已经修改为同一目录下的版本。
