---
title: 糟糕的 [@[] mutableCopy] 和 [@{} mutableCopy]
date: 2017-01-11 16:32:41
tags:
- objc
- mutablecopy
categories:
- ios
---

### 糟糕的写法
翻阅项目中的老代码，发现一百来处如下的写法：
```objc
NSMutableArray* array = [@[] mutableCopy];
NSMutableDictionary *dict = [@{} mutableCopy];
```

这种写法会多构造一个临时的对象，虽然随后就会回收销毁掉，毕竟多了内存申请释放的开销；从语法角度，这个写法也并不能节省多少时间，表意也不简明。
<!-- more -->

### 总结
应该使用标准的创建对象的方式:
```objc
NSMutableArray* array = [NSMutableArray array];
NSMutableDictionary* dict = [NSMutableDictionary dictionary];
```
