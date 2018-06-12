---
title: ios之界面之间的数据正逆向传递方法
date: 2018-06-08 17:02:22
tags: "iOS"
categories: "iOS基础"
---

## 1. 初始化传值

> 自定义`View`初始化方法.h文件

```
#import <UIKit/UIKit.h>

@interface XMContentView : UIView

- (instancetype)initTitles:(NSArray <NSString *>*)titles;

@end
```

> 自定义`View`初始化方法.m文件

```
#import "XMContentView.h"

@interface XMContentView ()

// 数据保存
@property (nonatomic, strong) NSArray *titles;

@end

@implementation XMContentView

- (instancetype)initTitles:(NSArray<NSString *> *)titles {
    if (self = [super init]) {
        // 进行页面数据传递
    }
    return self;
}

@end
```

<!--more-->

> 初始化传值使用

```
- (void)viewDidLoad {
    [super viewDidLoad];
    
    _contentView = [[XMContentView alloc] initTitles:@[@"初始化传值1",@"初始化传值2"]];
    [self.view addSubview:_contentView];

}
```

## 2. 属性传值

> 自定义`View`添加属性.h文件

```
#import <UIKit/UIKit.h>

@interface XMContentView : UIView

@property (nonatomic, copy) NSString *titles;

@end
```

> 自定义`View`添加属性.m文件

```
#import "XMContentView.h"

@interface XMContentView ()


@end

@implementation XMContentView

- (void)setTitles:(NSString *)titles {
    _titles = titles;
    // 进行页面数据传递
}

@end
```

> 属性传值使用

```
- (void)viewDidLoad {
    [super viewDidLoad];

    _contentView = [[XMContentView alloc] initWithFrame:CGRectZero];
    _contentView.titles = @"属性传值";
    [self.view addSubview:_contentView];

}
```

## 3. 方法传值

> 自定义`View`添加方法.h文件

```
#import <UIKit/UIKit.h>

@interface XMContentView : UIView

- (void)reloadContentViewTitles:(NSArray <NSString *>*)titles;

+ (void)reloadContentViewNames:(NSString *)names;

@end
```

> 自定义`View`添加方法.m文件

```

#import "XMContentView.h"

@interface XMContentView ()

@end

@implementation XMContentView

- (void)reloadContentViewTitles:(NSArray<NSString *> *)titles {
    // 进行页面传递数据配置
}

+ (void)reloadContentViewNames:(NSString *)names {
    // 进行页面传递数据配置
}

@end
```

> 方法传值使用

```
- (void)viewDidLoad {
    [super viewDidLoad];

    _contentView = [[XMContentView alloc] initWithFrame:CGRectZero];
    [_contentView reloadContentViewTitles:@[@"方法传值1",@"方法传值2"]];
    [XMContentView reloadContentViewNames:@"方法传值"];
    [self.view addSubview:_contentView];

}
```

## 4. Delegate(协议)传值

> 自定义`View`添加协议.h文件

```
#import <UIKit/UIKit.h>

@protocol XMContentViewDelegate <NSObject>
// delegate 必须实现的方法
@required
- (void)sendInformation:(NSInteger)tag;
// delegate 选择实现的方法
@optional

@end

@interface XMContentView : UIView

@property (nonatomic, weak) id<XMContentViewDelegate> delegate;

@end
```

> 自定义`View`添加协议.m文件

```
#import "XMContentView.h"

@interface XMContentView ()

@end

@implementation XMContentView

- (instancetype)initWithFrame:(CGRect)frame {
    if (self = [super initWithFrame:frame]) {
    	// 一般用于反向传值,按钮或者手势处理
        if (_delegate && [_delegate respondsToSelector:@selector(sendInformation:)]) {
            [_delegate sendInformation:0];
        }
    }
    return self;
}

@end
```

> 协议传值使用

```
// 引用协议
@interface ViewController ()<XMContentViewDelegate>

@end

- (void)viewDidLoad {
    [super viewDidLoad];

    _contentView = [[XMContentView alloc] initWithFrame:CGRectZero];
    // 遵守协议
    _contentView.delegate = self;
    [self.view addSubview:_contentView];
 
}

// 实现协议传值
- (void)sendInformation:(NSInteger)tag {
    
}

```

## 5. Block传值

> 自定义`View`定义Block.h文件

