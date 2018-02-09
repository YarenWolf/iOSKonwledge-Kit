## 一、创建与合并分支

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

## 二、解决冲突

* 新建 feature1 分支，在上面做修改然后 add-&gt; commit。 然后切换到 master 分支，然后在 master 分支上修改然后 add -&gt; commit 。最后整个情况就是如下图所示，此时合并会到造成自动合并失败，产生冲突，需要手动合并。

![](/assets/0-6.png)

* 查看分支合并图

```
git log --graph
```



## 三、合并

合并分支的时候加上 --no-ff 参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而 `fast foward` 合并就看不出来曾经做过的合并

```
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt |    1 +
 1 file changed, 1 insertion(+)
```

## 四、分支策略

* 首先 master 分支上应该是非常稳定的本本，也就是用来发布或者上线的版本，平时不能在上面干活
* 那在哪干活？干活都在 dev 分支上，也就是说 dev 分支是不稳定的，到某个时候，比如发 1.0 版本的时候，再把 dev 分支 合并到 master 分支。将用 master 分支发布 1.0 版本。
* 团队中的开发者每个人应该有自己的分支，时不时滴往 dev 分支上合并就可以了。

![](/assets/0-7.png)





































