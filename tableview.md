###tableview设置线左对齐
```
//设置cell的横线左对齐
-(void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath{
 cell.separatorInset=UIEdgeInsetsZero;
 cell.layoutMargins=UIEdgeInsetsZero;
 cell.preservesSuperviewLayoutMargins=NO;
}
```