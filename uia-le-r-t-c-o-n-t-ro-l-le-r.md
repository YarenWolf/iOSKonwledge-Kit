###改变颜色


```
UIAlertController *alert = [UIAlertController alertControllerWithTitle:@"警告!" message:result preferredStyle:UIAlertControllerStyleAlert];
    NSMutableAttributedString *hogan = [[NSMutableAttributedString alloc] initWithString:@"警告!"];
    [hogan addAttribute:NSFontAttributeName value:[UIFont systemFontOfSize:18] range:NSMakeRange(0, [[hogan string] length])];
    [hogan addAttribute:NSForegroundColorAttributeName value:[UIColor redColor] range:NSMakeRange(0, [[hogan string] length])];
    [alert setValue:hogan forKey:@"attributedTitle"];
    
    
    UIAlertAction *ok = [UIAlertAction actionWithTitle:@"自爆模式" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
        
    }];
    [ok setValue:[UIColor colorWithRed:15/255.0 green:183/255.0 blue:227/255.0 alpha:1] forKey:@"_titleTextColor"];
    
    UIAlertAction *cancel = [UIAlertAction actionWithTitle:@"自动报警" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
        
    }];
    [cancel setValue:[UIColor colorWithRed:15/255.0 green:183/255.0 blue:227/255.0 alpha:1] forKey:@"_titleTextColor"];
    [alert addAction:ok];
    [alert addAction:cancel];
    [self presentViewController:alert animated:YES completion:nil];
```


