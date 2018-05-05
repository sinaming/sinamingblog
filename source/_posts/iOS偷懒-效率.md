---
title: iOS偷懒_效率
date: 2018-05-04 11:05:36
tags: "iOS"
categories: "iOS效率"
---

## `import`导入`pod`第三方库不提示问题
1. 选择`target` > `BuildSettings` > `search Paths`下的`User Header Search Paths`

2. 双击后面的空白区域

3. 点击"+"号添加一项:并且输入:`$(PODS_ROOT)`,选择:`recursive`(会在相应的目录递归搜索文件)

<!--more-->

## 添加`pch`文件

1. `Xcode`正确创建`pch`文件

2. 选择`target` > `BuildSettings` > `Apple LLVM 8.0 -Language`下的`Prefix Header`(或者搜索`Prefix Header`)

3. 双击后面的空白区域

4. 点击"+"号添加一项:并且输入:`$(SRCROOT)/项目中创建.pch`,选择:`recursive`(会在相应的目录递归搜索文件)

5. `Precompile Prefix Header`为`YES`,预编译后的pch文件缓存起来

## 扩展随机颜色
```
[UIColor colorWithRed:arc4random_uniform(255) / 255.0 green:arc4random_uniform(255) / 255.0 blue:arc4random_uniform(255) / 255.0 alpha:1];
```

## UIView常用setNeedsDisplay和setNeedsLayout

**UIView的setNeedsDisplay和setNeedsLayout方法**

>首先两个方法都是异步执行的。而`setNeedsDisplay`会调用自动调用`drawRect`方法，这样可以拿到`UIGraphicsGetCurrentContext`，就可以画画了。而`setNeedsLayout`会默认调用`layoutSubViews`，就可以处理子视图中的一些数据。

>综上所诉，`setNeedsDisplay`方便绘图，而`layoutSubViews`方便出来数据。

`layoutSubviews`在以下情况下会被调用：

- `init`初始化不会触发`layoutSubviews`。
- `addSubview`会触发`layoutSubviews`。
- 设置`view`的`Frame`会触发`layoutSubviews`，当然前提是`frame`的值设置前后发生了变化。
- 滚动一个`UIScrollView`会触发`layoutSubviews`。
- 旋转`Screen`会触发父`UIView`上的`layoutSubviews`事件。
- 改变一个`UIView`大小的时候也会触发父`UIView`上的`layoutSubviews`事件。
- 直接调用`setLayoutSubviews`。


`drawRect`在以下情况下会被调用：

- 如果在`UIView`初始化时没有设置`rect`大小，将直接导致`drawRect`不被自动调用。`drawRect`调用是在`Controller->loadView, Controller->viewDidLoad `两方法之后掉用的.所以不用担心在控制器中,这些`View`的`drawRect`就开始画了.这样可以在控制器中设置一些值给`View`(如果这些`View draw`的时候需要用到某些变量值).
- 该方法在调用`sizeToFit`后被调用，所以可以先调用`sizeToFit`计算出`size`。然后系统自动调用`drawRect:`方法。
- 通过设置`contentMode`属性值为`UIViewContentModeRedraw`。那么将在每次设置或更改`frame`的时候自动调用`drawRect:`。
- 直接调用`setNeedsDisplay`，或者`setNeedsDisplayInRect:`触发`drawRect:`，但是有个前提条件是`rect`不能为0。
- 以上1,2推荐；而3,4不提倡


`drawRect`方法使用注意点：

- 若使用`UIView`绘图，只能在`drawRect：`方法中获取相应的`contextRef`并绘图。如果在其他方法中获取将获取到一个`invalidate`的`ref`并且不能用于画图。`drawRect：`方法不能手动显示调用，必须通过调用`setNeedsDisplay` 或者 `setNeedsDisplayInRect`，让系统自动调该方法。
- 若使用`calayer`绘图，只能在`drawInContext:`中（类似于`drawRect`）绘制，或者在`delegate`中的相应方法绘制。同样也是调用`setNeedDisplay`等间接调用以上方法
- 若要实时画图，不能使用`gestureRecognizer`，只能使用`touchbegan`等方法来掉用`setNeedsDisplay`实时刷新屏幕


## UIView调用

>`-(void)layoutSubviews`

>`-(void)layoutIfNeeded`

>`-(void)setNeedsLayout`

