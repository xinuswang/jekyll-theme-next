---
title: iOS系统控件显示中文
date: 2013-10-17 10:19:24
tags:
- localization
categories:
- ios
---

App中使用系统控件，一般默认会显示英文，即便系统的语言环境设置的是简体中文。这需要在App的工程中加入中文支持，这样在中文的系统环境下，调用的系统控件，比如“返回”而不是“Back”。
### 步骤如下：

#### 为工程添加中文的本地化支持。
  - 在XCode中打开工程;
  - 左侧的工程栏中，选中工程，出现工程信息的页面。在Info标签页中，在"Localizations"部分，点击“+”添加“Chinese（zh-Hans）”;

#### 新建文件“Localizable.strings”，并添加中文的本地化支持。
  - 选中“Localizable.strings”文件，并打开最后侧的“File Inspector”；
  - 在"Localization"部分中，点击“+”添加“Chinese（zh-Hans）”;

#### 重新编译App，设备切换到简体中文语言环境时，查看运行结果 :)
