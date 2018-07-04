---
title: iOS全局处理键盘事件
date: 2013-01-22 19:10:36
tags:
- keyboard
categories:
- ios
---

#### 注册监听键盘事件的通知
```objc
[[NSNotificationCenter defaultCenter] addObserver:self
                                         selector:@selector(keyboardWillShow:)
                                             name:UIKeyboardWillShowNotification
                                           object:nil];

[[NSNotificationCenter defaultCenter] addObserver:self
                                         selector:@selector(keyboardShow:)
                                             name:UIKeyboardDidShowNotification
                                           object:nil];

[[NSNotificationCenter defaultCenter] addObserver:self
                                         selector:@selector(keyboardWillHide:)
                                             name:UIKeyboardWillHideNotification
                                           object:nil];

[[NSNotificationCenter defaultCenter] addObserver:self
                                         selector:@selector(keyboardHide:)
                                             name:UIKeyboardDidHideNotification
                                           object:nil];

```

#### 在键盘将要出现和隐藏的回调中，加入动画
``` objc
- (void)keyboardWillShow:(NSNotification *)notif {
    if (self.hidden == YES) {
        return;
    }

    CGRect rect = [[notif.userInfo objectForKey:UIKeyboardFrameEndUserInfoKey] CGRectValue];
    CGFloat y = rect.origin.y;
    [UIView beginAnimations:nil context:nil];
    [UIView setAnimationDuration:0.25];
    NSArray *subviews = [self subviews];
    for (UIView *sub in subviews) {

        CGFloat maxY = CGRectGetMaxY(sub.frame);
        if (maxY > y - 2) {
            sub.center = CGPointMake(CGRectGetWidth(self.frame)/2.0, sub.center.y - maxY + y - 2);
        }
    }
    [UIView commitAnimations];
}

- (void)keyboardShow:(NSNotification *)notif {
    if (self.hidden == YES) {
        return;
    }
}

- (void)keyboardWillHide:(NSNotification *)notif {
    if (self.hidden == YES) {
        return;
    }
    [UIView beginAnimations:nil context:nil];
    [UIView setAnimationDuration:0.25];
    NSArray *subviews = [self subviews];
    for (UIView *sub in subviews) {
        if (sub.center.y < CGRectGetHeight(self.frame)/2.0) {
            sub.center = CGPointMake(CGRectGetWidth(self.frame)/2.0, CGRectGetHeight(self.frame)/2.0);
        }
    }
    [UIView commitAnimations];
}

- (void)keyboardHide:(NSNotification *)notif {
    if (self.hidden == YES) {
        return;
    }
}

```
