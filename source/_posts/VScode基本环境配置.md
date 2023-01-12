---
title: VScode基本环境配置
author: 圣奇宝枣
description: 个人以及大众最爱的编辑器，新手时期被繁杂的配置劝退过，而本教程将带你渡过难关
sticky: 0
date: 2022-04-27
updated: 2023-01-05
readmore: true
tags:
  - 前置准备
  - VScode
  - 环境配置
categories:
  - 编辑器与IDE
---

---

#### **VScode 下载**

---

[VScode 官方网站](https://code.visualstudio.com/)

---

#### **基础配置**

---

<div class="info">

> 1、在左下角找到**账户**登录，我个人使用**github**登录，记得打开设置同步，以此同步设置(以前设置过并且同步正常的话下方就不用看了)  
> 2、首次使用可能需要设置语言，找到**拓展**(Ctrl+Shift+X)，搜索**Chinese**，下载语言包  
> 3、安装**Code Runner**插件

</div>

---

<!-- more -->

#### **其他基础设置**

---

- **“齿轮”-设置**：

<div class="success">

> **Mouse Wheel Zoom**：按住 Ctrl 滑动滚轮调整字体大小  
> **Run In Terminal**：在终端运行  
> **Trim Final Newlines**：保存时删除末尾多余新行  
> **Trim Trailing Whitespace**：保存时删除行末尾多余空格  
> **Workbench Tree Indent**：文件树缩进，建议设置 18  
> **Workbench Tree render Indent - Guides**：文件树缩进线，建议设置 always  
> **Auto Save**：改为**afterDelay**，随后**Auto Delay Save**为自动保存时间(1000=1 秒)  
> **git.mergeEditor**：git merge 合并编辑器，可以更方便地处理冲突，建议设置 true  
> **Snippets Prevent Quick Suggestions**：阻止快速建议，建议关闭，关闭后可以在使用代码补全后仍继续收到提示  
> **smooth**：搜索出的结果，分别代表四项不同的平滑效果设置，按需开启  
> **Font Family**：配置字体(如写入**JetBrains Mono**)  
> **Font Ligatures**：配置是否启用字体连字特性(在 JSON 中打开，设置为 true)

</div>

---

#### **拓展推荐**

---

<div class="warning">

> **xxx Extension Pack**：**xxx**改为需要的语言，此拓展为微软配置的开发环境支持拓展包  
> **Live Server**：实时显示刷新页面，网页使用  
> ----运行时右下角**Go Live**，或鼠标右键，选择**Open with Live Server**即可  
> **indent-rainbow**：彩色缩进，缩进错误会报大红  
> **Bracket Pair Colorizer**：彩色括号，方便读代码(已内置于 VScode 的**Bracket Pair Colorization**设置项)  
> **Code Translate**(w88975)：划词翻译，且显示变量名定义类型与语法使用规则  
> **Prettier**：格式化代码，自动排版  
> ----设置：**Format On Save**，保存时自动排版  
> ----代码格式化大括号不独立换行：**C_Cpp:Clang_format_style**设置为`{BasedOnStyle: Chromium, IndentWidth: 4}`  
> **One Dark Pro**：黑灰色主题，好看  
> **Material Icon Theme**：图标主题，好看且易分辨  
> **Github Copilot**：AI 自动补全写代码，需要 github 账号登录，并且需要提前申请，通过后才可以使用(已收费，[官网](https://copilot.github.com/))  
> **CodeGeeX**：github copilot 的下位替代，开源免费的 AI 补全([官网](https://models.aminer.cn/codegeex/))
> **Tabnine AI**：github copilot 的下位替代，有免费版本([官网](https://www.tabnine.com/))  
> **Power Mode**：打字炫酷特效，需要在设置里自行设定  
> **Auto Close Tag**：自动闭合标签(已内置于 VScode 的**Auto Closing Tags**设置项)  
> **Auto Rename Tag**：自动重命名相对应的闭合 HTML 标签(已内置于 VScode 的**Linked Editing**设置项)  
> **Color Highlight**：显示色码(#000000)颜色，在设置中设置**Marker Type**设置显示样式，推荐**dot-before**  
> **Markdown all in one**：分屏 markdown 预览，更多 Markdown 功能  
> **Excel Viewer**：在 VScode 中打开 Excel 表格  
> **Hex Editor**：在 VScode 中编辑二进制文件  
> **exe Runner**：右键更方便的执行.exe 文件  
> **Bookmarks**：书签跳转，使用右键在需要的行上设置书签以快速跳转  
> **GitLens**：侧边栏显示光标所在位置的历史 git 提交并快速访问，拓展 VScode 自带 git 管理器的功能  
> **Git Graph**：将 git 提交记录转换为可视化图表  
> **CodeSnap**：快速美观代码截图  
> **Project Manager**：更方便的管理文件夹和项目  
> **Better Comments**：更好的注释，注释信息变色：包括`TODO`、`!`、`?`、`*`、`//`等注释高亮  
> **Todo Tree**：自动集合整理所有 TODO 待办事项  
> **Draw.io Integration**：将**Draw.io**集成到 VScode 中，画流程图，文件后缀为`.drawio`  
> **Codelf**：变量命名插件(注意名字是l不是i)  

</div>

---

#### **配置用户代码片段**

---

- VScode 允许用户**配置**自己的**代码片段**，以快速完成**经常重复输入的代码**

- **进入配置文件**：**文件**-**首选项**-**配置用户代码片段**-**选择配置的语言**

- **配置自己的代码片段**

  ```json
  "对象名"{
      "prefix": "关键字",
      "body": [
          "代码片段第1行",
          "代码片段第2行",
          "",
          "空行后代码片段第3行"
      ],
      "description": "描述"
  }
  ```

- **配置说明**

  > 1、**对象名**：任意  
  > 2、**prefix**：触发所需的关键字  
  > 3、**body**：触发后的代码片段  
  > 4、**description**：描述  
  > 5、_占位符_：使用**Dollar 符**`$`表示占位，放置 Dollar 符可以使代码片段的**鼠标光标**默认出现在**占位符**处，使用**Tab**可以**快速跳转**到下一个占位符处。此外可以给 Dollar 符编号，即使用`$1`，`$2`等符号设置光标切换的顺序。还可以给占位符处设置默认的值，比如`printf("&{1:hello} ${2:world}");`，这样片段默认带有**hello**和**world**，且鼠标光标定位到对应占位符时**自动选中默认的值**以便修改

---

#### **C/C++环境配置**

---

- **下载编译器**(按需下载，不需要都下载，新手推荐使用下面第二个链接。速度慢，可复制下载链接使用最下面的站点加速)

[MinGW-w64 官方下载网站(已废弃下载，只提供其他集成支持下载地址)](https://www.mingw-w64.org/downloads/)

[MinGW-w64 单独下载(github 仓库地址)](https://github.com/niXman/mingw-builds-binaries)

[下载 LLVM/Clang(github 仓库地址)](https://github.com/llvm/llvm-project/tree/llvmorg-15.0.6)

[winlibs.com 下载 WinLibs 套件(套件包含 GCC + LLVM/Clang/LLD/LLDB + MinGW-w64)](https://winlibs.com/)

[github 加速站点 1](https://github.welab.eu.org/) [github 加速站点 2](https://d.serctl.com/)

- **MinGW-w64 下载说明**(下载其他编译器可忽略)

  > 1、下载时**向下翻**下载**压缩包**文件，不要使用在线安装，无法使用  
  > 2、**64 位系统**选择**x86_64**，**32 位系统**选择**i686**  
  > 3、开发**Windows 下程序**选择**Win32**，开发**其他操作系统下程序**选择**posix**  
  > 4、**异常处理模型**中，**seh**是**新发明**的且**性能好**，但**不支持 32 位**，**sjlj**较为**古老**，**稳定性好**且**支持 32 位**

- **配置环境**

  > 1、解压后找地方存放，复制目录下的**bin**目录的路径  
  > 2、环境变量设置：**此电脑**-**属性**-**高级系统设置**-**环境变量**-**Path**-(如果一个框)选择到最后，加一个英文的分号`;`，粘贴你的`bin`路径。(如果列表)选择到最下方，新建，粘贴路径  
  > 3、**VScode**中安装**C/C++ Extension Pack**插件包  
  > 4、重启，打开存代码的文件夹，新建文件为`.cpp`文件即可  
  > 5、代码运行点击右上角**Run Code**(Ctrl+Alt+N)即可运行  
  > 6、想要调试运行，点击**运行与调试**(Ctrl+Shift+D)，选择**GDB/LLDB**，**g++.exe**，按**F5**运行调试，也可以在**行号**前打**断点**进行调试，上方**按步运行**按键来控制运行(调试全路径不能有中文)  
  > 7、**Encoding**：将编码格式**UTF-8**改为**GB18030**以正常显示中文

---

#### **Python 环境配置**

---

- **Python**下载，一定勾选**Add Python to Path**(自动添加到环境变量)

[Python 官方网站](https://www.python.org/)

- **配置环境**

  > 1、检查环境变量设置，查漏补缺添加环境变量(同 C/C++环境配置)，将 python**根目录**添加到环境变量中  
  > 2、**VScode**中安装**Python Extension Pack**插件包  
  > 3、重启，打开存代码的文件夹，新建文件为`.py`文件即可  
  > 4、代码运行点击右上角**Run** **Code**中**Run Python File**即可运行

---

#### **Java 环境配置**

---

- **Java JDK**下载，VScode 插件支持目前只支持到 Java17 及以上，如需使用老版本 JDK 请自行将插件降版本

[Oracle 官方网站](https://www.oracle.com/cn/java/)

- **配置环境**：

  > 1、同 C/C++环境配置进入环境变量设置  
  > 2、新建系统变量**JAVA_HOME**，路径设置为**JDK 总文件夹的路径**  
  > 3、新建**CLASSPATH**，填入以下内容：`.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar`  
  > 4、找到**Path**变量，添加 **%JAVA_HOME%\bin** 和 **%JAVA_HOME%\jre\bin**  
  > 5、**VScode**中安装**Extension Pack for Java**插件包  
  > 6、**Language Support For Java 拓展设置**下，找到**Java›Jdt›Ls›Java:Home**设置项，**在.json 中编辑**，在`"java.jdt.ls.java.home:"`后写入`"JDK的路径(有双引号，注意反斜杠转义)"`  
  > 7、重启**VScode**，创建`.java`文件即可使用，运行前需要在**VScode**的**调试**处调试并自动生成文件，后续可以直接使用**Run**运行

---

#### **页底评论**

---
