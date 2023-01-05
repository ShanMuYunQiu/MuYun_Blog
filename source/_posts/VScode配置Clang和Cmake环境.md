---
title: VScode配置Clang和Cmake环境
author: 圣奇宝枣
description: Vscode进阶篇，学习.vscode配置文件和Clang+Cmake的配置与使用
sticky: 0
date: 2023-01-05 21:14:13
updated: 2023-01-05 21:14:13
readmore: true
tags:
  - 前置准备
  - VScode
  - 环境配置
categories:
  - 编辑器与IDE
---

---

#### **前置准备**

---

- **前言及准备**

  > 1、首先，请确保你已经根据[VScode 基本环境配置](https://www.shengqibaozao.eu.org/2022/04/27/VScode%E5%9F%BA%E6%9C%AC%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/?t=1672920021184)文章学会了配置环境，并成功配置了 C/C++环境  
  > 2、比起前文的方案使用**GCC 编译器**，本方案使用**Clang + Cmake**配置环境，因此需要在上方文章所给链接下载**LLVM/Clang**套件  
  > 3、同**MinGW-w64**配置方法类似，将**bin 目录**添加到**Path 环境变量**中，确保在**cmd**中输入`clang -v`输出**clang 版本号**，配置完成后进入下面的步骤

<!-- more -->

---

#### **VScode 预定义替换变量**

---

- 接下来的**配置文件**中可能经常出现以下变量，其会替换为**对应含义的路径**

- **预定义替换变量**(其他见[官网文档](https://code.visualstudio.com/docs/editor/variables-reference))

  > 一个文件 `/home/your-username/your-project/folder/file.ext` 在你的编辑器中打开  
  > 一个目录 `/home/your-username/your-project` 作为你的根目录

  | 预定义替换变量             | 含义                                             | 路径                                             |
  | -------------------------- | ------------------------------------------------ | ------------------------------------------------ |
  | ${workspaceFolder}         | 当前工作目录(根目录)                             | /home/your-username/your-project                 |
  | ${workspaceFolderBasename} | 当前文件的父目录                                 | your-project                                     |
  | ${file}                    | 当前打开的文件名(完整路径)                       | /home/your-username/your-project/folder/file.ext |
  | ${relativeFile}            | 当前根目录到当前打开文件的相对路径(包括文件名)   | folder/file.ext                                  |
  | ${relativeFileDirname}     | 当前根目录到当前打开文件的相对路径(不包括文件名) | folder                                           |
  | ${fileBasename}            | 当前打开的文件名(包括扩展名)                     | file.ext                                         |
  | ${fileBasenameNoExtension} | 当前打开的文件名(不包括扩展名)                   | file                                             |
  | ${fileDirname}             | 当前打开文件的目录                               | /home/your-username/your-project/folder          |
  | ${fileExtname}             | 当前打开文件的扩展名                             | .ext                                             |
  | ${lineNumber}              | 当前激活文件所选行                               | 光标所在行                                       |
  | ${selectedText}            | 当前激活文件中所选择的文本                       | 编辑器中所选择的文本                             |
  | ${execPath}                | vscode 执行文件所在的目录                        | Code.exe 的位置                                  |
  | ${cwd}                     | 启动时 task 工作的目录                           |                                                  |
  | ${defaultBuildTask}        | 默认编译任务(build task)的名字                   |                                                  |

---

#### **VScode 配置 Clang**

---

- **插件配置**

  > 1、安装**Clangd**：提供比默认 C++ 拓展更强更快的代码提示，**需要关闭微软的原 C++ 拓展**。配置方法：下载后**需要额外下载 Clangd**([github 下载地址](https://github.com/clangd/clangd))，解压配置`bin`目录为**Path 环境变量**，在**拓展设置**设置**路径为 clangd**  
  > 2、安装**CodeLLDB**：下载后会**默认下载一个文件**，如果下载失败，在右下角**手动下载**，下载后在 VScode 调出**命令台**(`F1`或`Ctrl+Shift+P`)，搜索**从 VSIX 安装**-**显示本地**-**选择下载的文件**，完成后重载  
  > 3、配置**Code Runner**以便捷完成单文件编译运行：在拓展设置中找到**Executor Map**-**在 json 中编辑**，自行按照**编译器使用方法**配置编译选项。此外，由于微软**原 C++ 拓展**关闭，所以**code runner**的**自动调试**功能**不能使用**，需要自行在工作区**配置**`.vscode`**编译配置文件夹**，然后使用`F5`**启动调试**(或在**命令台**输入`debug`)

- `.vscode`**配置文件夹内的配置**

  > 1、在侧边栏的**调试**，选择**创建 launch.json**文件。或自己创建`.vscode`文件夹，其中创建`launch.json`文件，按照下方模板对照修改  
  > 2、唤出**控制台**，输入**task**，选择**配置任务**-**使用模板**-**Others**，

  ```json
  // launch.json
  // 此处所写都是调试任务如何执行
  {
      // 使用 IntelliSense 了解相关属性。
      // 悬停以查看现有属性的描述。
      // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
      "version": "0.2.0",
      "configurations": [
          {
              "type": "lldb",
              "request": "launch",
              "name": "Debug",
              "program": "${workspaceFolder}/${fileBasenameNoExtension}.exe",
              "args": [],
              "cwd": "${workspaceFolder}",
              "preLaunchTask": "Build C++",           // 创建前置编译任务
              "internalConsoleOptions": "neverOpen",  // 不默认切换到调试控制台
              "console": "integratedTerminal"         // 使用内置终端
          }
      ]
  }
  ```

  ```json
  // task.json
  // 此处所写都是编译任务
  {
      // See https://go.microsoft.com/fwlink/?LinkId=733558
      // for the documentation about the tasks.json format
      "version": "2.0.0",
      "tasks": [
          {
              "type": "shell",
              "label": "Build C++",
              "command": "clang++",                                       // 编译器名
              "args": [                                                   // 给编译器的参数
                  "-std=c++20",                                           // C++标准
                  "-g",                                                   // 表示debug模式，可以使用断点
                  "-Wall",                                                // 打开所有警告
                  "-o",                                                   // 重命名
                  "${workspaceFolder}/${fileBasenameNoExtension}.exe",    // 重命名的名称
                  "${file}"                                               // 当前打开的源代码
              ],
              "group": {
                  "kind": "build",
                  "isDefault": true
              }
          }
      ]
  }
  ```

---

#### **VScode 配置 Cmake**

---

- 码字中。。。

---

#### **页底评论**

---
