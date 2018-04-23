# Git 学习

## 版本控制

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

### 集中式版本控制系统（Centralized Version Control System）

有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。
**缺点**：1.必须联网才能工作 2.不安全

### 分布式版本控制系统（Distributed Version Control System）

没有“中央服务器”的概念（但通常有一个充当“中央服务器”的电脑用来方便交换修改）。客户端并不是只提取最新版本的文件快照，而是把代码仓库完整地镜像下来。因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。
**优点**：1.不用联网，本地拥有完整的版本库 2.安全性高

## 基本概念

### 直接记录快照

  ![快照流](D:\blog\source\_posts\git\snapshots.png)

* 在每次提交更新，或在 Git 中保存项目状态时，Git 主要对当时的全部文件只做一个快照并保存这个快照的索引。若文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。Git 对待数据更像是一个 **快照流**。
* 存储每个文件与初始版本的差异（如上图）
* 存储项目随时间改变的快照
* 几乎所有操作都是本地执行
* Git 保证完整性
* Git 一般只添加数据

### 三个工作区域

  ![工作区域](D:\blog\source\_posts\git\areas.png)

* 工作目录（Working Directory）
    对项目的某个版本独立提取出来的内容，这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。
* 暂存区域（Staging Area）
    暂存区（也叫索引"Index"）是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。
* Git 仓库（Repository）
    仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。从其它计算机克隆仓库时，拷贝的就是这里的数据。

### Git 的三种状态

* 已修改（Modified）
    修改了文件，但还没保存到数据库中。
* 已暂存（Staged）
    表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
* 已提交（Committed）
    表示数据已经安全的保存在本地数据库中。

## 常用命令

### 获取 Git 仓库

* `$ git init`
    在当前项目（文件夹）内初始化 Git 仓库
* `$ git init [directory]`
    在新建的 directory 文件夹内初始化 Git 仓库
* `git clone https://github.com/minejoy/git_basic.git`
     使用 HTTPS 协议从 GitHub 克隆一个仓库
* `git clone git@github.com:MineJoy/git_basic.git`
     使用 SSH（速度更快，若只开放http端口不能使用）
* `git clone git@github.com:MineJoy/git_basic.git local_repo_name`
     自定义本地仓库的名字为 local_repo_name（远程仓库名 git_basic）

### Make changes

* `$ git add [file1] [file2] ...`
    将指定文件添加到暂存区（跟踪）
* `$ git add [dir]`
    将指定文件夹添加到暂存区，包括子目录
* `$ git add .`
    跟踪当前目录的所有文件
---

* `$ git rm [file]`
    Deletes the file from the working directory and stages the deletion
* `$ git rm --cached [file]`
    Remove the file from version control but preserves the file locally
* `$ git mv [file-original] [file-renamed]`
    Changes the file name and prepares it for commit
* `$ git mv [existing-path] [new-path]`
    Changes an existing file path and stage the move
---

* `$ git diff`
    Shows unstaged changes between your index and working directory
* `$ git diff HEAD`
    Shows difference between working directory and last commit
* `$ git diff --staged`
    Shows difference between staging and the last commit
* `$ git diff --cached`
    Shows difference between staged changes and last commit
---

* `$ git reset`
    Reset staging area to match most recent commit, but leave the working directory unchanged
* `$ git reset --hard`
    Reset staging area and working directory to match most recent commit and **overwrite all changes** in the working directory
* `$ git reset [commit]`
    Move the current branch tip backward to [commit], reset the staging area to match, **but leave the working directory alone**
* `$ git reset --hard [commit]`
    Same as previous, but resets both the staging area & working directory to match. **Deletes** uncommitted changes, and **all commits after [commit]**
* `$ git reset --hard [HEAD^/HEAD^^/HEAD~100]`
    back
* `$ git reset file`
    Remove file from the staging area, but leave the working directory unchanged. This unstages a file without overwriting any changes
* `$ git reset HEAD file`
    把暂存区的修改撤销（unstage），重新放回工作区。再用 `$ git checkout --file` 丢弃工作区的修改。（针对写错了东西add后想要删除的方法）

### Log 相关

* `$ git lg`
    Mine config
* `$ git log`
    Lists version history for the current branch
* `$ git reflog`
    Show a log of changes to the local repository's HEAD. Add `--relative-date` flag to show date info or `--all` to show all refs.
* `$ git log --follow [file]`
    Lists version history for a file, including renames
* `$ git log branchB..branchA`
    show the commits on branchA that arenot on branchB
* `$ git last`
    List the newest commit(Mine config)
* `$ git log -<limit>`
    E.g. `$ git log -5` will limit to 5 commits
* `$ git log --author="<pattern>"`
    Search for commits by a particular author

### Branch 相关

* `$ git branch`
    Lists all local branches in the current repository
* `$ git branch [branch-name]`
    Creates a new branch
* `$ git checkout [branch-name]`
    Switches to the specified branch and updates the working directory
* `$ git branch -b [branch-name]`
    Create and **check out** a new branch named branch-name
* `$ git merge [branch]`
    Combines the specified branch's history into the current branch
* `$ git merge --no-ff -m "description" [branch-name]`
    Merge without fast-forward (will have a new commit)
* `$ git branch -d [branch-name]`
    Deletes teh specified branch
* `$ git diff [first-branch]...[second-branch]`
    Shows content difference between two branches

### Stash 相关

* `$ git stash`
    Temporarily stores all modified tracked files
* `$ git stash list`
    Lists all stashed changesets(变更)
* `$ git stash pop`
    Restores the most recently stashed files(会删除stash内容)
* `$ git stash drop`
    Discards the most recently stashed chagneset(搭配`$ git stash apply`，这个命令不会删除stash内容，区别`$ git stash pop`)

### 打标签

* [git-scm](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE)
* [廖雪峰git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013762144381812a168659b3dd4610b4229d81de5056cc000)

### 远程仓库

* `$ git remote -v`
    List all currently configured remotes
* `$ git remote show <remote>`
    Show information about a remote
* `$ git remote add <shortname> <url>`
    Add new remote respository (git remote add origin git@github.com:minejoy/git_basic.git)
* `$ git push <remote> <branch>`
    Publish local changes on a remote.(第一次推送最好加 -u: `$ git push -u origin master`, 此后的本地提交：`$ git push origin master`)
* `$ git fetch <remote>`
    Download all changes from remote, but don't integrate into HEAD
* `$ git pull`
    Downloads bookmark history and incorporates changes [参考](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013760174128707b935b0be6fc4fc6ace66c4f15618f8d000)

### Else

* `$ pwd`
    当前路径
* `$ cd dir`
    进入dir文件夹（`$ cd ../` 切换到上一级）
* `$ ls`
    列出当前目录下的所有文件
* `$ mkdir directory`
    创建一个 directory 文件夹
* `$ touch file`
    创建一个空文件（可不加文件名或后缀名）
* `$ cat file`
    显示file文件内容
* `$ rm file`
    删除文件
* `$ code file`
    用 VScode 打开文件（配置的）
* `$ reset`
    清空控制台

## 推荐资料

[Pro Git](https://git-scm.com/book/zh/v2)
[Using Git](https://help.github.com/categories/using-git/)
[廖雪峰Git教程](https://git-scm.com/book/zh/v2)