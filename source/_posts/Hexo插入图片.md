---
title: Hexo插入图片
date: 2018-05-05 10:10:12
tags: "Hexo"
categories: "Hexo教程"
---

## 前言

>当我们想在博客上上传图片,如果直接引用本地图片,会存在很多问题,图片损坏等


## 解决

- 在`hexo`文件夹中找到`_config.yml`里的`post_asset_folder`:这个选项设置为`true`

- 在`hexo`目录下执行这样一句话`npm install hexo-asset-image --save`命令,来安装一个可以上传图片的插件

<!--more-->

- 命令创建`hexo -n xxxx.md`,执行完成之后,会在`/source/_posts`文件夹内除了`xxxx.md`文件还有一个同名的文件夹

- 最后把图片复杂到`xxxx.md`的文件夹中,按照这样的方式`![你想输入的替代文字](xxxx/图片名.jpg)`

## 举例
![phone](Hexo插入图片/phone.jpg)

>如果完成上面操作,会实现显示自己添加的图片,说明已经完成


## 参考文档
[hexo生成博文插入图片](https://blog.csdn.net/sugar_rainbow/article/details/57415705)