---
title: Hexo教程
date: 2018-05-03 16:28:19
tags: "Hexo"
categories: "Hexo教程"
---

## 配置Node.js环境

**Node.js: [Node.js官网](https://nodejs.org/en/#download)**

![图片](http://upload-images.jianshu.io/upload_images/727768-fb3a458f1555ea46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!--more-->

**下载成功之后是这样的一个文件:**

![图片](http://upload-images.jianshu.io/upload_images/727768-de9f36c841717341.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 安装 Node.js 和npm

![图片](http://upload-images.jianshu.io/upload_images/727768-2cf4a54b45c7bf90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**终端下测试下Node.js是否可以使用:**

```
node -v
```

**如果Node.js 成功安装，可以看到类似如下的信息:**

```
$ node -v
v10.0.0
```

**终端下测试下npm是否可以使用:**

```
npm -v
```

**如果npm成功安装，可以看到类似如下的信息:**

```
$ npm -v
5.6.0
```

>全部完成上面的配置,已经完成第一步

## 搭建GitHub博客

新建一个名为`用户名.github.io`的仓库,`用户名`其实就是你自己`GitHub用户名`

## 安装Hexo

```
终端中执行 npm install hexo-cli -g
```

```
mkdir hexo文件夹
```

```
hexo init
```

```
hexo generate hexo server hexo g | hexo s
```

```
登录本地localhost:4000
```

![图片](http://onq81n53u.bkt.clouddn.com/YY%E5%9B%BE%E7%89%8720180110175140.jpg)

## 配置Hexo主题
```
deploy:
  type: git
  repository:
            github: https://github.com/sinaming/sinaming.github.io.git
  branch: master
```
