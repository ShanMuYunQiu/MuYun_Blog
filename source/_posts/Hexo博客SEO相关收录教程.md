---
title: Hexo博客SEO相关收录教程
author: 圣奇宝枣
description: 所有有关于网站收录SEO的注意事项，以及网站SEO相关的收录教程。
sticky: 0
date: 2022-06-16 22:01:14
updated: 2022-07-03 17:27:13
readmore: true
tags:
  - Hexo博客
  - Sitemap地图
  - SEO收录
categories:
  - 其他教程
---

---

#### **SEO 简介**

---

- **SEO**，译为**搜索引擎优化**。是一种利用**搜索引擎**的规则提高网站在有关搜索引擎内的**自然排名**的行为。很大程度上是网站经营者的一种商业行为，将自己或自己公司网站的排名前移。个人博客应该比较少

- 现如今，个人博客式微。我想大部分搭建博客的个人博主，都有分享欲。应该希望写的文章让有需要的人看到。但是以 **Hexo** 为框架搭建的**静态博客**，做 SEO 不像**动态网站**那么容易。你不**主动**向搜索引擎提交收录，搜索引擎不太可能搜到你写的文章，尤其是百度

- 下面讲一讲静态网站如何向搜索引擎提交收录和进行 SEO 优化

<!-- more -->

---

#### **搜索引擎收录**

---

<div class="info">

