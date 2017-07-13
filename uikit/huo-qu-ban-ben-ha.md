# 获取版本号

> iOS工程的基本信息都在info.plist文件中，版本号也是如此。这玩意存储的是一个NSDictionary类型。如果你打开info.plist文件你会看到`Bundle versions string, short`
>
> 这玩意就是App的版本号，所以获取App的版本号关键步骤就是读取info.plist文件，然后从NSDictionary中根据key找到版本号：CFBundleShortVersionString



```
//获取App版本号方法1
    NSString *path = [[NSBundle mainBundle] pathForResource:@"Info" ofType:@"plist"];
    NSDictionary *dic = [NSDictionary dictionaryWithContentsOfFile:path];
    NSLog(@"版本号:%@",dic[@"CFBundleShortVersionString"]);
    
    //获取App版本号方法2
    NSDictionary *infoplistDic = [NSBundle mainBundle].infoDictionary;
    NSLog(@"版本号:%@",infoplistDic[@"CFBundleShortVersionString"]);
}
```



