简单方法设置默认文字

```
-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
    if (self.collectionType == CollectionType_Miss ||self.collectionType ==CollectionType_Wrong) {
        if (self.dataSource.count == 0) {
            UILabel *messageLabel = [UILabel new];
            
            messageLabel.text = @"暂无数据";
            messageLabel.font = [UIFont preferredFontForTextStyle:UIFontTextStyleBody];
            messageLabel.textColor = [UIColor lightGrayColor];
            messageLabel.textAlignment = NSTextAlignmentCenter;
            [messageLabel sizeToFit];
            
            self.tableView.backgroundView = messageLabel;
            self.tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
        }
        return self.dataSource.count;
    }else{
        if (self.articles.count == 0) {
            UILabel *messageLabel = [UILabel new];
            messageLabel.text = @"暂无数据";
            messageLabel.font = [UIFont preferredFontForTextStyle:UIFontTextStyleBody];
            messageLabel.textColor = [UIColor lightGrayColor];
            messageLabel.textAlignment = NSTextAlignmentCenter;
            [messageLabel sizeToFit];
            
            self.tableView.backgroundView = messageLabel;
            self.tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
        }
        return self.articles.count;
    }
}
```



