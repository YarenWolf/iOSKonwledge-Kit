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

说明

1. 如果想实现深拷贝，那么就重新创建1个对象，并将对象的属性值复制
2. 如果想实现浅拷贝，那么就返回原来对象的地址
3. NSString对象用copy修饰

```
 //首先声明的str指针指向常量区的字符串地址，然后给p1指针指向的person对象的name属性赋值的时候会将name的地址赋值给p1的name指针
    
    Person *p1 = [Person new];
    NSString *name = @"杭城小刘";
    p1.name = name;
    //如果给name指针重新设置字符串内容，因为是在常量区，所以会得到一个新的字符串地址，所以赋值操作不会影响person对象的name属性
    name = @"iOS开发者";
    
    NSLog(@"p1.name->%@",p1.name);
```

结果

![](/assets/QQ20170703-093431@2x.png)![](/assets/QQ20170703-093504@2x.png)

![](/assets/QQ20170703-094341@2x.png)![](/assets/QQ20170703-094416@2x.png)

* 如果是NSString的话是没问题的，给对象设置了字符串之后会指向该字符串的地址，如果该字符串重新赋值，那么会得到一个新的字符串地址，所以新得到的字符串对象不会影响之前的的字符串的值
* 如果是NSMutableString的话，如果新的值改变了，那么不会得到一个新的字符串对象，所以用strong修饰的话会影响之前的值
* 所以对于NSMutableString，必须用copy，setter的时候必须拷贝一个新的，这样子即使值改变了，不影响之前的属性值





