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

- **上面的例子**实际上就是**内部构建**，其产生的**临时文件特别多**，**不方便清理**

- **外部构建**，就会把生成的**临时文件**放在**build 文件夹**，强烈推荐使用**外部构建**

- **使用外部构建**

  > 1、建立一个**build 目录**，可以在**任何地方**，推荐在**项目当前目录下**  
  > 2、进入**build 目录**，运行`cmake ..`(`..`表示上级目录，仅适用于 build 在当前目录下的情况)，当然也可以写`CMakeLists.txt`所在的**绝对路径**  
  > 3、现在，生成的文件都在**build 目录**下了，在**build 目录**中使用`make`来构建工程

- **注意外部构建的两个变量**

  > 1、`HELLO_BINARY_DIR`：是**编译路经**，即编译时**build 文件夹的路径**  
  > 2、`HELLO_SOURCE_DIR`：是**工程路径**，仍然是之前的**工程路径**

---

#### **构建一个工程的规则与操作**

---

- 一个工程的**开发和管理**通常有以下规则：

  > 1、为工程添加一个**子目录 src**，用于存放**工程源码**  
  > 2、添加一个**子目录 doc**，用来放置这个**工程的文档**`hello.txt`  
  > 3、在工程目录**添加文本文件**：`COPYRIGHT`**版权文件**、`README`**用户说明**  
  > 4、在工程目录**添加**`runhello.sh`**脚本**，用来**调用 hello 二进制文件**  
  > 5、将**构建后的目标文件**放入**构建目录**的**bin 子目录**  
  > 6、将**doc 目录的内容**以及`COPYRIGHT`、`README`安装到`/usr/share/doc/cmake`(此为 linux 路径)

- **将目标文件放入构建目录的 bin 子目录**

  - 创建一个**src 目录**，将源码移入**src**。注意工程的**每个目录下**都有要`CMakeLists.txt`，所以**src**下也需要

  - **重写**`CMakelists.txt`

    ```
    目录树结构：

    .
    |---build
    |---CMakeLists.txt
    |---src
        |---CMakeLists.txt
        |---main.cpp
    ```

    ```cmake
    # 根目录CMakeLists.txt

    project(HELLO)

    add_subdirectory(src ./bin)
    ```

    ```cmake
    # src目录CMakelists.txt

    add_executable(hello main.cpp)
    ```

  - `add_subdirectory`**命令**

    > 1、**完整参数**：`add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL])`  
    > 2、该命令用于**向当前工程添加存放源文件的子目录**，并可以**指定中间二进制和目标二进制存放的位置**，写在**最外层的**`CMakeLists.txt`来**首先告知**  
    > 3、`EXCLUDE_FROM_ALL`函数是**将写的目录从编译中排除**，如程序中的 example  
    > 4、`add_subdirectory(src bin)`：将**src 目录加入工程**并指定**编译输出的路径为 bin 目录**。如果不指定 bin 目录，则所有编译结果都会存放在 src 目录中

  - **更改二进制的保存路径**

    > 1、可以使用`set`**命令**重新定义`EXECUTABlE_OUTPUT_PATH`和`LIBRARY_OUTPUT_PATH`变量，来指定最终的**目标二进制**的位置  
    > 2、`set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)`  
    > 3、`set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)`

---

#### **使用 CMake 来进行安装**

---

- **安装的命令**

  > 1、一种是从**代码编译后**直接`make install`安装  
  > 2、一种是**打包时**的**指定目录安装**。两种指定目录的方法，`make install DESTDIR=/tmp/test`或`./configure -prefix=/usr`

- **如何安装 hello world**

  > 1、使用 Cmake 一个**新的命令**：`install`，`install`的安装可以包括**二进制**、**动态库**、**静态库**以及**文件**、**目录**、**脚本**等  
  > 2、使用 Cmake 一个**新的变量**：`CMAKE_INSTALL_PREFIX`，该变量用于指定**安装的路径**

  ```
  目录树结构：

  .
  |---build
  |---CMakeLists.txt
  |---COPYRIGHT
  |---README
  |---doc
      |---hello.txt
  |---runhello.sh
  |---src
      |---CMakeLists.txt
      |---main.cpp
  ```

- **安装文件**`COPYRIGHT`和`README`

  > 1、在**根目录**`CMakeLists.txt`中添加：`install(FILES COPYRIGHT README DESTINATION share/doc/cmake)`  
  > 2、`FILES`：安装的是**文件**  
  > 3、`DESTINATION`：写**绝对路径**。也可以写**相对路径**，但相对路径的**实际路径**是`${CMAKE_INSTALL_PREFIX}/<DESTINATION定义的路径>`  
  > 4、`CMAKE_INSTALL_PREFIX`默认是在`/usr/local/`(此为 linux 路径，windows 默认`C:\Program Files (x86)\`)，在**cmake**时使用`cmake -DCMAKE_INSTALL_PREFIX=/usr`可以**指定该变量的路径**，也可以使用`set`命令更改变量地址

- **安装脚本**`runhello.sh`

  > 1、在**根目录**`CMakeLists.txt`中添加：`install(PROGRAMS runhello.sh DESTINATION bin)`  
  > 2、`PROGRAMS`：**非目标文件的可执行程序的安装**(比如脚本)

- **安装目录 doc**

  > 1、可以通过**在 doc 目录下**创建`CMakeLists.txt`，在其中**安装目录下的文件**
  > 2、也可以在**根目录**`CMakeLists.txt`中添加：`install(DIRECTORY doc/ DESTINATION share/doc/cmake)`  
  > 3、`DIRECTORY`：安装的是**目录**。后面连接的是**所在目录的相对路径**  
  > 4、注意，`doc`和`doc/`**差别很大**。目录**以**`/`**结尾**，将这个**目录中的内容**安装到指定目录；目录**不以**`/`**结尾**，则会将**这个目录**安装到指定目录

- **安装过程**

  ```bash
  cmake
  make
  make install
  ```

---

#### **Cmake 构建共享库**

---

- 码字中。。。

---

#### **页底评论**

---
