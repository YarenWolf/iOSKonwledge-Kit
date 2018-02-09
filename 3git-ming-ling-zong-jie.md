# 常用命令

* cat 可以看到文件的全部内容。
* git add 文件名 将文件从工作区提交到缓存区
* git commit -m '\*\*\*' 将文件从缓存区提交到仓库
* git status 可以查看文件的状态
* git diff 文件名 查看文件的修改内容
* git log 查看历史记录，如果需要看起来更加方便， 使用 git log --pretty=oneline
* git 必须知道当前版本是哪个版本。在 git 中用 HEAD 表示当前版本，上一个版本就是 HEAD^ ，上上个版本就是 HEAD^^，当前往上100个版本写100个 ^  不容易，所以写成 HEAD~100
* 版本回退用 git reset --hard 版本号。git reset --hard HEAD^。其实本质上就是将指向当前版本号的指针指向了当前位置
* git reflog 可以查看命令历史。
* 工作区（working directory）：就是电脑上可以看到的目录。
* 版本库（Repository）： 工作区有一个隐藏目录 .git ，这个不是工作区而是 git 的版本库。 git 的版本库里面存了很多东西，其中最重要的就是 stage （或者叫 index） 的暂存区。还有 git 为我们创建的第一个分支 master ，以及指向 master 的一个指针叫 HEAD。
* 把文件往 git 版本库添加的时候分两步进行。第一步是 git add 把文件添加进去，实际上就是把文件修改添加到暂存区。第二步是 git commit 提交更改，实际上就是把暂存区的所有内容提交到当前分支。
* git diff HEAD -- 文件名 可以查看工作区和版本库里面最新版本的区别
* 撤销更改：git 会告诉你 git checkout -- file 可以丢弃工作区的更改。（让这个文件回到最后一次 git commit 或 git add 时的状态）

  ```
  $ git status
  # On branch master
  # Changes not staged for commit:
  #   (use "git add <file>..." to update what will be committed)
  #   (use "git checkout -- <file>..." to discard changes in working directory)
  #
  #       modified:   readme.txt
  #
  no changes added to commit (use "git add" and/or "git commit -a")
  ```

* 撤销更改：假如你的更改已经提价到暂存区，但是还没有 commit。git 告诉我们， git reset HEAD 文件名 ，可以把暂存区的修改撤销掉（unstage）。重新放回工作区

  ```
  $ git status
  # On branch master
  # Changes to be committed:
  #   (use "git reset HEAD <file>..." to unstage)
  #
  #       modified:   readme.txt
  #
  ```

* 删除文件：普通删除文件用 rm 文件名。 git 删除文件用 git rm 文件名。并且 git commit: 。另一种情况是删错了。因为版本库里面还有文件，都可以轻松还原（把误删的文件恢复到最新版本）

  ```
  git checkout -- 文件名
  ```

* git checkout 其实是用版本库里面的版本替换工作区的版本，无论工作区是修改还是删除都可以“一键还原”





