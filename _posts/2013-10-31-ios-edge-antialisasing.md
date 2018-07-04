---
title: iOS抗锯齿的方式
date: 2013-10-31 16:07:01
tags:
- render
- antialisasing
categories:
- ios
---

iOS开发中，有时候展示图片等内容的时候，会出现锯齿。比如笔者最近使用 iCarousel 控件的Cover flow效果来展示几幅图片时，两侧的图片出现了较为严重的锯齿，着实不好看。这里列出两个方式：

1. 在info.plist中打开抗锯齿，但是会对影响整个应用的渲染速度；
```
Renders with edge antialisasing = YES （UIViewEdgeAntialiasing）
Renders with group opacity = YES （UIViewGroupOpacity）
```

2. View.layer.shouldRasterize = YES；
