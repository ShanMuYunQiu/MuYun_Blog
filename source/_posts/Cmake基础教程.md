---
title: Cmake基础教程
author: 圣奇宝枣
description: 学习有关Cmake构建的知识，学会使用Cmake进行编译
sticky: 0
date: 2023-01-07
updated: 2023-01-07
readmore: true
tags:
  - C语言
  - C++
  - Cmake
categories:
  - 专业技能教程
---

---

#### **Cmake 简介**

---

- **Cmake 是什么**

  > 1、**Cmake**是**高级编译配置工具**。当多个人要用**不同的语言**或者**编译器**开发一个项目，最终要输出一个**可执行文件**或**共享库**，这时候**Cmake 的作用**就凸显出来了  
  > 2、**Cmake**的**所有操作**都是通过**编译**`CMakeLists.txt`**所写内容**来完成的  
  > 3、学习**Cmake**的目的，是为了将来**处理大型 C/C++/Java 项目**做准备，其也是一种**生成 Makefile 的工具**

<!-- more -->

- **Cmake 的安装**

  - 之前的[VScode 配置 Clang 和 Cmake 环境](https://www.shengqibaozao.eu.org/2023/01/04/VScode%E9%85%8D%E7%BD%AEClang%E5%92%8CCmake%E7%8E%AF%E5%A2%83/)文章中写过 Cmake 的安装与环境变量配置，请参考此文章

---

#### **引入：Cmake 编译 hello_world 程序**

---

- 首先**项目文件夹**内中写一个名为`main.cpp`的**hello_world**源码

  ```cpp
  // main.cpp

  #include <iostream>
  using namespace std;
  int main()
  {
      cout << "hello world";
      return 0;
  }
  ```

- 使用**Cmake**编译，需要在**项目文件夹**创建`CMakeLists.txt`文件(注意区分大小写)

  ```cmake
  # CMakeLists.txt

  project(HELLO CXX)

  set(SRC_LIST test.cpp)

  message(STATUS "This is BINARY_DIR " ${PROJECT_BINARY_DIR})

  message(STATUS "This is SOURCE_DIR " ${PROJECT_SOURCE_DIR})

  add_executable(hello ${SRC_LIST})
  ```

- 在**当前目录**打开终端，输入`cmake .`，使用 Cmake 进行编译，如果没有报错，会**生成很多文件**，再使用`make`**编译生成可执行文件**

---

#### **Cmake 语法介绍**

---

- `project`**命令**

  - **说明**

    > 1、`project`可以用来**指定工程的名字**和**支持的语言**，默认**支持所有语言**  
    > 2、`project(HELLO)`：指定**工程名**为**HELLO**，并**支持所有语言**  
    > 3、`project(HELLO CXX)`：指定**工程名**为**HELLO**,并**指定语言为 C++**  
    > 4、`project(HELLO C CXX)`：指定**工程名**为**HELLO**,并**指定语言为 C 和 C++**

  - **两个隐式变量**

    > 1、该指定**隐式声明**了**两个 Cmake 变量**  
    > 2、`<projectname>_BINARY_DIR`，上例中为`HELLO_BINARY_DIR`  
    > 3、`<projectname>_SOURCE_DIR`，上例中为`HELLO_SOURCE_DIR`  
    > 4、当前该项目这**两个变量**都指向**当前工作目录**，后续会讲**外部编译**  
    > 5、如果**更改了工程名**，这两个**变量名也会更改**。为了**解决这个问题**，可以使用**Cmake 预定义**的**两个变量**：`PROJECT_BINARY_DIR`和`PROJECT_SOURCE_DIR`

- `set`**命令**

  > 1、`set`可以用来**显式指定变量**  
  > 2、`set(SRC_LIST main.cpp)`：指定**SRC_LIST**变量包含`main.cpp`  
  > 3、`set(SRC_LIST main.cpp t1.cpp t2.cpp)`：指定**SRC_LIST**变量包含后面的所有内容  
  > 4、`set(SRC_LIST "main.cpp")`：可以在**文件名外**加`""`，如果**源文件名包含空格**，则必须用`""`包括

- `message`**命令**

  > 1、`message`用于**向终端输出用户自定义的信息**，主要包含**三种信息**  
  > 2、**SEND_ERROR**：产生错误，生成过程被跳过  
  > 3、**STATUS**：输出前缀为`--`的信息  
  > 4、**FATAL_ERROR**：立即终止所有 Cmake 过程

- `ADD_EXECUTABLE`**命令**

  > 1、`ADD_EXECUTABLE`用于**生成可执行文件**  
  > 2、`ADD_EXECUTABLE(hello ${SRC_LIST})`：生成的**可执行文件名**为**hello**，**源文件**来自读取**SRC_LIST**变量中的内容

- **语法的基本原则**

  > 1、**变量**使用`${变量名}`方式**取值**，但在**IF 控制语句**中是**直接使用变量名**  
  > 2、**命令**的**参数**使用`()`括起，**参数之间**使用**空格或分号**隔开  
  > 3、**命令**不区分大小写，但**参数和变量**区分大小写

---

#### **内部构建与外部构建**

---

- 码字中。。。

---

#### **页底评论**

---
