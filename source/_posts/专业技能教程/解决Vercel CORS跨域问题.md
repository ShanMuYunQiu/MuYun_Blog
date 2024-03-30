---
title: 解决Vercel CORS跨域问题
author: 圣奇宝枣
description: 在实现功能时常常会遇到跨域请求，而如果使用Vercel部署，则可以按此教程操作
sticky: 0
readmore: true
tags:
  - Vercel
categories:
  - 专业技能教程
abbrlink: f6c193b6
date: 2022-07-19 00:00:00
updated: 2022-07-19 00:00:00
---

---

### **npm 安装**

---

- **终端输入以下命令**

  ```bash
  npm i -D http-proxy-middleware
  ```

---

#### **根目录创建以下文件**

---

<!-- more -->

- `api/proxy.js`，注意文件目录也要一致(名为 api 的文件夹)，注意根据注释修改部分配置：

  ```js
  // api/proxy.js
  // 该服务为 vercel serve跨域处理
  const { createProxyMiddleware } = require('http-proxy-middleware')

  module.exports = (req, res) => {
      let target = ''
      // 代理目标地址
      // 这里使用 backend 主要用于区分 vercel serverless 的 api 路径
      // target 替换为你跨域请求的服务器 如： http://baidu.com
      if (req.url.startsWith('/backend')) {
          target = 'https://fanyi-api.baidu.com'
      }
      // 创建代理对象并转发请求
      createProxyMiddleware({
          target,
          changeOrigin: true,
          pathRewrite: {
              // 通过路径重写，去除请求路径中的 `/backend`
              // 例如 /backend/user/login 将被转发到 https://fanyi-api.baidu.com/user/login
              '^/backend/': '/',
          },
      })(req, res)
  }
  ```

- `Vercel.json`：

  ```json
  {
    "rewrites": [
      {
        "source": "/backend/(.*)", // 准备匹配的接口
        "destination": "/api/proxy" // 配置路径
      }
    ]
  }
  ```

- 根据情况，http 接口请求代码前缀记得换成`/backend/`，代码提交，默认自动部署到vercel了

---

#### **代码提交后 Vercel 操作**

---

> 1、打开项目，点击 **Production**  
> 2、在**部署记录**里的最新部署，找到右侧的**三个竖点**点击  
> 3、点击**Promote to Production**(推广到生产)去处理跨域  
> 4、返回项目，在**Functions**中选择`api/proxy.js`文件

---

#### **页底评论**

---
