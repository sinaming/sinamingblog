---
title: iOS仿淘宝详情页导航栏
date: 2018-06-02 16:41:58
tags: "iOS"
categories: "iOS开发"
---

## 前提
>当看到好的App应用,都想好好的了解下他们的实现

## 分析
>正常的App应用是不能分析App的结构的,所以我们只能借助越狱的手机,对App进行分析,如果没有越狱手机我们一样的可以分析App的结构

1. `Mac`下载`PP助手`程序,再越狱应用中搜索`参考的App`

2. 下载`参考的App`,对`App`进行解压,获取应用安装包

3. 安装工具,具体的工具自行搜索百度(`逆向分析工具`)

4. 安装好工具,把应用安装包放入工具中,运行安装等等(更多的逆向自行搜索)

<!--more-->

5. 通过Xcode工具UI界面分析工具查看淘宝详情页的页面

> 了解,UI界面分析完成之后,大概知道了他的实现View,查看UI界面分析得到的淘宝详情页导航栏实现的View

## 实现

>开始我们的实现,如上的的分析,我们得到了淘宝详情页的大致结构,跳转页面隐藏系统导航栏,添加导航栏View

**看代码**

### 隐藏导航栏(第一步)
>`UINavigationControllerDelegate`实现代理,侧滑的同时也不会隐藏导航栏的正确显示和隐藏

```
- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    self.navigationController.delegate = self;
}

- (void)navigationController:(UINavigationController *)navigationController willShowViewController:(UIViewController *)viewController animated:(BOOL)animated {
    // 判断要显示的控制器是否是自己
    BOOL isShowHomePage = [viewController isKindOfClass:[self class]];
    [self.navigationController setNavigationBarHidden:isShowHomePage animated:YES];
}
```

### 创建导航栏View(第二步)

>创建导航栏View,里面放置详情页面需要的按钮信息和跳转逻辑,`Controller`添加完`UITableView`,只会添加导航栏View,他们都同样的添加的`Controller`的View上
```
  // tableView
  [self.view addSubview:self.tableView];

  self.tableView.tableHeaderView = self.tableHeadLabel;
  // 导航栏View
  [self.view addSubview:self.floatView];

- (void)viewWillLayoutSubviews {
    [super viewWillLayoutSubviews];
    // 让导航栏View显示最上层,backgroundView设置成透明
    [self.view bringSubviewToFront:self.floatView];
}

```

>完成上面的操作,是不能滑动tableView的

### 解决
**分析iOS**我们通过响应链知识,导航栏View重写`- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event`方法,具体看代码
```
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event {
//    UIView *tmpView = [super hitTest:point withEvent:event];
//    if (tmpView.superview == self) {
//        return nil;
//    }
//    return tmpView;
    
//    处理的关键点在于判断条件
//
//    tmpView.superview == self!
//
//    如果需要穿透UIView，则变更为tmpView == self
    
    UIView *tmpView = [super hitTest:point withEvent:event];
    if (tmpView == self) {
        return nil;
    }
    return tmpView;
}

```

## 完成
完成上面的操作,这个手势滑动导航栏View,就能响应`tableView`的手势了

>上面的逻辑大致可以实现大部分功能了,所以多查查资料可以更好的提升自己的能力






