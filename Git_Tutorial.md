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

   `git add .`可以add当前文件夹内的所有文件。
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
  > 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到**暂存区**；
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

- 场景2：当你不但改乱了工作区某个文件的内容，还**添加到了暂存区**时，想**丢弃修改**，分两步，第一步用命令`git reset HEAD <file>`，就**回到了场景1，第二步按场景1操作**。

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

## 4. 远程仓库

`GitHub`提供了Git仓库托管服务。由于本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以需要将**本地的公钥**添加到GitHub的`SSH Key`中。

```
ssh-keygen -t rsa -C "youremail@example.com"
```

本机生成的文件路径：`C:\Users\11198;\.ssh`

### 4.1 添加远程库

要**关联一个远程库（GitHub中已经存在一个版本库，要与本地的版本库进行关联）**，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

### 4.2 从远程库克隆

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

`git clone git@server-name:path/repo-name.git``

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。

## 5. 分支管理

### 5.1 创建与合并分支

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <branch_name>`

切换分支：`git checkout <branch_name>`

创建+切换分支：`git checkout -b <branch_name>`

合并某分支到当前分支：`git merge <branch_name>`，**只改变当前分支**。

删除分支：`git branch -d <branch_name>`

### 5.2 解决冲突

如果不同的分支各自都**分别有了新的提交(commit)**，这种情况下，如果要**进行merge**，Git无法执行“快速合并”，只能试图把各自的修改合并起来，如果**修改了同一部分**，就会引起**冲突(conflict)**。

冲突(conflict)举例:

```

<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick and simple.
```

再提交：

```
$ git add readme.txt 
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```

用带参数的`git log`也可以看到分支的合并情况：

```
$ git log --graph --pretty=oneline --abbrev-commit
```

### 5.3 分支管理策略

通常，合并分支时，Git会用`Fast forward`模式，但是在这种模式下，删除分支后会丢掉分支信息。如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```
git merge --no-ff -m "merge with no-ff" dev
```

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

### 5.4 Bug分支

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下（会将工作区中修改保留到`stash`中），然后去修复bug，修复后，再`git stash pop`，回到工作现场。

### 5.4 Feature分支

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

### 5.5 多人协作

**推送分支**

```
git push origin master
git push origin dev
```

**抓取分支**

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

当你的小伙伴**从远程库clone**时，默认情况下，你的小伙伴**只能看到本地的`master`分支**。

```
$ git branch
* master
```

现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

```
$ git checkout -b dev origin/dev
```

现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程

因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；

2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；

3. 如果合并有冲突，则解决冲突，并在本地提交；

4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

### 5.6 git fetch和git pull的区别

[https://www.jianshu.com/p/a5c4d2f99807](https://www.jianshu.com/p/a5c4d2f99807)

git fetch从远程分支拉取代码。

fetch常结合merge一起用，`git fetch + git merge == git pull`
一般要用git fetch+git merge，因为`git pull`会将代码直接**合并**，**造成冲突**等无法知道，fetch代码下来要`git diff orgin/xx`来**看一下差异然后再合并**。

**用法：**

- **git fetch**

- **git fetch origin**

- **git fetch origin branch1**

- **git fetch origin branch1:branch2**

- **git fetch origin :branch2**

## 6. 标签管理

发布一个版本时，我们通常先在**版本库**中打一个标签（tag），这样，就**唯一确定了打标签时刻的版本**。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，**标签也是版本库的一个快照**。

Git的**标签**虽然是版本库的快照，但**其实它就是指向某个commit的指针**（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，**创建和删除标签都是瞬间完成的**。

### 6.1 创建标签

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id`git tag <tagname> commit_id` ；

- 命令`git tag -a <tagname> -m "blablabla..." commit_id`可以指定标签信息；

- 命令`git tag`可以查看所有标签。

### 6.2 操作标签

- 命令`git push origin <tagname>`可以推送一个本地标签；

- 命令`git push origin --tags`可以推送全部未推送过的本地标签；

- 命令`git tag -d <tagname>`可以删除一个本地标签；

- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

## 7. 自定义Git

### 7.1 忽略特殊文件

**在Git工作区的根目录下**创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

在`.gitignore`文件中添加要忽略的文件类型和文件夹

```
*.pyc    # 忽略pyc类型的文件

build    # 忽略build文件夹
```

忽略文件的原则是：

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

### 7.2 配置别名和配置文件

**配置别名**

把`commit`简写为`ci`

```
git config --global alias.ci commit
```

提交就可以简写成：

```
$ git ci -m "bala bala bala..."
```

**配置文件**

配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的Git配置文件都放在`.git/config`文件中

而当前用户的Git配置文件放在**用户主目录下(C:\Users\11198;\)**的一个隐藏文件`.gitconfig`中
