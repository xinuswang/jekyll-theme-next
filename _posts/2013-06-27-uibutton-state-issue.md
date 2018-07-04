---
title: UIButton设置了UIControlStateSelected和UIControlStateHighlighted状态的图片点击会闪烁的解决方案
date: 2013-06-27 17:59:17
tags:
- uibutton
categories:
- ios
---

通过代码增加如下设置:
``` objc
[btn setBackgroundImage:img1 forState:UIControlStateSelected|UIControlStateHighlighted];
[btn setImage:img2 forState:UIControlStateSelected|UIControlStateHighlighted];
```

注: 在界面设计器中无法设置。
