# Copy

* NSString的copy是浅拷贝（没有产生新对象，而是直接返回地址）

```
NSString *str = @"杭城小刘";
    NSString *str2 =  [str copy];

    NSLog(@"str-%@",str);       //杭城小刘
    NSLog(@"str-%p",str);       //str-0x100002030
    NSLog(@"str2-%@",str2);     //杭城小刘
    NSLog(@"str-%p",str2);       //str-0x100002030
```

* NSMutableString的copy是深拷贝（产生新的对象）
* 拷贝出的对象是NSString

```
    NSMutableString *str = [NSMutableString stringWithFormat:@"杭城小刘"];
    NSMutableString *str2 =  [str copy];

    NSLog(@"str-%@",str);       //杭城小刘
    NSLog(@"str-%p",str);       //str-0x100403230
    NSLog(@"str2-%@",str2);     //杭城小刘
    NSLog(@"str-%p",str2);       //str-0x100403270

    [str2 appendString:@"hha"];     //Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: 'Attempt to mutate immutable object with appendString:'
```

# 

# 

# MutableCopy

* NSString的NSMutableCopy过后是深拷贝，且产生的是NSMutableString

```
NSString *str = @"杭城小刘";
    NSMutableString *str2 =  [str mutableCopy];
    [str2 appendString:@"OC"];
    NSLog(@"str-%@",str);       //杭城小刘
    NSLog(@"str-%p",str);       //str-0x100002030
    NSLog(@"str2-%@",str2);     //杭城小刘OC
    NSLog(@"str-%p",str2);       //str-0x1003069b0
```

* NSMutableString的NSMutableCopy过后是深拷贝，且产生的是NSMutableString

```
 NSMutableString *str = [NSMutableString stringWithFormat:@"杭城小刘"];
    NSMutableString *str2 =  [str mutableCopy];
    [str2 appendString:@"OC"];
    NSLog(@"str-%@",str);       //杭城小刘
    NSLog(@"str-%p",str);       //str-0x100306920
    NSLog(@"str2-%@",str2);     //杭城小刘OC
    NSLog(@"str-%p",str2);       //str-0x100300220
```



