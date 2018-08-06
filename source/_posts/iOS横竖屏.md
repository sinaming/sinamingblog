---
title: iOS横竖屏
date: 2018-08-06 15:13:34
tags: "iOS"
categories: "iOS开发"
---

### 前言

>熟悉`iOS`框架结构,一般都是`UITabBarController`结合`UINavigationController`,所有需要配置信息

- `UITabBarController`配置
```
- (BOOL)shouldAutorotate{
    return [self.selectedViewController shouldAutorotate];
}
- (UIInterfaceOrientationMask)supportedInterfaceOrientations{
    return [self.selectedViewController supportedInterfaceOrientations];
}
- (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation{
    return [self.selectedViewController preferredInterfaceOrientationForPresentation];
}
```

<!--more-->

- `UINavigationController`配置
```
- (BOOL)shouldAutorotate {
    return [[self.viewControllers lastObject] shouldAutorotate];
}

- (UIInterfaceOrientationMask)supportedInterfaceOrientations {
    return [[self.viewControllers lastObject] supportedInterfaceOrientations];
}

- (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation{
    return [[self.viewControllers lastObject] preferredInterfaceOrientationForPresentation];
}
```
> 一般应用app开发都是个别页面需要设置好横屏设置,所有在基类里面设置所有的类不能横屏
```
-(UIInterfaceOrientationMask)supportedInterfaceOrientations
{
    return UIInterfaceOrientationMaskPortrait ;
}
- (BOOL)shouldAutorotate
{
    return NO;
}
-(UIInterfaceOrientation)preferredInterfaceOrientationForPresentation
{
    return UIInterfaceOrientationPortrait;
}
- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation{
    return NO;
}
```

- 旋转`ViewController`

```
// 允许屏幕旋转设置
- (BOOL)shouldAutorotate {
    return YES;
}
-(UIInterfaceOrientationMask)supportedInterfaceOrientations
{
    return UIInterfaceOrientationMaskAll;
}
- (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation
{
    return UIInterfaceOrientationPortrait;
}
- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation
{
    return YES;
}

// 设置状态栏信息
//设置样式
- (UIStatusBarStyle)preferredStatusBarStyle {
    return UIStatusBarStyleDefault;
}

//设置是否隐藏
- (BOOL)prefersStatusBarHidden {
    return NO;
}

//设置隐藏动画
- (UIStatusBarAnimation)preferredStatusBarUpdateAnimation {
    return UIStatusBarAnimationFade;
}
```

> 页面设置跳转的逻辑

```
//iOS8旋转动作的具体执行
- (void)viewWillTransitionToSize:(CGSize)size withTransitionCoordinator:(id<UIViewControllerTransitionCoordinator>)coordinator {
    [super viewWillTransitionToSize:size withTransitionCoordinator: coordinator];
    // 监察者将执行： 1.旋转前的动作  2.旋转后的动作（completion）
    [coordinator animateAlongsideTransition: ^(id<UIViewControllerTransitionCoordinatorContext> context) {
        if ([XMRotatoUtil isOrientationLandscape]) {
            self->_lastOrientation = [UIApplication sharedApplication].statusBarOrientation;
            [self prepareFullScreen];
        } else {
            [self prepareSmallScreen];
        }
    } completion: ^(id<UIViewControllerTransitionCoordinatorContext> context) {
        
    }];
}

//iOS7旋转动作的具体执行
- (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration {
    [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
    if (toInterfaceOrientation == UIDeviceOrientationLandscapeRight) {
        _lastOrientation = [UIApplication sharedApplication].statusBarOrientation;
        [self prepareFullScreen];
    } else {
        [self prepareSmallScreen];
    }
}
```
> 考虑到横屏,默认状态栏会隐藏,需要在`info.plist`文件中`View controller-based status bar appearance`为YES

### 最后

> 在横屏页面所有的都采用自动布局来达到对于的效果


```
// 屏幕方法调用
+ (void)forceOrientation: (UIInterfaceOrientation)orientation {
    if ([[UIDevice currentDevice] respondsToSelector:@selector(setOrientation:)]) {
        SEL selector = NSSelectorFromString(@"setOrientation:");
        NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:[UIDevice instanceMethodSignatureForSelector:selector]];
        [invocation setSelector:selector];
        [invocation setTarget: [UIDevice currentDevice]];
        int val = orientation;
        [invocation setArgument:&val atIndex:2];
        [invocation invoke];
    }
}

+ (BOOL)isOrientationLandscape {
    if (UIInterfaceOrientationIsLandscape([UIApplication sharedApplication].statusBarOrientation)) {
        return YES;
    } else {
        return NO;
    }
}
```

> 需要配置Deplymengt info勾选竖屏和向右横屏,以解决横屏问题
