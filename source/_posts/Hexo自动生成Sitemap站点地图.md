---
title: Hexo自动生成Sitemap站点地图
author: 圣奇宝枣
description: 通过简单的操作，实现Hexo自动生成站点地图，方便搜索引擎SEO抓取
sticky: 0
date: 2022-06-16 22:01:14
updated: 2022-06-17 15:07:30
readmore: true
tags:
  - Hexo博客
  - Sitemap地图
  - SEO收录
  - 教程分享
categories:
  - 其他教程
---

---

#### **站点地图(Sitemap)**

- 站点地图**描述了一个网站的架构**。 它可以是一个**任意形式的文档**，用作网页设计的设计工具，也可以是列出网站中所有页面的一个网页，通常采用分级形式。这**有助于访问者以及搜索引擎的爬虫找到网站中的页面**。

- 站点地图**为 SEO 带来的好处**：

<!-- more -->

> 1、为搜索引擎爬虫提供可以浏览整个网站的链接  
> 2、为搜索引擎爬虫提供一些链接，指向动态页面或者采用其他方法比较难以到达的页面  
> 3、如果访问者试图访问网站所在域内并不存在的 URL，那么这个访问者就会被转到“无法找到文件”的错误页面，而网站地图可以作为该页面的“准”内容  
> 4、说白了就是让搜索引擎的爬虫，尽可能多的收录你站点上的页面，页面收录的越多，你的网站的流量就会越大

---

#### **Hexo 自动生成站点地图**

- 安装命令：

  ```bash
  谷歌版本：
  npm install hexo-generator-sitemap --save

  百度版本(注:个人使用经常出错，亲测谷歌版也能胜任百度，不建议安装)：
  npm install hexo-generator-baidu-sitemap --save
  ```

- 安装结束后，在**配置文件\_config.yml**中找到**url**，改成你**自己的域名**

- 额外配置项(如果没效果或需要自己配置文件路径，在**配置文件**中添加)：

  ```yaml
  Plugins:
    - hexo-generator-baidu-sitemap
    - hexo-generator-sitemap

  baidusitemap:
    path: baidusitemap.xml
  sitemap:
    path: sitemap.xml
  ```

- 此后每一次执行`hexo g`都会在**public**下自动生成站点地图.xml 文件，提交 SEO 时使用`域名/sitemap.xml`或`域名/baidusitemap.xml`提交即可

---

#### **页底评论**

---