>`-(CGSize)sizeThatFits:(CGSize)size`

>`-(void)sizeToFit`

>`-(void)setNeedsDisplay`

>`-(void)drawRect`

### `layoutSubviews`在以下情况下会被调用/被触发？？

1. `init`初始化不会触发`layoutSubviews`，但是是用`initWithFrame`进行初始化时，当`rect`的值 非`CGRectZero`时,也会触发。

2. `addSubview`会触发`layoutSubviews`

3. 设置`view`的`Frame`会触发`layoutSubviews`，当然前提是`frame`的值设置前后发生了变化

4. 滚动一个`UIScrollView`会触发`layoutSubviews`

5. 旋转`Screen`会触发父`UIView`上的`layoutSubviews`事件

6. 改变一个`UIView`大小的时候也会触发父`UIView`上的`layoutSubviews`事件


**(在苹果的官方文档中强调)**

`You should override this method only if the autoresizing behaviors of the subviews do not offer the behavior you want.layoutSubviews`,(当我们在某个类的内部调整子视图位置时，需要调用。反过来的意思就是说：如果你想要在外部设置`subviews`的位置，就不要重写。)

## 刷新子对象布局??

### 什么时候，需要重写？


>view是系统的，不需要重写 `- (void)layoutSubviews`

>view是自定义的，需要重写  `- (void)layoutSubviews`

>`-layoutSubviews`方法：这个方法，默认没有做任何事情，需要子类进行重写，自定义`view`时，手动重写，这里面只能写`subview`的`frame`限制。

### 手动调用这个方法，系统默认 我们不能手动直接调用这个方法，这能通过下列两种方式，调用/触发 `- (void)layoutSubviews`方法


>`-setNeedsLayout`方法： 标记为需要重新布局，告诉系统未来某个时间点异步调用。不立即刷新，但`layoutSubviews`一定会被调用。

>`-layoutIfNeeded`方法：如果，有需要刷新的标记，立即调用`layoutSubviews`进行布局（如果没有标记，不会调用`layoutSubviews`）

>若需要立即刷新`view`的`frame`更改：（同时调用，注意先后顺序）

>先调用`[view setNeedsLayout]`，把标记设为需要布局

>然后马上调用`[view layoutIfNeeded]`，实现布局

>在初始化方法`init`..。、或者view第一次显示之前，标记总是“需要刷新”的，可以直接调用`[view layoutIfNeeded]`

### 重绘


>`-drawRect:(CGRect)rect`方法：重写此方法，执行重绘任务

>`-setNeedsDisplay`方法：标记为需要重绘，异步调用`drawRect`

>`-setNeedsDisplayInRect:(CGRect)invalidRect`方法：标记为需要局部重绘

- **（注意：`sizeToFit`会 自动调用`sizeThatFits`方法；**

`sizeToFit`不应该在子类中被重写，应该重写`sizeThatFits`）

>`sizeThatFits`传入的参数是`receiver`当前的`size`，返回一个适合的`size`

>`sizeToFit`可以被手动直接调用,注意(系统默认的一些控件可以通过调用`sizeToFit`方法使其有尺寸,`egUIBarButtonItem,UITableView`的组头,组尾,表头,表尾,,,......)

>`sizeToFit`和`sizeThatFits`方法都没有递归，对`subviews`也不负责，只负责自己

>`layoutSubviews`对`subviews`重新布局

>`layoutSubviews`方法调用先于`drawRect`

>`setNeedsLayout`在`receiver`标上一个需要被重新布局的标记，在系统`runloop`的下一个周期自动调用`layoutSubviews`

>`layoutIfNeeded`方法如其名，`UIKit`会判断该`receiver`是否需要`layout`.根据Apple官方文档,`layoutIfNeeded`方法应该是这样的

>`layoutIfNeeded`遍历的不是`superview`链，应该是`subviews`链

>`drawRect`是对`receiver`的重绘，能获得`context`

>`setNeedDisplay`在`receiver`标上一个需要被重新绘图的标记，在下一个`draw`周期自动重绘，`iphone device的刷新频率是60hz`，也就是`1/60`秒后重绘


## 参考文档
[UIView常用的一些方法小记之setNeedsDisplay和setNeedsLayout](http://blog.sina.com.cn/s/blog_a573f7990101cdpe.html)