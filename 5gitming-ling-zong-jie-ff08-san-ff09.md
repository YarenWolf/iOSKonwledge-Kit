* 当手头的工作没有完成的时候，先把工作现场 git stash 一下，然后去修复 bug， 修复后，再 git stash pop ,回到工作现场
* git stash list 查看保存的现场，可以用 git stash apply stash@{0}，恢复后 stash 的内容不会删除，所以还需要 git stash drop 删除。
* 另一种方式是 git stash pop ，恢复的同时删除了 stash 的内容





# 一、删除远端分支

```
hangcheliudeMBP:pendi geek$ git branch -a
  daief
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/daief
  remotes/origin/master
  remotes/origin/pad_up_only
hangcheliudeMBP:pendi geek$ git branch -d daief
Deleted branch daief (was 18c3110).
hangcheliudeMBP:pendi geek$ git push origin :daief
To heletech.cn:august/pendi.git
 - [deleted]         daief
hangcheliudeMBP:pendi geek$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/pad_up_only
```



