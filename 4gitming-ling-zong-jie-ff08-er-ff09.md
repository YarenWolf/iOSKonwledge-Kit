# 创建与合并分支



* 每次提交 Git 都把它们串成一条时间线，这条时间线是一个分支，主分支叫做 master 分支，HEAD 指向 master 分支， master 指向提交的，所以 HEAD 就是当前分支

![](/assets/0.png)

* 当我们创建分支的时候，Git 新建了一个指针叫做 dev。 指向 master 相同的提交，再把 HEAD 指向 dev，就表示当前分支再 dev上

![](/assets/0-2.png)

* 从现在开始，对工作区的修改和提交都是针对 dev 分支，比如一次新的提交， dev 指针向前移动一步，而 master 指针不变

![](/assets/0-3.png)

* 如果 dev 工作完成，就可以把 dev 合并到 master 上，直接把 master 指向 dev 的当前提交。

![](/assets/0-4.png)

* 创建分支然后切换到相应的分支

```
git branch dev
git checkout dev
// git checkout -b dev(创建并切换)
```

* 在当前 master 分支，将 dev 分支的内容合并在一起

```
$ git merge dev
Updating 170a795..1126c69
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

注意 Fast-forward 是 快进模式的意思，也就是直接把 master 指向 dev 的当前提交。

* 删除分支

```
$ git branch -d dev
Deleted branch dev (was 1126c69).
```









