> 个人博主通常使用**两种域名**： `xxx.xxx.com` 和 `xxx.com` 。关于这两种域名，到底哪种域名的 SEO 更好，众说纷纭。可以看看[此链接](https://www.zhihu.com/question/20414602/answer/2084139715)

</div>

##### **百度收录**

> 基于 GitHub Pages 的个人博客， Google 收录非常及时全面。然而，到目前为止，GitHub 还是拒绝百度爬虫的访问，直接返回 403。官方给出原因：百度爬虫爬得太狠，影响了 Github Pages 服务的正常使用。这就导致，在 Github Pages 上搭建的个人博客，都无法被百度收录。也就是 `xxxx.github.io` 的域名，百度不会收录  
> 当然，这是可以解决的 —— 建立 Gitee、Coding 镜像站。但是新版 Coding 被腾讯云收购，静态页面的部署依赖于腾讯云的对象存储功能，对于新用户会提供 6 个月的免费流量包，之后开始收费；而 gitee 开通 gitee pages 需要手持身份证拍摄照片。如果可以接受，就可以自行尝试此方法

- 进入[百度站长工具](https://ziyuan.baidu.com/site/index#/)网站。按照网站提示，添加网站。验证网站可选**文件验证**、**HTML 标签验证**、**CNAME 验证**

- **文件验证**：

  - 下载**验证文件**，放入 **Test_Blog\source** 路径。打开该文件，添加以下代码

  ```yaml
  ---
  layout: false
  ---
  ```

- **HTML 标签验证**

  - 复制 HTML 标签验证，进入 **Test_Blog\themes\fluid\layout\\\_partial** 路径，打开 **head.ejs** 文件，添加复制的 HTML 标签验证。

  ```js
  <!-- 添加位置 -->
  <meta name="baidu-site-verification" content="code-???????" />
  <title><%= title %></title>
  ```

- **CNAME 验证**

  - 按照说明**手动添加域名解析**，简单快捷但需要有域名

##### **必应收录**

- 进入[必应站长工具](https://www.bing.com/webmasters/about)网站，点击开始，进入下一环节。中间还需要登录账号

- 右侧输入网址。按照网站提示，验证网站。可选**文件验证**、**HTML 标签验证**、**CNAME 验证**。验证方式**与百度收录差不多**，推荐使用**文件验证**，不需要添加代码

##### **谷歌收录**

- 进入[谷歌站长工具](https://www.google.com/webmasters/tools/dashboard)网站。前提：科学上网+谷歌邮箱账号

- 同上**按照引导添加网站**

---

#### **提交网站资源**

---

<div class="info">

> 搜索引擎会逐步收录我们的博客，但这个时间是不确定的。所以我们需要**主动提交**网站资源到各大搜索引擎  
> |提交方式|解释|  
> |:---:|---|  
> |**主动推送**|最为快速的提交方式，使用插件完成。方便快捷|  
> |**自动推送**|最为便捷的提交方式，将自动推送的 JS 代码放在站点的每一个页面源代码中，页面被浏览时，链接会被自动推送给百度。可以与主动推送配合使用|  
> |**Sitemap**|通过插件生成 sitemap，并将其提交给搜索引擎。搜索引擎会周期性地抓取、检查提交的 sitemap，对其中的链接进行处理。收录速度慢于主动推送|  
> |**手动提交**|手动提交链接给搜索引擎。较麻烦|

</div>

##### **站点地图 Sitemap**

- **什么是站点地图**

  - 站点地图是一个包含网站**所有资源链接**的网页。很多网站的连接层次比较深，爬虫很难抓取到，站点地图可以**方便爬虫抓取网站**页面。
  - 网站地图一般存放在**根目录**下并命名 **sitemap**，格式一般为 **TXT** 或者 **XML**。主要使用后者，即`sitemap.xml`

- **插件介绍**

- Hexo 生成站点地图的插件主要有三个，简介如下

> 1、[hexojs/hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap)：官方插件，一直在维护。生成的 XML 文件没有分类，可指定页面不编入 sitemap.xml 文件  
> 2、[ludoviclefevre/hexo-generator-seo-friendly-sitemap](https://github.com/ludoviclefevre/hexo-generator-seo-friendly-sitemap)：已停止维护，但仍可以使用。可生成 page、post、tags、category 对应的 XML 文件。可指定页面不编入 sitemap.xml 文件。这个插件可以生成适合搜索引擎优化的站点地图  
> 3、[coneycode/hexo-generator-baidu-sitemap](https://github.com/coneycode/hexo-generator-baidu-sitemap)：已停止维护，但仍可以使用。可生成利于百度爬虫抓取的 sitemap.xml 文件。与插件二的 post-sitemap.xml 基本一致

- **插件配置**

- **下载**

  ```bash
  # 不可以三个同时使用，会报错!!!且官方和友好可能不兼容!!!

  # 官方插件
  npm install hexo-generator-sitemap --save
  # 友好地图
  npm install hexo-generator-seo-friendly-sitemap --save
  # 百度地图
  npm install hexo-generator-baidu-sitemap --save
  ```

- `_config.yml`**配置**

  ```yaml
  # 官方插件
  sitemap:
  path:
  - sitemap.xml
  - sitemap.txt
  template:
  template_txt:
  rel: false
  tags: false
  categories: false
  # 友好地图
  sitemap:
  path: sitemap.xml
  tag: false # true：生成，false：排除
  category: false # true：生成，false：排除
  # 百度地图
  baidusitemap:
  path: baidusitemap.xml
  ```

- **提交 Sitemap**

![](https://s2.loli.net/2022/07/03/GkcXwCtO3iVeBr5.jpg)

- 推荐提交以下站点地图

  ```
  https://...../sitemap.xml
  https://...../post-sitemap.xml
  https://...../page-sitemap.xml
  https://...../baidusitemap.xml
  ```

##### **主动推送**

- **下载插件**

  - [cjh0613/hexo-submit-urls-to-search-engine](https://github.com/cjh0613/hexo-submit-urls-to-search-engine)插件

  ```bash
  npm install --save hexo-submit-urls-to-search-engine --save
  ```

- **获取秘钥**

  - **百度**

    - 站长工具 -> 选择域名，点击进入 -> 点击侧边栏普通收录 -> API 提交，下方**推送接口token**为 Baidu Key
  
  - **必应**

    - 站长工具 -> 右上角齿轮图标 -> 选择接口访问（**API access**） -> 查看接口密钥 Bing Key

  - **谷歌**

    - 较为复杂，请自行查询，且需要配置git bash代理，否则会推送失败

- `_config.yml`**配置**

  ```yaml
  hexo_submit_urls_to_search_engine:
    submit_condition: count # 链接被提交的条件，可选值：count / period，现仅支持 count。
    count: 10 # 提交最新的 10 个链接。
    period: 900 # 提交修改时间在 900 秒内的链接。
    google: 1 # 是否向 Google 提交，可选值：1 / 0（0：否；1：是）
    bing: 1 # 是否向 Bing 提交，可选值：1 / 0（0：否；1：是）
    baidu: 1 # 是否向 Baidu 提交，可选值：1 / 0（0：否；1：是）
    txt_path: submit_urls.txt # 文本文档名，需要推送的链接会保存在此文本文档里。
    baidu_host: https://jinghu-moon.top # 在百度站长平台注册的域名。
    baidu_token: iuqwwvDFbYfNJtw8 # 百度站长秘钥，不要把它直接发布在公众仓库里!
    bing_host: https://jinghu-moon.top # 在必应站长平台注册的域名。
    bing_token: 89c791499bbb4786bb47026153357b3f # 必应站长秘钥，不要把它直接发布在公众仓库里!
    google_host: https://jinghu-moon.top ## 在谷歌站长平台注册的域名。
    google_key_file: central-droplet-337107-098e588b1d98.json #存放google key 的 json 文件，与hexo _config.yml文件放于同一路径。不要把json 文件直接发布在公众仓库里!
    google_proxy: http://127.0.0.1:7890 # 向谷歌提交网址所使用的系统http 代理，填 0 不使用。以 Clash for Windows 为例。必须使用代理，否则，本地谷歌推送会失败
    replace: 0 # 是否替换链接中的部分字符串，可选值：1 | 0（0：否；1：是）
    find_what:
    replace_with:

  deploy:
    - type: cjh_google_url_submitter
    - type: cjh_bing_url_submitter
    - type: cjh_baidu_url_submitter
  ```

##### **其他两种**

- **自动推送**：百度自动推送已失效

- **手动推送**：自行前往站长工具进行提交

---

#### **页底评论**

---
