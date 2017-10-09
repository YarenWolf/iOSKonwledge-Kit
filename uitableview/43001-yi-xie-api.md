一些api

1、**@property\(nonatomic,readonly,nullable\)NSIndexPath\*indexPathForSelectedRow; // returns nil or index path representing section and row of selection.返回当前tableView被选中行的indexPath**

```
[self.tableView indexPathForSelectedRow];
```



