---
title: iOS模型嵌套模型
date: 2018-06-23 09:57:14
tags: "iOS"
categories: "iOS开发"
---

## 问题

> 网络请求到服务器数据,根据服务器的数据结构,我们正确使用数据模型`model`接收,但会出现这样的情况,数组嵌套数组获取嵌套字典等,所以我们就有了模型切图模型的解决方案

## 代码`STCountryModel`h文件

> `STCountryModel`接收类

```
#import "STBaseModel.h"

@class STCountryListModel;

@interface STCountryModel : STBaseModel

@property (nonatomic, copy) NSString *initial;
// 使用强引用在接收是必须使用copy方法,防止出现问题
@property (nonatomic, strong) NSArray <STCountryListModel *>*list;

@end
```

<!--more-->

## 代码`STCountryModel`m文件

```
#import "STCountryModel.h"
#import "STCountryListModel.h"

@implementation STCountryModel

// 重新父类模型嵌套
- (instancetype)initWithSTDataModelDict:(NSDictionary *)dict {
    if (self = [super init]) {
        [self setValuesForKeysWithDictionary:dict];
        // 性能提升
        NSMutableArray *dataSource = [NSMutableArray arrayWithCapacity:self.list.count];
        for (NSDictionary *brandDict in self.list) {
            STCountryListModel *model = [[STCountryListModel alloc] initWithSTDataModelDict:brandDict];
            [dataSource addObject:model];
        }
        self.list = [dataSource copy];
    }
    return self;
}

@end
```

## 代码`STCountryListModel`h文件

> `STCountryListModel`接收类

```
#import "STBaseModel.h"

@interface STCountryListModel : STBaseModel

@property (nonatomic, copy) NSString *initial;

@property (nonatomic, copy) NSString *country;

@property (nonatomic, copy) NSString *file_fid;

@property (nonatomic, copy) NSString *ID;

@property (nonatomic, copy) NSString *country_code;

@property (nonatomic, copy) NSString *short_name;

@property (nonatomic, copy) NSString *file_pic;

@property (nonatomic, copy) NSString *name;

@property (nonatomic, copy) NSString *city_code;

@end
```

## 代码`STCountryListModel`m文件

```
#import "STCountryListModel.h"

@implementation STCountryListModel

// 关键字冲突
- (void)setValue:(id)value forUndefinedKey:(NSString *)key {
    if([key isEqualToString:@"id"]) {
    	// [self.ID setValue:value forKey:key];
        self.ID = value;
        return ;
    }
}

@end
```