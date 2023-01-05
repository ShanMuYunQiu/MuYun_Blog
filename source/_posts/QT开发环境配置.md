---
title: QT 开发环境配置
author: 圣奇宝枣
description: QT环境配置，以及使用其他IDE或编辑器开发QT
sticky: 0
date: 2022-12-10 20:10:11
updated: 2022-12-10 20:10:11
readmore: true
tags:
  - 前置准备
  - 环境配置
  - QT
categories:
  - 编辑器与IDE
---

---

#### **下载**

---

[QT 中文官网](https://www.qt.io/zh-cn/)

---

#### **安装 QT**

---

- **QT 安装说明**

> 1、安装 QT，记住**QT 安装的路径**，选择**组件**时，在**QT**一栏选择需要的**QT 版本**
> 2、其中 **QT 版本**下选择的是对应的开发库，**MSVC**和**MinGW**两个编译器**使用哪个就选中哪个**，其余组件按需选择
> 3、此外最下方的**开发工具**，按需选择。其中**Cmake**和**Ninja**建议必选，如果使用**MinGW**还需要选择**对应的 MinGw 版本**

<!-- more -->

---

#### **使用 Visual Studio 开发 QT**

---

- **基本配置**

> 1、安装 Visual Studio，比较简单，在此不再赘述，注意需要选择 **C++的开发套件**  
> 2、VS 中安装**QT Visual Studio Tools**拓展插件并重启  
> 3、**控制面板**-**程序**-**启用或关闭 Windows 功能**，启用`.NET 4.8`(或更高版本)  
> 4、打开 VS，在**Qt VS Tools**选项中选择**Qt Options**，在**Qt Versions**中添加 QT 路径，路径为**安装路径下**-**Qt x.xx.x**-**MSVC**，找到其中**qmake.exe**的路径  
> 5、此时 VS 创建工程已经可以创建 QT 的项目，选择**QT Widgets Application**创建图形程序，出现**QT 创建引导**，选择**对应需要的模块**

- **自动创建的文件的说明**

  > 1、创建后，`.ui`文件为**QT 界面文件**，双击可以使用**QT Designer**打开  
  > 2、`.h`头文件为 QT 自动生成的**继承 QMainWindow 的以工程名命名的类**  
  > 3、`.qrc`为**QT 的资源文件**，双击可以打开，后续可以添加需要的资源，如图片等  
  > 4、`main.cpp`中包含**刚才的头文件**和**QtWidgets 界面类**

- 此时可以**调试运行**查看目前结果

---

#### **页底评论**

---
