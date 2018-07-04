---
title: iOS/Objective-C 开发常用代码(1)
date: 2012-12-16 22:38:19
tags:
- codefragments
- objc
categories:
- ios
---

#### 判断iOS设备是否是iPad
```objc
#define IS_IPAD ([[UIDevice currentDevice] respondsToSelector:@selector(userInterfaceIdiom)] && [[UIDevice currentDevice] userInterfaceIdiom] == UIUserInterfaceIdiomPad)
```

#### iOS中URL编码
```objc
NSString* escapedURLString = [unescapedString
stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding]
```
或
```objc
NSString * encodedString = (NSString *)CFURLCreateStringByAddingPercentEscapes
(NULL, (CFStringRef)yourtext, NULL,
(CFStringRef)@”!*’();:@&=+$,/?%#[]“, kCFStringEncodingUTF8);
```

#### iOS中不定参数（可变参数）的方法
常见于NSArray初始化方法中，比如：
```objc
@interface NSArray (NSArrayCreation)
+ (id)arrayWithObjects:(id)firstObj, ... NS_REQUIRES_NIL_TERMINATION;
- (id)initWithObjects:(id)firstObj, ... NS_REQUIRES_NIL_TERMINATION;
//...
@end
```
NS_REQUIRES_NIL_TERMINATION 是一个宏，用于编译时非nil结尾的检查。自定义不定参数的方法与C/C++一样，示例如下：
```objc
- (id)initWithColumns: (NSString*)firstColumnName, ... {
    if (self = [self init]) {
        NSMutableArray* arrays = [NSMutableArray array];
        va_list argList;
        if (firstColumnName) {
            [arrays addObject:firstColumnName];
            va_start(argList, firstColumnName);
            id arg;
            while ((arg = va_arg(argList, id))) {
                [arrays addObject:arg];
            }
        }
        self.columnNames = [NSArray arrayWithArray:arrays];
    }
    return self;
}
```
