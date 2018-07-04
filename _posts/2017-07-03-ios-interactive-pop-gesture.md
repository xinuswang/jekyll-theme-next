---
title: iOS侧滑返回功能
date: 2017-07-03 16:48:38
tags:
- pop-gesture
categories:
- ios
---

## 前言
UINavigationController从iOS7开始，加入了右滑返回的功能。具体的操作，就是从左侧屏幕边缘，向右滑动当前页面，返回上一个页面的效果。

侧滑返回这是一个手势操作，因为iOS SDK中，在UINavigationController增加了这个手势的属性`interactivePopGestureRecognizer`，如下：
```objc
@property(nullable, nonatomic, readonly) UIGestureRecognizer *interactivePopGestureRecognizer NS_AVAILABLE_IOS(7_0) __TVOS_PROHIBITED;
```

侧滑返回某种意义上，可以算作UI层的默认操作，类似左上角返回按钮。App中可能需要控制这个行为，在用户决定离开当前页面，返回上一层时，弹框询问，或者提交、更新一些数据。

同时，又存在差异：点击返回按钮，这个页面pop到上一层页面的过程是不可逆的；而侧滑返回时，用户中途中断（取消）这个手势，页面并不会返回上一层，而是会恢复到当前页面。

对开发人员来说，如果代码上没有考虑到这点不同，便可能在代码中埋下一些Bug。
<!-- more -->

## 禁用侧滑返回功能
### 自定义返回按钮
通过自定义当前UIViewController（以下简称VC）中的`navigationItem.leftBarButtonItem`，即返回按钮。在响应的`selector`中实现pop操作，同时，也可以加入很多其他代码。

这时，侧滑返回的手势会自动失效。

### 设置`interactivePopGestureRecognizer`的`enable`属性
```obj
self.navigationController.interactivePopGestureRecognizer.enabled = NO;
```
由于这是`UINavigationController`下的属性，所以某个VC修改它后，对这个UINavigationController栈中其他的VC也是起作用的。如果我们只想在某个页面去禁掉侧滑返回的手势，那么在该页面的VC中调用如上代码后，记得在其他页面的VC中回复这个手势操作。

这时，开发者会首先想到在`viewWillAppear`或`viewDidAppear`中去设置，但一不小心，就会引起界面卡死的问题。

## Push和Pop时VC响应的方法
便于描述，假定UINavigationController中存在3个VC，依次是 **root** -> **vc1** -> **vc2**。
### vc1进入vc2（push）
- vc1.viewWillDisappear
- vc2.viewWillAppear
- vc1.viewDidDisappear
- vc2.viewDidAppear

### vc2返回按钮返回到vc1（pop）
- vc2.viewWillDisappear
- vc1.viewWillAppear
- vc2.viewDidDisappear
- vc1.viewDidAppear

### vc2右滑返回到vc1（pop）
- vc2.viewWillDisappear
- vc1.viewWillAppear
- vc2.viewDidDisappear
- vc1.viewDidAppear

### vc2右滑返回, 但中途取消 (pop->cancel)
- <刚开始滑>
- vc2.viewWillDisappear
- vc1.viewWillAppear
- <松手取消>
- vc1.viewWillDisappear
- vc1.viewDidDisappear
- vc2.viewWillAppear
- vc2.viewDidAppear

## 常见的问题
### 页面卡死，无响应
如果需求要求 **vc1** 是不允许侧滑返回的手势的，而其他页面不能受影响。开发人员的第一个想法可能就是在进入vc1时禁止掉这个手势，离开vc1时再打开这个手势，
- vc1.viewWillAppear      (self.navigationController.interactivePopGestureRecognizer.enabled=NO）
- vc1.viewWillDisappear   (self.navigationController.interactivePopGestureRecognizer.enabled=YES)

在**vc2**页面上，侧滑返回，依次响应如下几个方法：
- vc2.viewWillDisappear
- vc1.viewWillAppear

由于vc1.viewWillAppear把侧滑的手势给禁止掉了，这是侧滑无法完成，但又没有触发取消。此时页面会卡死（假死），动画都无法呈现出来，但实际相应的响应都有走到。