```
#import <UIKit/UIKit.h>

typedef void(^contentBlock)(NSString *titles);

@interface XMContentView : UIView

@property (nonatomic, copy) contentBlock contentblk;

@property (nonatomic, copy) dispatch_block_t contentdisblk;

// block第一个参数为返回值,第二个参数为block名字,第三个参数为携带的参数
@property (nonatomic, copy) void (^contentBlock)(void);

- (void)reloadBlock:(void (^)(NSString *titles))animations;

// 方法和属性设置block
//  returnType(^name)(arguments);

// - void)reloadBlock:(returnType(^name)(arguments))animations;


@end
```

> 自定义`View`定义Block.m文件

```
#import "XMContentView.h"

@interface XMContentView ()

@end

@implementation XMContentView

- (instancetype)initWithFrame:(CGRect)frame {
    if (self = [super initWithFrame:frame]) {
      
    }
    return self;
}

- (void)reloadBlock:(void (^)(NSString *titles))animations {
   // 进行页面传递数据配置
}

@end
```

> Block传值使用

```
- (void)viewDidLoad {
    [super viewDidLoad];

    // block中处理weak属性,防止循环引用
    __weak typeof(self) weakSelf = self;

    _contentView = [[XMContentView alloc] initWithFrame:CGRectZero];
    [_contentView reloadBlock:^(NSString *titles) {
        
    }];
    _contentView.contentBlock = ^{
        
    };
    [self.view addSubview:_contentView];

}
```

## 6. 单例传值

> 自定义单例.h文件

```
#import @interface nameObject : NSObject
/**
 单例类方法
 @return 返回一个共享对象
 */
+ (instancetype)sharedInstance;

// 姓名
@property (nonatomic, copy) NSString* name;

@end
```

> 自定义单例.m文件

```
#import "nameObject.h"

@implementation nameObject

static nameObject* kNameObject = nil;

/** 单例类方法 */
// GCD
+ (instancetype)sharedInstance {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
    	<!-- if (kNameObject == nil) { -->
    	    kNameObject = [[super allocWithZone:NULL] init];
    	<!-- } -->
   });
    return kNameObject;
}


// 同步锁
/*
+ (instancetype)sharedInstance {
	volatile static LaunchIntroductionView *singleInstance = nil;
	    if (singleInstance == nil) {
	        @synchronized(self) {
	            if (singleInstance == nil) {
	                singleInstance = [[LaunchIntroductionView alloc] init];
	            }
	        }
	    }
	    return singleInstance;
}
*/

+ (id)allocWithZone:(struct _NSZone *)zone {
	static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        kNameObject = [super allocWithZone:zone];
    });
    return kNameObject;
	<!-- return [self sharedInstance]; -->
}

- (id)copy {
	<!-- return self; -->
	return kNameObject;
}

- (id)muntableCope {
	<!-- return self; -->
	return kNameObject;
}

@end

```

> 单例传值使用

```
- (void)viewDidLoad {
    [super viewDidLoad];

	// [nameObject sharedInstance].name    

}
```

## 7. 通知传值

**编写通知命名** 
```
UIKIT_EXTERN NSNotificationName const ZOCFooDidBecomeBarNotification
NSNotificationName const ZOCFooDidBecomeBarNotification = @"ZOCFooDidBecomeBarNotification";
```

> 通知.h文件,设置通知名字`extern NSString * const aNotification;`

```
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(requestName:) name:aNotification object:nil];
```

> 通知.m文件,设置通知名字`NSString * const aNotification = @"aNotification";`

**实现`requestName`方法**

`dealloc`移除通知


## NSUserDefaults数据保存

> 统一方法保存
```
+ (void)saveValueString:(NSString *)valueString withKey:(NSString *)keyString;

+ (NSString *)getValueStringWithKey:(NSString *)keyString;

+ (void)cleanValueStringWithKey:(NSString *)keyString;
```

> 统一方法实现

```
+ (void)saveValueString:(NSString *)valueString withKey:(NSString *)keyString {
    NSUserDefaults *uidDefaults = [NSUserDefaults standardUserDefaults];
    [uidDefaults setObject:valueString forKey:keyString];
    [uidDefaults synchronize];
}

+ (NSString *)getValueStringWithKey:(NSString *)keyString {
    NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];
    NSString *valueString = [userDefaults objectForKey:keyString];
    return valueString;
}

+ (void)cleanValueStringWithKey:(NSString *)keyString {
    NSUserDefaults *UserLoginState = [NSUserDefaults standardUserDefaults];
    [UserLoginState removeObjectForKey:keyString];
    [UserLoginState synchronize];
}
```



