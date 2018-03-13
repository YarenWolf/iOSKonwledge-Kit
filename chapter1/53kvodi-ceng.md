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

1、KVO 对于观察的属性是自动发出通知的（观察者模式的一种。当被观察的对象的属性值发生改变，KVO会自动发送通知给观察者）。但是有些需求就是不需要自动通知。那么有个方法可以让开发者自己处理，对某个观察的属性设置是否需要自动通知观察者

`+(BOOL)automaticallyNotifiesObserversForKey:(NSString *)key`

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
```

2、

