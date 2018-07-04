---
title: 悬浮窗口调试工具私有类UIDebuggingInformationOverlay
date: 2017-06-06 16:22:39
tags:
- debugging
categories:
- ios
---

### 起因

最近看到一篇文章([原文](http://ryanipete.com/blog/ios/swift/objective-c/uidebugginginformationoverlay/))，大意是做这位哥们在看Apple的私有API的时候，无意发现`UIDebuggingInformationOverlay`这么一个类，然后Google也搜不到太多的信息，就自己研究了一下，发现这个类可能是Apple内部用来帮助开发人员和设计人员用来Debug他们的app的。

这个类是一个`UIWindow`的一个私有子类，可以在App里面显示一个浮动的窗口，里面有当前View的层级结构和一些尺寸信息，很方便的查看相关信息。如图：

![image](/assets/images/UIDebuggingInformationOverlay.jpg)

### 如何使用
启动App时，加入初始化的相关代码，如下 （原文中是Swift语言版本）：
```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.

    id overlayClass = NSClassFromString(@"UIDebuggingInformationOverlay");
    [overlayClass performSelector:NSSelectorFromString(@"prepareDebuggingOverlay")];

    return YES;
}
```
运行App，用两个手指点击顶部的状态栏就可以调出该窗口。

### 总结
对查找一些UI问题时，比如View和ViewController层级结构、Frame尺寸等，是一个非常直观的工具。如果工程比较大，针对一些页面不知道找什么类的时候，可以通过这个工具反查到View或者ViewController的类名，然后去看代码。

除了能修改透明度以外，其他的参数都没有办法通过这个浮动窗口来调节，如果能加入修改背景色等功能，可能更加实用。
