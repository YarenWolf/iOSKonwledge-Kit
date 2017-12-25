# UITableView 删除、增加功能

> 使用UITableView的时候经常需要增加一行、删除一行的功能，下面就记录一下初次使用的一些api

```
//删除UITableview的Section



    NSIndexSet *indexSet = [NSIndexSet indexSetWithIndex:5];
    [self.tableview beginUpdates];
    [self.tableview deleteSections:indexSet withRowAnimation:UITableViewRowAnimationTop];
    self.liveTimeLabel.text = @"请选择直播时间";
    [self.tableview endUpdates];
    
    
//增加Section

    NSIndexSet *indexSet = [NSIndexSet indexSetWithIndex:5];
    [self.tableview beginUpdates];
    [self.tableview insertSections:indexSet withRowAnimation:UITableViewRowAnimationTop];
    [self.tableview endUpdates];
```



