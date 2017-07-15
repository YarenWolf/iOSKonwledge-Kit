# 单例模式

> 1个类的对象，如轮在何时何地创建，也无论创建多少次，创建出来的都是同一个对象
>
> 创建的时候，最终都调用alloc方法来创建对象

* 在alloc方法的内部什么也没做，只是调用allocWithZone方法
* 真正创建对象就是在allocWithZone方法中做的

做个小实验

```
+(instancetype)allocWithZone:(struct _NSZone *)zone{
    return nil;
}

Person *p1 = [Person new];
Person *p2 = [Person new];
Person *p3 = [Person new];
```

![](/assets/4046AF17-101E-4828-A3E2-BC51D3B58E40.png)

单例模式的写法规范

* shared类名；default类名

```
-(Person *)sharedInstance{
    static Person *instance = nil;
    if (instance == nil) {
        instance = [[Person alloc] init];
    }
    return instance;
}

+(Person *)sharedInstance{
    static Person *instance = nil;
    if (instance == nil) {
        instance = [[Person alloc] init];
    }
    return instance;
}
```

# 类似UIApplication一样创建单例



