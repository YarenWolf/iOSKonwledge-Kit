```
//1、根据日期得到字符串
    NSDate *date = [NSDate date];
    NSDateFormatter *formatter = [NSDateFormatter new];
    formatter.dateFormat = @"YYYY-MM-dd HH:mm:ss";
    NSString *time = [formatter stringFromDate:date];
    
    
    //2、根据字符串得到日期
    NSString *timeStr = @"2019-12-21 12:23:32";
    NSDate *future = [formatter dateFromString:timeStr];
    NSLog(@"未来时间:%@",future);
    
    //3、计算2个日期的间隔
    NSDate *beginDate = [NSDate date];
    int var = 0;
    for(int i =0;i<100000000;i++){
        var++;
    }
    
    NSDate *endDate = [NSDate date];
    NSTimeInterval second = [endDate timeIntervalSinceDate:beginDate];
    NSLog(@"经历了%f秒",second);
    
    
    //4、得到年月日号数等信息
    NSDate *current = [NSDate date];
    NSCalendar *caledner = [NSCalendar currentCalendar];
    NSInteger year = [caledner component:NSCalendarUnitYear fromDate:current];
    NSInteger month = [caledner component:NSCalendarUnitMonth fromDate:current];
    NSInteger day = [caledner component:NSCalendarUnitDay fromDate:current];
    NSLog(@"年：%ld,月：%ld，日:%ld",year,month,day);
```



