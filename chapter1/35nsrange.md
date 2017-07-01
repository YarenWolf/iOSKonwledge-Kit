# NSRanege

* 创建NSRange的3种方式

```
    NSRange range =  NSMakeRange(0, 10);
    NSRange range2 = {0,2};
    NSRange range3 = {.location=2,.length=5};
    
    
    NSString *Str = @"哈哈哈热敷热敷个啃骨头和文化危机";
    NSLog(@"%@",[ Str substringWithRange:range]);
    NSLog(@"%@",[ Str substringWithRange:range2]);
    NSLog(@"%@",[ Str substringWithRange:range3]);
```



