---
title: iOS8上CGPathAddLineToPoint引起的crash
date: 2017-01-09 14:35:30
tags:
- ios8
- crash
categories:
- ios
---

### Crash日志
![image](/assets/images/ios8-crash-1.png)

如果传入的CGPoint的值是NaN, 就可能触发这个Crash:
```objc
CGPoint pt = {
    [[NSDecimalNumber notANumber] doubleValue],
    [[NSDecimalNumber notANumber] doubleValue]};
CGPathAddLineToPoint(pt); // iOS8 crash here!
```
通过上述代码，模拟该crash，同时会在控制台看到如下输出：
```
Assertion failed: (CGFloatIsValid(x) && CGFloatIsValid(y)), function void CGPathAddLineToPoint(CGMutablePathRef, const CGAffineTransform *, CGFloat, CGFloat), file Paths/CGPath.cc, line 265.
```
<!-- more -->
### 判断有效性
在`math.h`中存在如下宏定义:
``` c
#define isnan(x)                                                         \
    ( sizeof(x) == sizeof(float)  ? __inline_isnanf((float)(x))          \
    : sizeof(x) == sizeof(double) ? __inline_isnand((double)(x))         \
                                  : __inline_isnanl((long double)(x)))

```

### 结语
以上crash只存在于iOS8的各版本系统，猜测CGPathAddLineToPoint这个函数的内部实现中，增加了断言，正式发布时未被去掉。解决方案可以先判断CGPoint的有效性，然后再进行绘制。
