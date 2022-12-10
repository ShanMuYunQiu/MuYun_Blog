---
title: 使用Visual Studio开发QT
author: 圣奇宝枣
description: 配置Visual Studio，使用Visual Studio开发QT界面
sticky: 0
date: 2022-12-10 20:10:11
updated: 2022-12-10 20:10:11
readmore: true
tags:
  - 前置准备
  - Visual Studio
  - QT
  - 环境配置
categories:
  - 编辑器与IDE
---

---

#### **下载**

---

[QT 中文官网](https://www.qt.io/zh-cn/)

[Visual Studio 官网](https://visualstudio.microsoft.com/zh-hans/)

---

#### **安装**

---

- 安装 Visual Studio，比较简单，在此不再赘述，注意需要 **C++的开发套件**

- 安装 QT，记住**QT 安装的路径**，选择**组件**时，在**QT**一栏选择需要的**QT 版本**与**对应所需的组件**

<!-- more -->

- VS 中安装**QT Visual Studio Tools**拓展插件并重启

---

#### **配置**

---

- **控制面板**-**程序**-**启用或关闭 Windows 功能**，启用`.NET 4.7`(或更高版本)

- 打开 VS，在**Qt VS Tools**选项中选择**Qt Options**，在**Qt Versions**中添加 QT 路径，路径为**安装路径下**-**Qt x.xx.x**-**MSVC**的路径

- 此时 VS 创建工程已经可以创建 QT 的项目，选择**QT GUI**创建图形程序，出现**QT 创建引导**，选择**对应需要的模块**

- **自动创建文件的说明**

  > 1、创建后，`.ui`文件为**QT 界面文件**，双击可以使用**QT Designer**打开  
  > 2、`.h`头文件为 QT 自动生成的**继承 QMainWindow 的以工程名命名的类**  
  > 3、`.qrc`为**QT 的资源文件**，双击可以打开，后续可以添加需要的资源，如图片等  
  > 4、`main.cpp`中包含**刚才的头文件**和**QtWidgets 界面类**

- 此时可以**调试运行**查看目前结果

---

#### **页底评论**

---
