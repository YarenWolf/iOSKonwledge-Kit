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

# 引用计数器

* 字符串存储在常量区，不允许被回收因此引用计数器是很大的值

* 发送retain和release不影响引用计数器

```
NSString *str = @"杭城小刘";
    NSLog(@"str-%lu",str.retainCount);       //18446744073709551615
    [str retain];
    NSLog(@"str-%lu",str.retainCount);       //18446744073709551615

    [str release];
    NSLog(@"str-%lu",str.retainCount);       //18446744073709551615
```

* 若字符串存储在堆区，对象的初始引用计数器为1，retain和release对引用计数器有影响
* 字符串对象是浅拷贝，对象的引用计数器加1
* 字符串对象是深拷贝，字符串对象的引用计数器不变，新拷贝出来的对象的引用计数器为1



# Copy

* copy方法的确声明在NSObject类中，但是copy方法调用的内部是调用了copyWithZone方法
* 这个方法声明在NSCopying协议中，因为我们的类没有遵循NSCopying协议，所以我们的类就没有copyWithZone方法
* 如果想让我们自己的类具有对象拷贝的能力，那么就让我们的类遵循NSCopying协议，并实现copyWithZone方法

```
-(id)copyWithZone:(NSZone *)zone{
    //1、深拷贝
    Person *p = [Person new];
    p.name = _name;
    p.age = _age;
    return p;
    
    
    //2、浅拷贝
    //return self;
    
    
}
```



