---
title: VScode配置
author: 圣奇宝枣
description: 个人以及大众最爱的编辑器，新手时期被繁杂的配置劝退过，而本教程将带你渡过难关
sticky: 0
date: 2022-04-27 12:56:14
updated: 2022-07-11 20:04:12
readmore: true
tags:
  - 前置准备
  - VScode
  - 环境配置
categories:
  - 代码编辑器
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
> **Workbench Tree render Indent** **Guides**：文件树缩进线，建议设置 always
> **git.mergeEditor**：git merge 合并编辑器，可以更方便地处理冲突，建议设置 true

</div>

---

#### **拓展推荐**

---

<div class="warning">

> **Live Server**：实时显示刷新页面，网页使用  
> ----设置：**Auto Save**改为**afterDelay**，**Auto Delay Save**为自动保存时间(1000=1 秒)  
> ----运行时鼠标右键，选择**Open with Live Server**即可  
> **indent-rainbow**：彩色缩进，缩进错误会报大红  
> **Bracket Pair Colorizer**：彩色括号，方便读代码  
> **code-translate**(w88975)：即时变量名翻译  
> **Prettier**：格式化代码，自动排版  
> ----设置：**Format On Save**，保存时自动排版  
> ----代码格式化大括号不独立换行：**C_Cpp:Clang_format_style**设置为`{BasedOnStyle: Chromium, IndentWidth: 4}`  
> **Markdown Preview Enhanced**：分屏 markdown 预览  
> **C++ Intellisense**：c++代码自动补全，查找代码引用，跳转到定义，列举 symbols  
> **One Dark Pro**：黑灰色主题，好看  
> **Github Copilot**：AI 自动补全写代码，需要 github 账号登录，并且需要提前申请，通过后才可以使用(已收费，[官网](https://copilot.github.com/))  
> **Tabnine AI Autocomplete**：上面 github copilot 的下位替代，有免费版本([官网](https://www.tabnine.com/))  
> **Power Mode**：打字炫酷特效，需要在设置里自行设定  
> **Auto Rename Tag**：自动重命名相对应的闭合 HTML 标签  
> **Better Comments**：更好的注释，注释信息变色  
> ----使用：星号\*高亮亮绿色，叹号!警告红色，问号?记录问题蓝色，关键词 TODO 规划区域橙色  
> **Color Highlight**：显示色码颜色，在设置中设置**Marker Type**设置现实样式，推荐**dot-before**

</div>

---

#### **C/C++环境配置**

---

- 下载**mingw64**编译器

[MinGW64 官方下载网站(已废弃)](https://www.mingw-w64.org/downloads/)

[SourceForge 下载 MinGW64](https://sourceforge.net/projects/mingw-w64/files/?source=navbar)

- **下载注意事项**
  > 1、下载时**向下翻**下载**压缩包**文件，不要使用在线安装，无法使用  
  > 2、**64 位系统**选择**x86_64**，**32 位系统**选择**i686**  
  > 3、开发**Windows 下程序**选择**Win32**，开发**其他操作系统下程序**选择**posix**  
  > 4、**异常处理模型**中，**seh**是**新发明**的且**性能好**，但**不支持 32 位**，**sjlj**较为**古老**，**稳定性好**且**支持 32 位**
- 解压后找地方存放，复制目录下的**bin**目录的路径
- 环境变量设置：**此电脑**-**属性**-**高级系统设置**-**环境变量**-**Path**-(一个框)选择到最后，加一个英文的分号`;`，粘贴你的路径。(列表)选择到最下方，新建，粘贴路径
- **VScode**中安装**C/C++ Extension Pack**插件包
- 重启，打开存代码的文件夹，新建文件为`.cpp`文件即可
- 代码运行点击右上角**Run** **Code**(Ctrl+Alt+N)即可运行
- 想要调试运行，点击**运行与调试**(Ctrl+Shift+D)，选择**GDB/LLDB**，**g++.exe**，按**F5**运行调试，也可以在**行号**前打**断点**进行调试，上方**按步运行**按键来控制运行(调试全路径不能有中文)
- **Encoding**：将**UTF-8**改为**GB18030**以正常显示中文

[参考 bilibili 视频教程](https://www.bilibili.com/video/BV1UL411b7B4?spm_id_from=333.337.search-card.all.click)

---

#### **Python 环境配置**

---

- **Python**下载，一定勾选**Add Python to Path**(自动添加到环境变量)

[Python 官方网站](https://www.python.org/)

- 检查环境变量设置，查漏补缺添加环境变量(同 C/C++环境配置)，将 python**根目录**添加到环境变量中
- **VScode**中安装**Python Extension Pack**插件包
- 重启，打开存代码的文件夹，新建文件为`.py`文件即可
- 代码运行点击右上角**Run** **Code**中**Run Python File**即可运行

[参考菜鸟教程文字教程](https://www.runoob.com/python3/python3-install.html)

---

#### **Java 环境配置**

---

- **Java JDK**下载，VScode 插件支持目前只支持到 Java17 及以上，推荐使用 Java17

[Oracle 官方网站](https://www.oracle.com/cn/java/)

- 环境变量设置：同 C/C++环境配置进入环境变量设置
  - 新建系统变量**JAVA_HOME**，路径设置为**JDK 总文件夹的路径**
  - 新建**CLASSPATH**，填入以下内容：`.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar`
  - 找到**Path**变量，添加 **%JAVA_HOME%\bin** 和 **%JAVA_HOME%\jre\bin**
- **VScode**中安装**Extension Pack for Java**插件包
- **VScode 设置**，**用户**类别下，找到**Code Actions On Save**，在.json 中编辑，将新增的`"editor.codeActionsOnSave"`这一些代码删除，在第一层缩进下，在`"workbench.iconTheme": "vscode-icons",`前写入`"java.jdt.ls.java.home": "JDK的路径(注意反斜杠转义)",`
- 重启**VScode**，创建`.java`文件即可使用，运行前需要在**VScode**的**调试**处调试并自动生成文件，后续可以直接使用**Run**运行

[参考 bilibili 视频教程](https://www.bilibili.com/video/BV16y4y177b9)

---

#### **页底评论**

---
