# NSValue

> CGRect CGPoint CGSize 等结构体是无法直接存储在集合中（NSArray、NSDictionary等），所以先用NSValue包装结构体才可以存储到集合中



```
  CGPoint p1 = CGPointMake(1, 2);
    CGPoint p2 = CGPointMake(1, 2);
    CGPoint p3 = CGPointMake(1, 2);
    NSValue *value1 = [NSValue valueWithPoint:p1];
    NSValue *value2 = [NSValue valueWithPoint:p2];
    NSValue *value3 = [NSValue valueWithPoint:p3];
    NSArray *array = @[value1,value2,value3];
    for (int i=0; i<array.count; i++) {
        NSValue *value = array[i];
        NSLog(@"CGPoint ->%@",NSStringFromPoint(value.pointValue));
    }
```



