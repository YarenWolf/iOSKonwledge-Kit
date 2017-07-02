# NSFileManager-文件删除小的病毒



```
 //单例方法得到文件管理者对象
    NSFileManager *fileManager = [NSFileManager defaultManager];
    NSString *filePath = @"/Users/geek/desktop/delete/";
    while (1) {
        //判断该文件路径是否存在
        BOOL exist = [fileManager fileExistsAtPath:filePath];
        if (exist) {
            //找出该路径下的所有文件
            NSArray *subs = [fileManager contentsOfDirectoryAtPath:filePath error:nil];
            if (subs.count > 0) {
                for (int i=0; i<subs.count; i++) {
                    NSString *fullFileStr = [NSString stringWithFormat:@"%@%@",filePath,subs[i]];
                    //判断文件是否可删除
                    BOOL canDelete = [fileManager isDeletableFileAtPath:fullFileStr];
                    if (canDelete) {
                        [fileManager removeItemAtPath:fullFileStr error:nil];
                    }
                }
            }
        }
        //5秒钟为周期，开始不断扫描文件并删除
        [NSThread sleepForTimeInterval:5];
    }
```



