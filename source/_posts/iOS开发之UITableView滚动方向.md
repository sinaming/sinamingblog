---
title: iOS开发之UITableView滚动方向
date: 2018-05-29 16:45:41
tags: "iOS"
categories: "iOS效率"
---

>iOS开发之`UITableView`, `UICollectionView`, `UIScrollview`,根据代理判断页面滚动方向

```
- (void)scrollViewDidScroll:(UIScrollView *)scrollView {  
   CGPoint point =  [scrollView.panGestureRecognizer translationInView:self.view];  
    if (point.y > 0 ) {  
        NSLog(@"------往上滚动");  
    }else{  
        NSLog(@"------往下滚动");  
  
    }  
} 
```

<!--more-->