---
title: iOS熟悉@property和set方法和get方法
date: 2018-06-05 15:37:20
tags: "iOS"
categories: "iOS基础"
---

## 前言

`@property (nonatomic, assign) CGFloat height;`

>`@property`声明了成员变量，没有自己去写它的**`set`**方法和**`get`**方法,`Xcode`系统会自动生成**`set`**和**`get`**方法。同时生成一个下划线成员变量


- 你调用的时候，赋值的值是多少就是多少

```
Person *p = [[Person alloc] init];
p.height = 170.0;
NSLog(@"%f",p.height); //打印得170.0
```

- 如果你自己写了**`set`**方法，却没有在里面做任何操作，会默认调用**`_height`**。所有的成员变量初始值都是0.所以即便你在调用**`set`**方法的时候赋值，打印出来也是0

<!--more-->

**Set方法**

```
- (void)setHeight:(CGFloat)height {

}
```

```
Person *p = [[Person alloc] init];
p.height = 170.0;
NSLog(@"%f",p.height); //打印得0.0
```

- 如果你自己写了**`get`**方法，在里面return 180.0。你在调用**`get`**方法的时候赋值，打印出来也是就是你**`get`**方法里面返回的值180.0

**Get方法**

```
- (CGFloat)height {
	return 180.0;
}
```

```
Person *p = [[Person alloc] init];
p.height = 170.0;
NSLog(@"%f",p.height); //打印得180.0
```

>`=`左边**`height`**是调用**`set`**方法

>`=`右边**`height`**是调用**`get`**方法

> 如果你自己同时生成了**`set`**和**`get`**方法，那系统就不会生成`下划线成员变量`

- 手动实现**`set`**方法和**`get`**方法
```
@synthesize height = _height;

- (void)setHeight:(CGFloat)height {
    _height = height;
}

- (CGFloat)height {
    return _height;
}
```

## Runtime

`Runtime`key值使用C语言字符串

`static char imageURLKey` = `&imageURLKey`

`static char imageURLKey = "imageURLKey"`