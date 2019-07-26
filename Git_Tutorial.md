# Git Tutorial

## 1. 安装Git

[https://git-scm.com/downloads](https://git-scm.com/downloads)

安装完成后，点击`Git Bash`打开。同时设置用户名和邮箱。

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

**注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。**

## 2. 创建版本库

初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m <message>`，完成。

## 3. 时光机穿梭

- 要随时掌握工作区的状态，使用`git status`命令。

- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

### 3.1 版本回退

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

  > 首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

### 3.2 工作区和暂存区

- 工作区(Working Directory)

- 版本库（Repository)

  工作区的隐藏目录`.git`，这个是Git的版本库。

  Git的版本库中有很多东西，做重要的就是称为`stage`的**暂存区**，还有Git为我们自动创建的第一个分支`master`。

  > 前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
  > 
  > 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
  > 
  > 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。
  > 
  > 因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。
  > 
  > 你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

### 3.3 管理修改

Git跟踪并管理的是修改，而非文件。只有将添加到暂存库，commit之后才能更新。

### 3.4 撤销修改

- 场景1：当你改乱了工作区某个文件的内容，想**直接丢弃工作区的修改**时，用命令`git checkout -- file`。

- 场景2：当你不但改乱了工作区某个文件的内容，还**添加到了暂存区**时，想**丢弃修改**，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

- 场景3：已经**提交**了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是**没有推送到远程库。**

### 3.5 删除文件

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

```
$ rm test.txt
```

这个时候，Git知道你删除了文件，因此，**工作区和版本库就不一致了**，`git status`命令会立刻告诉你哪些文件被删除了。

现在你有**两个选择**，**一是确实要从版本库中删除该文件**，那就用命令`git rm`删掉，并且`git commit`：

```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

现在，文件就从版本库中被删除了。

另一种情况是**删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本**：

```
$ git checkout -- test.txt
```

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
