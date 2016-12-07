###UISegmentedControl的使用

以前傻傻的用2个Button去实现这个效果。哎，天真的一笔。
```
@property (nonatomic, strong) UISegmentedControl *segment;

-(UISegmentedControl *)segment{
    if (!_segment) {
        _segment = [[UISegmentedControl alloc] initWithItems:@[@"近日随访任务",@"所有随访任务"]];
          
        _segment.frame = CGRectMake(45, 15, BoundWidth-90, 40);
        _segment.tintColor = [UIColor colorWithHexString:@"56abe4"];
        [_segment addTarget:self action:@selector(segmentClick:) forControlEvents:UIControlEventValueChanged];
        _segment.selectedSegmentIndex = 0;
    }
    return _segment;
}


-(void)segmentClick:(UISegmentedControl *)segment{
    if (segment.selectedSegmentIndex == 0) {
        [self clickRecentTask];
    }else{
        [self clickAllTask];
    }
}

```
