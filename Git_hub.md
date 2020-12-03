[TOC]
# [[Git]]

## 三个区域

- 工作区 / Working Directory

- 暂存区 / Staging Index

- 仓库区 / Repository

:bulb: ​

1. 文件最开始在工作区
2.  使用shell命令**移动**到暂存区
3. 最后**提交**到仓库区



:warning: commit只能提交暂存区的内容

## 安装和配置

1. [下载地址](https://git-scm.com/downloads)

2. 配置终端（可选）

   1. `ls-a`查看主目录是否已经有`.bash_profile`

   2. `atom .bash_profile`打开文件进行编辑

      ```bash
      # Enable tab completion
      source ~/.udacity-terminal-config/git-completion.bash
      
      # Change command prompt
      source ~/.udacity-terminal-config/git-prompt.sh
      
      # colors!
      red="\[\033[38;5;203m\]"
      green="\[\033[38;05;38m\]"
      blue="\[\033[0;34m\]"
      reset="\[\033[0m\]"
      
      export GIT_PS1_SHOWDIRTYSTATE=1
      
      # '\u' adds the name of the current user to the prompt
      # '\$(__git_ps1)' adds git-related stuff
      # '\W' adds the name of the current directory
      export PS1="$red\u$green\$(__git_ps1)$blue \W
      $ $reset"
      ```

3. 配置Git

      1. 设置你的 Git 用户名`git config --global user.name "<Your-Full-Name>"`
      2. 设置你的 Git 邮箱`git config --global user.email "<your-email-address>"`
      3. 确保 Git 输出内容带有颜色标记`git config --global color.ui auto`
      4. 对比显示原始状态`git config --global color.ui autogit config --list`
      5. `git config --list`

4. 配置代码编辑器

      1. Atom Editor 设置

         > ```bash
         > git config --global core.editor "atom --wait"
         > ```

      2. Sublime Text 设置

         > ```bash
         > git config --global core.editor "'/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl' -n -w"
         > ```

      3. VSCode 设置

         > ```bash
         > git config --global core.editor "code --wait"
         > ```



[Git 基础 - 获取 Git 仓库 - 英](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository#Initializing-a-Repository-in-an-Existing-Directory) | [中](https://git-scm.com/book/zh/v2/Git-基础-获取-Git-仓库)

## Git 命令

### git init

- init 子命令是"initialize"（初始化）的简称，这个命令很有用，因为它将进行所有仓库初始设置。

- 在当前目录下创建新的空仓库。

- 运行此命令可以创建隐藏 `.git` 目录。此 `.git` 目录是仓库的核心/存储中心。它存储了所有的配置文件和目录，以及所有的 commit。

  :bulb: 下面简要概述了 `.git` 目录下的各项内容：

    - **config 文件** - 存储了所有与项目有关的配置设置。

        摘自于 [Git Book - 英](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration) | [中文点此处](https://git-scm.com/book/zh/v2/自定义-Git-配置-Git)：

        > Git 会查看 Git 目录下你当前所使用仓库对应的配置文件（.git/config）中的配置值。这些值仅适用于当前仓库。

        例如，假设你将 Git 全局配置为使用你的个人电子邮箱。如果你想针对某个项目使用你的工作邮箱，则此项更改会被添加到该文件中。

    - **description 文件** - 此文件仅用于 GitWeb 程序，因此可以忽略

    - **hooks 目录** - 我们会在此处放置客户端或服务器端脚本，以便用来连接到 Git 的不同生命周期事件

    - **info 目录** - 包含全局排除文件

    - **objects 目录** - 此目录将存储我们提交的所有 commit

    - **refs 目录** - 此目录存储了指向 commit 的指针（通常是“分支”和“标签”）

        注意，除了 hooks 目录，你应该不会对这里的其他内容有太多的困扰。hooks 目录可以用来连接到 Git 工作流的不同部分或事件，但是在这门课程中，我们不会对此作过多介绍。
  
- `rm -rf .git` 删除.git文件可将当前目录下的git仓库删除，不会影响子文件夹下的git仓库

- 深入研究
  - Git 内部原理 - 底层命令和高层命令 : [英](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain) | [中](https://git-scm.com/book/zh/v2/Git-内部原理-底层命令和高层命令)（进阶内容——请将此网址添加到书签中，并在以后查看）
  - [自定义 Git - Git Hooks - 英](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) | [中](https://git-scm.com/book/zh/v2/自定义-Git-Git-钩子)

### git clone

```bash
git clone https://github.com/udacity/course-git-blog-project
```

- `git clone` 命令被用于将项目仓库复制到当前目录
- `git clone <path-to-repository-to-clone> new_name` 将项目**重命名**为 new_name

### git status

- 支持中文

  ```bash
  git config --global core.quotepath false
  ```

- 告诉我们已在工作目录中被创建但 Git 尚未开始跟踪的新文件

- Git 正在跟踪的已修改文件

- 显示技巧

  - `git status | less`
  - 要向下滚动，按下
    - j 或 ↓ 一次向下移动一行
    - d 按照一半的屏幕幅面移动
    - f 按照整个屏幕幅面移动
  - 要 向上滚动，按上
    - k 或 ↑ 一次向上移动一行
    - u 按照一半的屏幕幅面移动
    - b 按照整个屏幕幅面移动
  - 按下 q 可以退出日志（返回普通的命令提示符）

### git add **将文件上传到暂存区**

- git rm --cached <file>，删除**暂存区**文件，不会破坏**工作区**文件。
- 可接受多个文件名（用**空格**分隔）
-  `git add . `来代替文件列表，告诉 git 添加当前目录至暂存区（以及所有嵌套文件）

### git commit 

- 

### git log **显示SHA，提交者信息，提交时间，提交内容备注**

- **--oneline 只显示SHA前7位，提交内容备注**

- SHA 作为最后一个参数 git log -p fdf5493

- **--stat 显示 commit 中更改的文件以及添加或删除的行数(stat 是“统计信息 statistics”的简称)**

- **--patch or -p 显示对文件作出的实际更改**
  
  ![img](Git_hub.assets/ud123-l3-git-log-p-lines-removed-annotated.png)
  
  - 🔵 - 正在显示的文件
  - 🔶 - 文件第一版的哈希值和第二版的哈希值
    - 通常不重要，因此可以忽略
  - ❤️ - 文件的旧版本和当前版本
  - 🔍 - 添加的行所在的位置以及添加了多少行
    - -15,83 表示旧版本（用 - 表示）从第 15 行开始，显示了 83 行
    - +15,85 表示当前版本（用 + 表示）从第 15 行开始，现在变成了 85 行...这 85 行显示在下方
  - ✏️ - 在 commit 中实际进行的更改
    - 用红色标示并以减号 (-) 开头的行是位于文件原始版本中，但是被 commit 删除的行
    - 用绿色标示并以加号 (+) 开头的行是 commit 新加的行
  - -w 忽略空格更改向所有这些命令提供 commit 的 
  
- **--graph 选项将条目和行添加到输出的最左侧。显示了实际的分支。**

- **--all 选项会显示仓库中的所有分支。**

- **--author=Surma**
  
  - git log --author="Paul Lewis",**引号很重要**！
  
- **--grep=bug**
  -  💡 grep 的更多说明 💡
  -  Grep 是一个模式匹配工具，它不在本课程教学范围内。但是简单介绍一下，如果你运行 git log --grep "fort"，那么 Git 将显示顺序包含字符 f、o、r、t 的 commit 。

### git show **显示最近的单个commit**

- SHA 作为最后一个参数提供给命令
- 与`git log SHA`区别是仅显示一条，git log 显示的是SHA之前的所有commit信息
- git show 可以与我们了解过的大部分其他选项一起使用：
  - --stat 显示更改了多少文件，以及添加/删除的行数
  - -p 或 --patch 显示默认补丁信息，但是如果使用了 --stat，将不显示补丁信息，因此传入 -p 以再次添加该信息
  - -w 忽略空格变化

### 





# [[Github]]

## 一个好的GitHub个人档案所需要具备的元素有：

- 个人肖像 （推荐使用）；
- 个人信息：让GitHub社区的使用者更好地了解你；
- 贡献的代码库：个人的成果展示；
- 贡献图表：展示在GitHub社区的活跃度。

## README

### Table of Contents / 内容列表

### Getting Stared / Installation

- 初始配置方面提供帮助
- 必要时引入示例代码
- 不要对用户所应具备的知识做任何假设，以初次接触项目的视角重新检视

### Known Bugs / 已知BUG

### Frequently asked questions / 常见问题

### Contributing

- 简述操作步骤
- 代码风格
- 其他希望贡献者了解的信息

 ### Copyright / License

[ 选择合适的License](https://choosealicense.com/)

***随着代码的增长去更新文档***



## [[提交说明]]

1. 第一行是主题。它应该简要说明更改的内容。不建议使用“修复”或“执行了某些操作”这类字眼，主题需要清楚、信息丰富，并努力避免使用亵渎语言。主题不应超过 50 个字符，首字母大写，不以句点结尾。在优达学城，如果提交的类型是错误修复、功能、文档更改等，我们还会包含有关提交类型的简短注释。

2. 接下来是正文，更详细地描述你进行更改的原因。正文每行通常包含 72 个字符左右。这是为了确保在命令行上使用 git 时，提交说明能够在终端窗口中显示出来。你还需要确保主题和正文之间有一个空行。当需要创建列表时，也可以使用星号或短横线添加项目符号。

- 有些提交的说明中不需要正文。例如，如果更正一个拼写错误，可以只有一个主题行。

- 还可以包含脚注，这通常用于指示此提交解决哪些问题或错误。

更详实的示例类似下面这样：

```markdown
功能：用 50 个或更少字符总结改动

如有必要，提供更详细的阐释文本。将其限制在 72 个字符左右。有些情况下，第一行被视为提交的主题，其余文本被视为正文。将摘要与正文分隔开的空行至关重要（除非你完全省略正文）；如果不分隔摘要和正文，“log”、“shortlog”和“rebase”等工具可能不知所措。

解释此次提交要解决的问题。重点是为什么要进行此次改动而不是如何改动（代码会说明这一点）。此次改动是否有任何副作用或其他不太直观的后果？可在此处解释这些方面。

空行后面可以有更多段落。

 - 还可以使用项目符号分项列出

 - 通常使用连字符或星号作为项目符号，前面有一个空格，中间是一些空行，但惯例不尽相同

如果你使用问题跟踪器，请在底部提供对问题的引用，
例如：

解决：# 123
另请参见：# 456、# 789
```

- 这也有例外。如果你从事的是一个开源项目，请务必遵守该项目的说明格式。这会让维护者的工作更轻松，并增加你的合并请求被接受的机会。





