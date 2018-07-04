---
title: 使用iOS 10 SDK编译的警告
date: 2016-08-17 15:34:23
tags:
- ios10
- warnings
categories:
- ios
---

### 升级XCode 8 beta
XCode 8 Beta使用了iOS 10 SDK为默认的SDK；目前的工程，使用XCode 8 Beta编译时，提示了一些警告信息。

### Method possibly missing a [super awakeFromNib] call
使用XCode 7时并没有报出这样的警告，其实故名思议，需要在`awakeFromNib`方法里面调用其父类的方法。
```objc
- (void)awakeFromNib {
  [super awakeFromNib];
  // Initialization code
}
```

关于这点，Apple的官方文档中也有说明:
> You must call the super implementation of awakeFromNib to give parent classes the opportunity to perform any additional initialization they require. Although the default implementation of this method does nothing, many UIKit classes provide non-empty implementations. You may call the super implementation at any point during your own awakeFromNib method.

<!-- more -->
### CAAnimationDelegate
`CAAnimationDelegate`在iOS 9 SDK及之前的SDK中，作为NSObject的**Category**：
```objc
@interface NSObject (CAAnimationDelegate)
- (void)animationDidStart:(CAAnimation *)anim;
- (void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag;
@end
```

`CAAnimationDelegate`在iOS 10 SDK中作为**Protocol**需要被类实现：
```objc
@protocol CAAnimationDelegate <NSObject>
@optional
- (void)animationDidStart:(CAAnimation *)anim;
- (void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag;
@end

```

如果要兼容XCode 7 和XCode 8，则可以通过判断SDK版本来实现：
```objc
#if __IPHONE_OS_VERSION_MAX_ALLOWED >= 100000
@interface SomeClass <AnyProtocal, CAAnimationDelegate>
#else
@interface SomeClass <AnyProtocal>
#endif

- (void)anyMethod;
@end
```

### CALayerDelegate
与前面提到的`CAAnimationDelegate`类似，`CALayerDelegate`在iOS 9 SDK及之前的SDK中，作为NSObject的**Category**：
```objc
@interface NSObject (CALayerDelegate)
- (void)displayLayer:(CALayer *)layer;
- (void)drawLayer:(CALayer *)layer inContext:(CGContextRef)ctx;
- (void)layoutSublayersOfLayer:(CALayer *)layer;
- (nullable id<CAAction>)actionForLayer:(CALayer *)layer forKey:(NSString *)event;
@end
```

iOS 10 SDK中作为**Protocol**需要被类实现：
```objc
@protocol CALayerDelegate <NSObject>
@optional
- (void)displayLayer:(CALayer *)layer;
- (void)drawLayer:(CALayer *)layer inContext:(CGContextRef)ctx;
- (void)layerWillDraw:(CALayer *)layer CA_AVAILABLE_STARTING (10.12, 10.0, 10.0, 3.0);
- (void)layoutSublayersOfLayer:(CALayer *)layer;
- (nullable id<CAAction>)actionForLayer:(CALayer *)layer forKey:(NSString *)event;
@end

```
如果要兼容XCode 7 和XCode 8，则可以通过判断SDK版本来实现，同`CAAnimationDelegate`。
