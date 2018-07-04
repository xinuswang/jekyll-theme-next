---
title: iOS开发陷阱之NSString - compare
date: 2013-01-26 15:42:11
tags:
- nsstring
categories:
- ios
---

NSString有多个compare相关方法：
```objc
- (NSComparisonResult)compare:(NSString *)string;
- (NSComparisonResult)compare:(NSString *)string options:(NSStringCompareOptions)mask;
- (NSComparisonResult)compare:(NSString *)string options:(NSStringCompareOptions)mask range:(NSRange)compareRange;
- (NSComparisonResult)compare:(NSString *)string options:(NSStringCompareOptions)mask range:(NSRange)compareRange locale:(id)locale;
```
**NSComparisonResult** 是定义的一个枚举，定义如下：
```objc
typedef NS_ENUM(NSInteger, NSComparisonResult) {NSOrderedAscending = -1L, NSOrderedSame, NSOrderedDescending};
```
其中，**NSOrderedSame** 表示比较的两个字符串完全一致, 同时，在这个枚举中，它的值是 0.
<!-- more -->
字符串比较在程序中很常见，比如：
```objc
if ([str1 compare:@"some text"] == NSOrderedSame) {
    // Do something
}
else {
    // Do something else
}
```
但，如果如上中的str1为nil，根据Objective-C的消息调用规则（方法调用），对nil发送的任何消息，得到的返回都是nil。这样的情况下，运行时是不会像C/C++那样，出现空指针的非法访问而使得程序强行终止。也就是说，在Objective-C下面，即便str1为nil，也不会造成程序崩溃，而是会继续运行。

那么当str1为空的时候，[str1 compare:@"some text"] 消息的返回就会为nil。nil表示一个空的Objective-C对象，实际就是表示一个空指针，而它代表的值就是0，与NSOrderedSame的值相等. 如此，回到最前面的if语句，如果str1为nil，那么整个语句的值为真。这会给程序造成非常严重的问题，小则逻辑错误，UI显示错误等，大则会造成数据泄漏等等。。。所以，一旦出现这种情况，还是很严重的。

笔者个人建议，以上代码至少应该写为：
```objc
if (str1!=nil && [str1 compare:@"some text"] == NSOrderedSame) {
    // Do something
}
else {
    // Do something else
}
```
