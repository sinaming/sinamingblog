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

进入`GitHub`,我们新建一个名为`用户名.github.io`的仓库,`用户名`其实就是你自己`GitHub用户名`

## 安装Hexo

- 终端中执行`npm install hexo-cli -g`

- `桌面`或者`自己熟悉的地方`,通过`终端`创建mkdir`hexo`文件夹

- 进入`hexo`文件夹,执行`hexo init`

**完成之后执行下面方法:**
```
hexo generate	创建静态页面	
hexo server	启动服务
```

- 缩写`hexo g | hexo s`

- 登录本地`localhost:4000`

** 启动服务,会显示下面页面 **
![图片](http://onq81n53u.bkt.clouddn.com/YY%E5%9B%BE%E7%89%8720180110175140.jpg)


- 接下来,我们就是把创建好的`hexo`项目,配置上传`GitHub`的前提条件

- 找到`hexo`文件夹下面的`_config.yml`文件

## 配置Hexo主题

- 把这段代码放置`_config.yml`文件的最后面,其中`GitHub用户名`是你自己在`GitHub`中创建的仓库对于的`信息`

```
deploy:
  type: git
  repository:
            github: https://github.com/GitHub用户名/GitHub用户名.github.io.git
  branch: master
```

 >这样我们就完成了对`hexo`的基础搭建和配置

## 同步GitHub

- 终端执行`npm install hexo-deployer-git --save`

- 完成上面的全部操作,我们博客基本完成,相应的主题配置自行`百度`,`Google`

**开始同步**
```
hexo clean

hexo generate

hexo deployer
```

- 缩写`hexo d -g`执行

- 浏览器中输入`https://GitHub用户名.github.io`,如果成功会显示上面同样的页面

## 最后
- 如果你已经看到这里,说明已经完成了对博客的搭建,我们就可以开心的撸博客了!!!