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

> 如果用户去调用UIApplication的alloc方法直接奔溃掉，这种效果如何实现？
>
> 其实这是系统跑出了一个异常

![](/assets/屏幕快照 2017-07-15 下午4.12.10.png)思路：

* 自己初始化一个对象，以static的形式存储
* 对外提供一个类方法以返回该类的唯一对象（方法要命名规范，参考苹果的单例获取方法，shared+类名）
* 外部如果调用alloc抛出异常信息，用NSException声明，然后raise

```
#import <Foundation/Foundation.h>

@interface Person : NSObject

+(Person *)sharedPerson;

@end
#import "Person.h"

@implementation Person

static Person *_instance = nil;

+(void)load{
    [super alloc];
   _instance = [[self alloc] init];
}


//该方法用于拦截用户调用者调用该类的alloc方法
+(instancetype)alloc{
    if (_instance) {
     NSException *excepttion = [NSException exceptionWithName:@"NSInternalInconsistencyException" reason:@"'There can only be one Person instance." userInfo:nil];
        //抛出异常
        [excepttion raise];
        
    }
    
    return [super  alloc];
}

+(Person *)sharedPerson{
    return _instance;
}

@end 
```



