###tableview设置线左对齐
```
//设置cell的横线左对齐
-(void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath{
 cell.separatorInset=UIEdgeInsetsZero;
 cell.layoutMargins=UIEdgeInsetsZero;
 cell.preservesSuperviewLayoutMargins=NO;
}
```
 ###tableView滚动设置改变背景颜色
 ```
-(void)scrollViewDidScroll:(UIScrollView *)scrollView

{
 CGPoint translation = scrollView.contentOffset;
 if (translation.y< 0) {
 self.tableView.backgroundColor = NavigationBarColor;
 }else if(translation.y> 0){
 self.tableView.backgroundColor = MainViewBackgroundColor;
 }
}
```

###性能优化


