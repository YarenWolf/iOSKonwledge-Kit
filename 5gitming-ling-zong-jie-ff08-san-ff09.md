* 当手头的工作没有完成的时候，先把工作现场 git stash 一下，然后去修复 bug， 修复后，再 git stash pop ,回到工作现场
* git stash list 查看保存的现场，可以用 git stash apply stash@{0}，恢复后 stash 的内容不会删除，所以还需要 git stash drop 删除。
* 另一种方式是 git stash pop ，恢复的同时删除了 stash 的内容



