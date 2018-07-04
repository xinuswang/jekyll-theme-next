---
title: 清理iOS工程中的Warnings
date: 2016-08-12 10:34:37
tags:
- warnings
categories:
- ios
---

### 直接屏蔽对应的警告
可以添加编译器选项，全局屏蔽掉某个警告，例如：
``` objc
-Wno-deprecated
```
也可以屏蔽掉某段代码中的指定警告，例如：
``` objc
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Warc-performSelector-leaks"
// Existing code here!
#pragma clang diagnostic pop
```
<!-- more -->
### Pods中的Warnings
CocoaPods禁止显示警告，Pod文件的头部中添加 `inhibit_all_warnings!`，这个实际上会在导入的第三方代码中，自动添加编译选项`-w -Xanalyzer -analyzer-disable-all-checks`。

### Unused variable 'xxx'
没有使用到的变量，可能由于长期维护删减代码，部门声明的变量已经没有用到，可以采用的方式有：
- 注释掉该代码或直接删除；
- 添加_unused关键字；

### Method 'xxx' in protocol 'yyy' not implemented
声明在某个class中实现了yyy协议，但是并没有实现xxx方法。
1. 首先应该排查逻辑错误，如果没有实现一个required的方法很可能会引起逻辑错误，特别时当该方法有返回值的时候；
2. 其次如果protocol也是自己的代码，则审查一下是否该method可以标记为optional；
3. 然后实现该方法，如果实在没有用到则添加一个空的实现。

### Deprecations
代码中使用到了被SDK标记为`deprecated`的类或方法，往往这些类或者方法的定义处，都会提示替代的方式，按照所提示的替代换用新的类或者方法即可。

### Switch condition has boolean value
对一个Boolean类型的值用switch语句。这个改成if-else语句即可，同时也需要去审查一下该部分的逻辑，是否有其他问题。

### Implicit conversion loses integer precision: 'long' to 'int'
常见于包含的C/C++代码，往往是用到的第三方代码。可以采用关闭该警告的方式去除。
```
Apple LLVM 7.1 - Warnings Policies - Implicit Conversion to 32 Bit Type - No
```

### PerformSelector may cause a leak because its selector is unknown
代码中使用`performSelector`时会引起该警告，首选需要确认的是代码中已经明确通过`respondsToSelector`判断是否存在使用到的方法，然后才调用的`performSelector`。可以通过局部屏蔽改警告的方式处理。
``` objc
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Warc-performSelector-leaks"
// Existing code here!
#pragma clang diagnostic pop
```

### Values of type 'NSInteger' should not be used as format arguments; add an explicit cast to 'long' instead
在模拟器和真机上，`NSInteger`是不同的类型定义：
``` objc
#if __LP64__ || (TARGET_OS_EMBEDDED && !TARGET_OS_IPHONE) || TARGET_OS_WIN32 || NS_BUILD_32_LIKE_64
  typedef long NSInteger;
  typedef unsigned long NSUInteger;
#else
  typedef int NSInteger;
  typedef unsigned int NSUInteger;
#endif
```
在格式化字符串时，使用'%ld'会在真机中报该警告。解决办法：
- use %zd for signed, %tu for unsigned, and %tx for hex.

### Incompatible pointer to integer conversion initializing 'BOOL' (aka 'signed char') with an expression of type 'NSNumber * '
把一个`NSNumber`对象赋值给了`BOOL`变量，这个往往会导致严重的逻辑问题，因为只要NSNumber对象不为nil，BOOL类型的变量都将是`YES`。需要修改代码：
``` objc
NSNumber* value = @(0);
// BOOL ok = value;
BOOL ok = [value boolValue];
```
