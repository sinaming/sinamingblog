---
title: hexo博客迁移
date: 2018-08-06 16:18:40
tags: "Hexo"
categories: "Hexo教程"
---

### 问题

> 如果换了电脑怎么正确处理博客的方法是博客进行备份

- 重新按钮Hexo
> `sudo npm install -g hexo`

- 初始化Hexo
> `hexo init`

<!--more-->

- 安装依赖包
> `npm install`

- 安装上传插件
> `npm install hexo-deployer-git --save`

- RSS插件
> `npm install hexo-generator-feed`

- 字数统计 阅读时长 插件
`npm i --save hexo-wordcount`

- 搜索插件
`npm install hexo-generator-searchdb --save`

### 博客数据备份

>`scaffolds`		文章模版                    
`source`              博客文章                           
`themes`            主题                              
`.gitignore`         限定在push时那些文件可以忽略         
`_config.yml`       站点配置文件                       
`package.json`


### 主题

> 重新按钮的hexo主题会被覆盖掉,所有需要自己配置