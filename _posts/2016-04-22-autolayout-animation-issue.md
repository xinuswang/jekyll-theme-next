---
title: iOS中AutoLayout引起的一个动画问题
date: 2016-04-22 13:43:45
tags:
- animation
- autolayout
categories:
- ios
---

### 动画问题及原因
我做了一个ConnectingView，用来显示一个连接中的动画。动画中有三部分，手机图标、服务器图标、循环移动的虚线。大致的示意如下：

    [App] = - - - - [Server]
    [App] - = - - - [Server]
    [App] - - = - - [Server]
    [App] - - - = - [Server]
    [App] - - - - = [Server]

整个ConnectingView用AutoLayout布局，然后动画部分通过改变进度条‘=’与左边[App]的间距约束来实现。然后再ViewController的viewDidLoad方法中，实例化ConnectingView，并通过代码设置AutoLayout放置在view中，然后startAnimation。
<!-- more -->

接着发现一个动画问题，再部分手机上，进入这个ViewController并开始动画时，进度条‘=’的初始位置可能会超过[App], 但第2次循环时，不会超过。

经过调查，这个问题的原因在于动画开始的时候，与AutoLayout重新布局存在时间上的冲突。

ConnectingView通过XIB设计，ViewController是在StoryBoard中设计。ViewController的viewDidLoad是程序载入StoryBoard中的设计完成，而初始的尺寸也就是Storyboard的尺寸；ConnectingView通过XIB实例化，初始的尺寸也是XIB的尺寸，虽然我们设置了AutoLayout，但整体的布局还没有适配屏幕；这个时候我们开始动画，也就是计算动画的初始位置都还是原始的尺寸。viewDidLoad完成后，再viewDidAppear之前的时间节点中，ViewController根据需要整体重新布局，而我们的动画已经照旧的位置已经开始，这时候的冲突就造成了前面提到的问题。

### 最后的解决
开始，我尝试过让动画开始的时间延后一定时间开始，或者干脆到ViewDidAppear中再开始动画，而不是viewDidLoad。这样虽然不会出现上面提到的问题，但明显能感觉到延迟，约0.5s左右，肉眼可以感知。

后来，我尝试把UIView这一层的动画，改为CALayer这一层的动画，问题就完美解决了。这也许是因为我不通过设置View本身属于AutoLayout中的约束（Constraint）来进行动画，而通过CALayer这一层避开了AutoLayout适应布局时的冲突。
