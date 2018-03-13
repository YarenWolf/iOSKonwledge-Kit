# KVO底层实现

## 一、KVO的基本使用

* 给要观察的对象的属性添加观察
* 实现 -\(void\)observeValueForKeyPath:\(NSString \*\)keyPath ofObject:\(id\)object change:\(NSDictionary&lt;NSKeyValueChangeKey,id&gt; \*\)change context:\(void \*\)context 方法
* 触发属性的改变

```
1、 [self.p addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionOld| NSKeyValueObservingOptionNew context:nil];
2、
-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context{
    NSLog(@"%@",change);
}

3、
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    static int a;
    self.p.name = [NSString stringWithFormat:@"%zd",a++]s;
}
```

## 二、KVO 的一些小实验

1、KVO 对于观察的属性是自动发出通知的（观察者模式的一种。当被观察的对象的属性值发生改变，KVO会自动发送通知给观察者）。但是有些需求就是不需要自动通知。那么有个方法可以让开发者自己处理，对某个观察的属性**设置是否需要自动通知观察者**

`+(BOOL)automaticallyNotifiesObserversForKey:(NSString *)key`

但是当关闭掉对监听的属性的自动通知，那么就无法实现 KVO，所以你得在属性的值改变前后手动触发 KVO 也就是2个方法

`willChangeValueForKey:`和 `didChangeValueForKey:`

```
//Person.h
@property (nonatomic, strong) NSString *name;

//Person.m
+(BOOL)automaticallyNotifiesObserversForKey:(NSString *)key{
    if ([key isEqualToString:@"name"]) {
        return NO;
    }
    return YES;
}

//ViewController.m
[self.p addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionOld| NSKeyValueObservingOptionNew context:nil];

-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context{
    NSLog(@"%@",change);
}

-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    static int a;
    [self.p willChangeValueForKey:@"name"];
    self.p.name = [NSString stringWithFormat:@"%zd",a++];
    [self.p didChangeValueForKey:@"name"];
}

//console
(lldb) po change
{
    kind = 1;
    new = 0;
    old = "<null>";
}
```

2、KVO **除了对当前对象的属性进行赋值外，还可以对其更“深层”的对象进行赋值。**

例如对当前对象的 dog 属性的 age 属性进行赋值。KVC进行多级访问时，直接类似于属性调用一样用点语法进行访问即可。

```
[self.p addObserver:self forKeyPath:@"dog.age" options:NSKeyValueObservingOptionOld| NSKeyValueObservingOptionNew context:nil];
```

3、KVO 观察普通属性很常见，也就是基本数据类型。那么 KVO 观察集合数据类型有坑吗？？

举个例子

```
//1、为上文的 Person 类增加一个 array 属性
@property (nonatomic, strong) NSMutableArray *array;

//2、在初始化的时候为 array 赋值
-(instancetype)init{
    if (self = [super init]) {
        _dog = [[Dog alloc] init];
        _array = [NSMutableArray array];
    }
    return self;
}


//VC.m
//3、观察
[self.p addObserver:self forKeyPath:@"array" options:NSKeyValueObservingOptionOld| NSKeyValueObservingOptionNew context:nil];


//4、实现方法
-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context{
    NSLog(@"%@",change);
}

//5、触发值改变
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    static int a;
    [self.p.array addObject:@(a)];
}
```

结论：对于 NSMutableArray 集合类对象，不能直接观察。



4、KVO 观察多个属性值的改变。

肯定可以写多个监听啊，但是这样很麻烦。

KVO 支持统一监听属性的改变。`+(NSSet<NSString *> *)keyPathsForValuesAffectingValueForKey:(NSString *)key`



