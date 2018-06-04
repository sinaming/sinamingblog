---
title: iOSPush任意新页面
date: 2018-05-10 10:19:01
tags: "iOS"
categories: "iOS开发"
---
## APP任意push新页面

>平时`push`一般都是`[self.navigationController pushViewController:newVC animated:YES];`

## 看黑板

>现在通过`UIApplication`添加分类获取`UINavigationController`来实现`push`

<!--more-->

```
- (UIWindow *)mainWindow {
    return self.delegate.window;
}

- (UIViewController *)visibleViewController {
    UIViewController *rootViewController = [self.mainWindow rootViewController];
    return [self getVisibleViewControllerFrom:rootViewController];
}

- (UIViewController *) getVisibleViewControllerFrom:(UIViewController *) vc {
    if ([vc isKindOfClass:[UINavigationController class]]) {
        return [self getVisibleViewControllerFrom:[((UINavigationController *) vc) visibleViewController]];
    } else if ([vc isKindOfClass:[UITabBarController class]]) {
        return [self getVisibleViewControllerFrom:[((UITabBarController *) vc) selectedViewController]];
    } else {
        if (vc.presentedViewController) {
            return [self getVisibleViewControllerFrom:vc.presentedViewController];
        } else {
            return vc;
        }
    }

}

- (UINavigationController *)visibleNavigationController {
    return [[self visibleViewController] navigationController];
}
```

## 调用

```
UINavigationController *navigationController = [[UIApplication sharedApplication] visibleNavigationController];
[navigationController pushViewController:newVC animated:YES];
```

## 参考文档
[iOS - APP任意push新页面那些事](https://blog.csdn.net/qq_34047841/article/details/59482767)