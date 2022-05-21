---
title: Git简单教程
author: 圣奇宝枣
description: 版本管理工具Git的基本使用教程，内含常用本地命令，分支命令，远程命令以及一些优化
date: 2022-05-20 08:30:14
updated: 2022-05-20 10:56:37
tags:
  - Git
  - 版本管理工具
  - 教程分享
categories:
  - 其他教程
---

---

#### **Git 的工作流程概述**

---

> 1.**clone**(**克隆**)：从远程仓库中克隆代码到本地仓库  
> 2.**checkout**(**检出**)：从本地仓库中检出一个仓库分支然后进行修订  
> 3.**add**(**添加**)：在提交前先将代码提交到暂存区  
> 4.**commit**(**提交**)：提交到本地仓库。本地仓库中保存修改的各个历史版本  
> 5.**fetch**(**抓取**)：从远程库，抓取到本地仓库，不进行任何的合并动作，一般操作比较少。  
> 6.**pull**(**拉取**)：从远程库拉到本地库，自动进行合并(merge),然后放到到工作区，相当于 fetch+merge  
> 7.**push**(**推送**)：修改完成后，需要和团队成员共享代码时，将代码推送到远程仓库

---

#### **Git 安装与基础命令**

---

- 在**Git Bash**中可以使用一些**Linux**的**基本命令**

  - `ls`/`ll`：查看当前目录文件，`ll`同 Linux 的`ls -l`查看隐藏文件
  - `cat`：查看文件内容
  - `touch`：创建文件
  - `vi`：使用 vi 编辑器

- 安装 Git 后首先要进行**用户配置**

  - 打开**Git Bash**，执行`git config --global user.name "名称"`，设置用户名
  - 执行`git config --global user.email "邮箱"`，设置邮箱
  - 不写后面修改的信息，则为查看当前设置的信息

- 为**常用指令**配置**别名**(根据个人需求)

  - 在用户目录下创建`.bashrc`文件，如果不允许使用点开头，则可以在**Git Bash**使用`touch ~/.bashrc`指令
  - 在`.bashrc`文件中按`alias 别名='被替换的指令'`格式写入

- 解决 Git**乱码问题**

  - 在**Git Bash**中输入`git config --global core.quotepath false`
  - 然后在**根目录\etc\bash.bashrc**文件中加入下面两行：

  ```
  export LANG="zh_CN.UTF-8"
  export LC_ALL="zh_CN.UTF-8"
  ```

---

#### **git 常用本地命令**

---

- `git init`：在需要管理的**文件夹**下使用`git init`**初始化**本地仓库，随后文件夹下会多出`.git`的隐藏文件夹,用于 git 存储信息
- `git add`：使用`git add .`将目录下所有文件**添加更新**到**暂存区**，也可将点变为文件名添加指定文件
- `git commit`：使用`git commit -m "提交说明"`将**暂存区**的修改提交到**本地仓库**
- `git status`：使用`git status`查看文件状态
- `git log` ：使用`git log`查看文件提交历史，可以用**后缀**来**优化显示**，执行下列指令，将使用`git lg`作为别名
  ```
  git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
  ```
- `git reset`：使用`git reset --hard 标识ID`**回退**到**需要的版本**
- `git reflog`：使用`git reflog`**查看已删除**的提交记录

---

#### **git 分支管理**

---

- `git branch`：**查看**本地分支
- `git branch 分支名`：**创建**本地分支
- `git checkout 分支名`：**切换**分支
- `git checkout -b 分支名`：**创建并切换**分支
- `git merge 分支名`：**合并**分支，将**输入**的分支**合并到当前**所在分支中
- `git branch -d 分支名`：**删除**分支，将`-d`换为`-D`为强制删除
- **合并冲突**：合并时**出现冲突**，查看冲突的文件，箭头指向 HEAD 的为当前分支，此外为其他分支，将冲突文件中冲突**手动解决**(**只留下需要的内容**)，再次提交

---

#### **远程仓库托管**

---

- **创建**远程仓库，可选 github、gitee、gitlib 等
- **配置 SSH 公钥**：使用`ssh-keygen -t rsa`**创建**公钥，一路回车即可，如果已存在则自动覆盖。使用`cat ~/.ssh/id_rsa.pub`**查看**公钥，复制后写入设置(随后可以访问 SSH 链接)
- **连接仓库**：使用`git remote add origin(本地显示名，最好写origin) 仓库地址`**连接**仓库，使用`git remote`**查看**连接的仓库
- **推送**：使用`git push origin(仓库名) master(本地分支)`**推送**到远程仓库，表示将本地 master 分支推送到 origin 所连接的仓库。如果**远程仓库分支名**与**本地相同**则可以**只写分支名**
- **建立关联**：使用`git push --set-upstream origin master`可以**建立关联**，此后可以**不写仓库名**与**分支名**
- **其他补充**：使用`git push origin 本地分支名:远端分支名`来将**指定本地分支**推到**指定远端分支**，此外，在 origin 前写`-f`表示**强制覆盖**
- `git clone`：使用`git clone 仓库地址 本地路径`将远程仓库**所有**文件**克隆**到本地
- `git fetch`：使用`git fetch 仓库名 分支名`**抓取**到本地，**但不会进行合并**，不指定名称则为全部
- `git pull`：使用`git pull 仓库名 分支名`**拉取**到本地，**会自动合并**，不指定名称则为全部
- **处理冲突**：使用`git pull`来处理冲突

---

#### **页底评论**

---
