---
title: Masonry设置UIScrollView的contentSize,实现复杂页面开发
date: 2018-05-08 15:09:22
tags: "iOS"
categories: "iOS开发"
---
## 前言

>开发项目中,遇到复杂页面开发,今天就使用`Masonry+UIScrollView`实现复杂页面开发

## 实例图

![复杂UIScrollView](Masonry设置UIScrollView的contentSize/复杂UIScrollView.gif)

<!--more-->

- 通过看上面的效果图,是否觉得这页面是否很复杂,不同的有不同的解答方法,今天,我就介绍下我个人的解决方案

## 看黑板

> 首先,我们先分析一波这个页面的具体内容和复杂程度,是否考虑使用`UITableView`或者`UIScrollView`来完成这个需求,我相信`UITableView`也是能实现这个方案,可能是我太懒!

> 我们可以分为三个模块,从顶部到联系客服下面的横线为第一个模块,简称`TopView`,第二个模块图文详情模块,简称`CenterView`,第三个模块腕表参数模块,简称`BottomView`,命名请忽视,我们可以自定义三个View,分别实现里面的内容显示

**`TopView`**

```
[self addSubview:self.testLabel];
[self.testLabel mas_makeConstraints:^(MASConstraintMaker *make) {
    make.top.left.right.mas_equalTo(self).offset(0);
}];

[self addSubview:self.testButton];
[self.testButton mas_makeConstraints:^(MASConstraintMaker *make) {
    make.top.mas_equalTo(self.testLabel.mas_bottom).offset(0);
    make.left.right.mas_equalTo(self).offset(0);
}];

[self addSubview:self.testImageView];
[self.testImageView mas_makeConstraints:^(MASConstraintMaker *make) {
    make.top.mas_equalTo(self.testButton.mas_bottom).offset(0);
    make.left.right.mas_equalTo(self).offset(0);
}];

// 重点,必须设置这个约束,确定底部的位置
[self mas_makeConstraints:^(MASConstraintMaker *make) {
    make.bottom.mas_equalTo(self.testImageView.mas_bottom).offset(0);
}];

```

* `CenterView`和`BottomView`同样设置从顶部和底部控件位置,确定当前`containerView`(容器)底部约束**这个尤为重要**

**Controller**

> 父视图
`@property (nonatomic, strong) UIScrollView *scrollView;`

> 容器视图
`@property (nonatomic, strong) UIView *containerView;`

> 分别创建好这两个

```
[self.view addSubview:self.scrollView];
[self.scrollView mas_makeConstraints:^(MASConstraintMaker *make) {
    make.edges.equalTo(self.view);
}];

[self.scrollView addSubview:self.containerView];
[self.containerView mas_makeConstraints:^(MASConstraintMaker *make) {
    make.edges.equalTo(self.scrollView);
    make.width.equalTo(self.scrollView);
}];

[self.containerView addSubview:self.topView];
[self.topView mas_makeConstraints:^(MASConstraintMaker *make) {
    make.left.right.top.mas_equalTo(self.containerView).offset(0);
}];

[self.containerView addSubview:self.centerView];
[self.centerView mas_makeConstraints:^(MASConstraintMaker *make) {
    make.top.mas_equalTo(self.topView.mas_bottom).offset(0);
    make.left.right.mas_equalTo(self.containerView).offset(0);
}];

[self.containerView addSubview:self.bottomView];
[self.bottomView mas_makeConstraints:^(MASConstraintMaker *make) {
    make.top.mas_equalTo(self.centerView.mas_bottom).offset(0);
    make.left.right.bottom.mas_equalTo(self.containerView).offset(0);
}];

// 这个很重要,controller里面同样需要确定containerView(容器)的底部约束
[self.containerView mas_makeConstraints:^(MASConstraintMaker *make) {
   make.bottom.mas_equalTo(self.bottomView.mas_bottom).offset(0);
}];

```

## 最后
- 布局页面我们已经全部都完成了,就是数据填充问题了,这样我们实行UIScrollView复杂页面开发的实现

**重要的事情说三遍**

1. 自定义`View`的子控件,约束必须是从`top`到`bottom`位置必须确定,接着确定自定义`View`的底部约束(就是自定义`View`底部控件的`bottom`的约束)

2. 自定义`View`的子控件,约束必须是从`top`到`bottom`位置必须确定,接着确定自定义`View`的底部约束(就是自定义`View`底部控件的`bottom`的约束)

3. 自定义`View`的子控件,约束必须是从`top`到`bottom`位置必须确定,接着确定自定义`View`的底部约束(就是自定义`View`底部控件的`bottom`的约束)

## 修改`View`约束动画
```
- (void)updateConstraints {
    // remake会将之前的全部移除，然后重新添加
    __weak __typeof(self) weakSelf = self;
    [self.testButton mas_remakeConstraints:^(MASConstraintMaker *make) {
        make.top.mas_equalTo(self.testLabel.mas_bottom).offset(0);
        make.left.right.mas_equalTo(self).offset(0);
        if (weakSelf.isExpanded) {
            make.height.mas_equalTo(200);
        } else {
            make.height.mas_equalTo(40);
        }
    }];
    
    [super updateConstraints];
}

// 响应时间处理
- (void)onGrowButtonTaped:(UIButton *)sender {
    self.isExpanded = !self.isExpanded;
    if (!self.isExpanded) {
        [self.testButton setTitle:@"点我展开" forState:UIControlStateNormal];
    } else {
        [self.testButton setTitle:@"点我收起" forState:UIControlStateNormal];
    }

    // 告诉self.view约束需要更新
    [self setNeedsUpdateConstraints];
    // 调用此方法告诉self.view检测是否需要更新约束，若需要则更新，下面添加动画效果才起作用
    [self updateConstraintsIfNeeded];

    [UIView animateWithDuration:0.3 animations:^{
        [self layoutIfNeeded];
    }];
}
```