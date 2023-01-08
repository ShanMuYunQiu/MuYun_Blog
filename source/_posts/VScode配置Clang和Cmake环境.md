---
title: VScode配置Clang和Cmake环境
author: 圣奇宝枣
description: Vscode进阶篇，学习.vscode配置文件和Clang+Cmake的配置与使用
sticky: 0
date: 2023-01-05
updated: 2023-01-07
readmore: true
tags:
  - VScode
  - 环境配置
  - Cmake
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

- **环境配置**

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
              "type": "lldb",   // 调试器名
              "request": "launch",
              "name": "Debug",
              "program": "${workspaceFolder}/${fileBasenameNoExtension}.exe",   // 调试启动的程序
              "args": [],
              "cwd": "${workspaceFolder}",
              "preLaunchTask": "Build C++",           // 创建前置编译任务，后为启动的任务名
              "internalConsoleOptions": "neverOpen",  // 不默认切换到调试控制台
              "console": "integratedTerminal",        // 终端：使用内置终端
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
              "label": "Build C++",                                       // 任务名，与 launch.json 中对应
              "command": "clang++",                                       // 编译器名
              "args": [                                                   // 给编译器的参数
                  "${file}",                                              // 当前打开的源代码
                  "-std=c++20",                                           // C++标准
                  "-g",                                                   // 表示debug模式，可以使用断点
                  "-Wall",                                                // 打开所有警告
                  "-o",                                                   // 重命名
                  "${workspaceFolder}/${fileBasenameNoExtension}.exe",    // 重命名的名称
              ],
              "group": {
                  "kind": "build",
                  "isDefault": true
              }
          }
      ]
  }
  ```

- **Clangd 插件配置**

  > 1、打开**Clangd 拓展设置**-**Arguments**，添加如下项  
  > 2、`--compile-commands-dir=${workspaceFolder}/build`：引导 Clang 找到**编译指令文件**  
  > 3、`--all-scopes-completion`：**全局补全**(会自动补充头文件)  
  > 4、`--completion-style=detailed`：**更详细地补全内容**  
  > 5、`--header-insertion=iwyu`：**补充头文件的形式**(如果为`never`则关闭头文件补全)  
  > 6、`--background-index`：在**后台进行代码分析**  
  > 7、`--clang-tidy`：启用**clang-tidy**功能，自行配置`.clang-tidy`，配置**静态分析**  
  > 8、`--pch-storage=memory`：设置缓存文件**在内存读写**(还有一个选项为`disk`，即在磁盘读写)  
  > 9、`-j=12`：**代码分析**可以使用**多少个线程**，根据电脑配置合理更改

- 如果配置正常后，写代码**调用函数**时**Clangd**会**自动显示函数的形参名**，如果需要关闭，将**Inlay Hints**设置项关闭即可

---

#### **Clang Format代码整理**

---

- **Clang Format**可以进行代码整理格式化

- 在**文件夹根目录**创建`.clang-format`文件，进行配置(该文件格式实际是yaml，可以自行设置文件关联)

  - **官方文档**：[网页链接](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)

  - 如果习惯了**Prettier 插件**的**格式化风格**，直接使用下面的配置文件

    ```yaml
    BasedOnStyle: Microsoft
    UseTab: Never
    IndentWidth: 4
    ```

  - **自定义修改模板**(可能随版本更新与官网文档有出入)

    ```yaml
    # 语言: None, Cpp, Java, JavaScript, ObjC, Proto, TableGen, TextProto
    Language: Cpp
    # 默认继承自xxx模板，目前有 LLVM、Microsoft、Google、Chromium、Mozilla、Webkit、GNU
    BasedOnStyle: LLVM
    # 访问说明符(public、private等)的偏移
    AccessModifierOffset: -4
    # 开括号(开圆括号、开尖括号、开方括号)后的对齐: Align, DontAlign, AlwaysBreak(总是在开括号后换行)
    AlignAfterOpenBracket: Align
    # 连续赋值时，对齐所有等号
    AlignConsecutiveAssignments: true
    # 连续声明时，对齐所有声明的变量名
    AlignConsecutiveDeclarations: true
    # 左对齐逃脱换行(使用反斜杠换行)的反斜杠
    AlignEscapedNewlinesLeft: true
    # 水平对齐二元和三元表达式的操作数
    AlignOperands: true
    # 对齐连续的尾随的注释
    AlignTrailingComments: true
    # 允许函数声明的所有参数在放在下一行
    AllowAllParametersOfDeclarationOnNextLine: true
    # 允许短的块放在同一行
    AllowShortBlocksOnASingleLine: false
    # 允许短的case标签放在同一行
    AllowShortCaseLabelsOnASingleLine: false
    # 允许短的函数放在同一行: None, InlineOnly(定义在类中), Empty(空函数), Inline(定义在类中，空函数), All
    AllowShortFunctionsOnASingleLine: Empty
    # 允许短的if语句保持在同一行
    AllowShortIfStatementsOnASingleLine: false
    # 允许短的循环保持在同一行
    AllowShortLoopsOnASingleLine: false
    # 总是在定义返回类型后换行(deprecated)
    AlwaysBreakAfterDefinitionReturnType: None
    # 总是在返回类型后换行: None, All, TopLevel(顶级函数，不包括在类中的函数), 
    #   AllDefinitions(所有的定义，不包括声明), TopLevelDefinitions(所有的顶级函数的定义)
    AlwaysBreakAfterReturnType: None
    # 总是在多行string字面量前换行
    AlwaysBreakBeforeMultilineStrings: false
    # 总是在template声明后换行
    AlwaysBreakTemplateDeclarations: false
    # false表示函数实参要么都在同一行，要么都各自一行
    BinPackArguments: true
    # false表示所有形参要么都在同一行，要么都各自一行
    BinPackParameters: true
    # 在大括号前换行: Attach(始终将大括号附加到周围的上下文), Linux(除函数、命名空间和类定义，与Attach类似), 
    #   Mozilla(除枚举、函数、记录定义，与Attach类似), Stroustrup(除函数定义、catch、else，与Attach类似), 
    #   Allman(总是在大括号前换行), GNU(总是在大括号前换行，并对于控制语句的大括号增加额外的缩进), WebKit(在函数前换行), Custom
    #   注：这里认为语句块也属于函数
    BreakBeforeBraces: Custom
    # 大括号换行，只有当BreakBeforeBraces设置为Custom时才有效
    BraceWrapping:   
      # class定义后面
      AfterClass: false
      # 控制语句后面
      AfterControlStatement: false
      # enum定义后面
      AfterEnum: false
      # 函数定义后面
      AfterFunction: false
      # 命名空间定义后面
      AfterNamespace: false
      # ObjC定义后面
      AfterObjCDeclaration: false
      # struct定义后面
      AfterStruct: false
      # union定义后面
      AfterUnion: false
      # catch之前
      BeforeCatch: true
      # else之前
      BeforeElse: true
      # 缩进大括号
      IndentBraces: false
    # 在二元运算符前换行: None(在操作符后换行), NonAssignment(在非赋值的操作符前换行), All(在操作符前换行)
    BreakBeforeBinaryOperators: NonAssignment
    # 在三元运算符前换行
    BreakBeforeTernaryOperators: true
    # 在构造函数的初始化列表的逗号前换行
    BreakConstructorInitializersBeforeComma: false
    # 每行字符的限制，0表示没有限制
    ColumnLimit: 200
    # 描述具有特殊意义的注释的正则表达式，它不应该被分割为多行或以其它方式改变
    CommentPragmas: '^ IWYU pragma:'
    # 构造函数的初始化列表要么都在同一行，要么都各自一行
    ConstructorInitializerAllOnOneLineOrOnePerLine: false
    # 构造函数的初始化列表的缩进宽度
    ConstructorInitializerIndentWidth: 4
    # 延续的行的缩进宽度
    ContinuationIndentWidth: 4
    # 去除C++11的列表初始化的大括号{后和}前的空格
    Cpp11BracedListStyle: false
    # 继承最常用的指针和引用的对齐方式
    DerivePointerAlignment: false
    # 关闭格式化
    DisableFormat: false
    # 自动检测函数的调用和定义是否被格式为每行一个参数(Experimental)
    ExperimentalAutoDetectBinPacking: false
    # 需要被解读为foreach循环而不是函数调用的宏
    ForEachMacros: [ foreach, Q_FOREACH, BOOST_FOREACH ]
    # 对#include进行排序，匹配了某正则表达式的#include拥有对应的优先级，匹配不到的则默认优先级为INT_MAX(优先级越小排序越靠前)，
    #   可以定义负数优先级从而保证某些#include永远在最前面
    IncludeCategories: 
      - Regex: '^"(llvm|llvm-c|clang|clang-c)/'
        Priority: 2
      - Regex: '^(<|"(gtest|isl|json)/)'
        Priority: 3
      - Regex: '.*'
        Priority: 1
    # 缩进case标签
    IndentCaseLabels: false
    # 缩进宽度
    IndentWidth: 4
    # 函数返回类型换行时，缩进函数声明或函数定义的函数名
    IndentWrappedFunctionNames: false
    # 保留在块开始处的空行
    KeepEmptyLinesAtTheStartOfBlocks: true
    # 开始一个块的宏的正则表达式
    MacroBlockBegin: ''
    # 结束一个块的宏的正则表达式
    MacroBlockEnd: ''
    # 连续空行的最大数量
    MaxEmptyLinesToKeep: 1
    # 命名空间的缩进: None, Inner(缩进嵌套的命名空间中的内容), All
    NamespaceIndentation: Inner
    # 使用ObjC块时缩进宽度
    ObjCBlockIndentWidth: 4
    # 在ObjC的@property后添加一个空格
    ObjCSpaceAfterProperty: false
    # 在ObjC的protocol列表前添加一个空格
    ObjCSpaceBeforeProtocolList: true
    # 在call(后对函数调用换行的penalty
    PenaltyBreakBeforeFirstCallParameter: 19
    # 在一个注释中引入换行的penalty
    PenaltyBreakComment: 300
    # 第一次在<<前换行的penalty
    PenaltyBreakFirstLessLess: 120
    # 在一个字符串字面量中引入换行的penalty
    PenaltyBreakString: 1000
    # 对于每个在行字符数限制之外的字符的penalty
    PenaltyExcessCharacter: 1000000
    # 将函数的返回类型放到它自己的行的penalty
    PenaltyReturnTypeOnItsOwnLine: 60
    # 指针和引用的对齐: Left, Right, Middle
    PointerAlignment: Left
    # 允许重新排版注释
    ReflowComments: true
    # 允许排序#include
    SortIncludes: true
    # 在C风格类型转换后添加空格
    SpaceAfterCStyleCast: false
    # 在赋值运算符之前添加空格
    SpaceBeforeAssignmentOperators: true
    # 开圆括号之前添加一个空格: Never, ControlStatements, Always
    SpaceBeforeParens: ControlStatements
    # 在空的圆括号中添加空格
    SpaceInEmptyParentheses: false
    # 在尾随的评论前添加的空格数(只适用于//)
    SpacesBeforeTrailingComments: 2
    # 在尖括号的<后和>前添加空格
    SpacesInAngles: true
    # 在容器(ObjC和JavaScript的数组和字典等)字面量中添加空格
    SpacesInContainerLiterals: true
    # 在C风格类型转换的括号中添加空格
    SpacesInCStyleCastParentheses: true
    # 在圆括号的(后和)前添加空格
    SpacesInParentheses: true
    # 在方括号的[后和]前添加空格，lamda表达式和未指明大小的数组的声明不受影响
    SpacesInSquareBrackets: true
    # 标准: Cpp03, Cpp11, Auto
    Standard: Cpp11
    # tab宽度
    TabWidth: 4
    # 使用tab字符: Never, ForIndentation, ForContinuationAndIndentation, Always
    UseTab: Never
    ```

- **插件推荐**

  > 1、上述为**文件配置**，现在可以下载使用**clang-format 插件**，设置**Fallback Style**项为**继承模板名**(如`Microsoft`)，其余自定义设置请自行学习  
  > 2、此外可以安装**clang-tidy 插件**来提供代码优化建议

---

#### **VScode 配置 Cmake**

---

- **下载 Cmake 和 Ninja**

[Cmake 官网](https://cmake.org/)
[Ninja github仓库](https://github.com/ninja-build/ninja)

- **配置环境**

  > 1、一定注意**安装 Cmake**时勾选**添加进环境变量**(英文选项)。如果忘记勾选，则需要手动将**bin 目录**添加进**Path 环境变量**。此外还需要将`Ninja.exe`**所在的目录**添加到**Path 环境变量**  
  > 2、下载**Cmake**和**Cmake Tools**拓展  
  > 3、具体**Cmake 如何使用**请**另行学习**，当写好`CMakeLists.txt`后，唤出**控制台**，输入**Cmake**，选择**Cmake：Configure**，选择工具链。便会开始构建项目，生成 build 目录，生成编译后的文件

- **Cmake 的调试**

  > 1、**Cmake调试**不需要`tasks.json`，删除该文件，同时把`launch.json`中的`"preLaunchTask"`项删除  
  > 2、更改`launch.json`文件的`"program"`项为`${command:cmake.launchTargetPath}`(为Cmake Tools拓展的预定义替换变量)

---

#### **页底评论**

---
