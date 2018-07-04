---
title: Mac系统命令行工具的一些用法
date: 2016-04-28 17:23:54
updated: 2016-04-28 17:23:54
tags:
- cli
categories:
- macos
---

#### sips批量处理图片
##### 查看图片的属性
```bash
$ sips -g all sample.jpg
```
<!-- more -->
输出：

    Users/xinus/tmp/sample.jpg
    pixelWidth: 3648
    pixelHeight: 2736
    typeIdentifier: public.jpeg
    format: jpeg
    formatOptions: default
    dpiWidth: 72.000
    dpiHeight: 72.000
    samplesPerPixel: 3
    bitsPerSample: 8
    hasAlpha: no
    space: RGB
    profile: sRGB IEC61966-2.1
    creation: 2010:03:18 11:08:11
    make: OLYMPUS IMAGING CORP.  
    model: FE5000,X905            
    software: Version 1.0                    
    description: OLYMPUS DIGITAL CAMERA    
##### 修改图片
```bash
# 旋转图片 90度
$ sips -r 90 sample.jpg --out sample-1.jpg
# 旋转图片 -90度
$ sips -r -90 sample.jpg --out sample-2.jpg
# 指定长宽的最大值，等比例缩放，这个例子中，执行后sample-3.jpg宽度600px，高度450px
$ sips -Z 600 sample.jpg --out sample-3.jpg
# 指定长宽值缩放，这个例子中，执行后sample-4.jpg宽度300px，高度400px
$ sips -z 400 300 sample.jpg --out sample-4.jpg
# 水平翻转
$ sips -f horizontal sample.jpg --out sample-5.jpg
# 垂直翻转
$ sips -f vertical sample.jpg --out sample-6.jpg
# 指定长宽值裁剪, 会以图片中点开始计算，不足的会补上黑色
$ sips -c 400 300 sample.jpg --out sample-7.jpg
```
##### 更多方式可以查看sips的帮助说明
```bash
$ sips -h
```

#### 查询某个命令的路径

``` bash
$ command -v gcc
> /usr/bin/gcc
```

#### [Homebrew](http://brew.sh/)的使用
##### 安装Homebrew
``` bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

##### 通过Homebrew安装包
```bash
$ brew install wget cmake
```
