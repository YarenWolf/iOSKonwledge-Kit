# 书写一个严谨的单例





#### 一般写法

```
@implementation Singleton

+(instancetype)shareInstance;

@end


@implementation Singleton

static Singleton *instance = nil;

+(instancetype)shareInstance{
    static dispatch_once_t onceToken;
    dispath_once(&onceToken,^{
        instance = [[self alloc]init];
    });
    return instance;
}

```

但是不知道这个类是单例的人会手动调用 alloc init 方法，为了防止这种情况，有2种解决办法

* 重写 alloc ，但调用 alloc 的时候给出 ASSert 奔溃
* 在创建对象的时候底层是 alloc （申请内存） 和 init（初始化），所以可以拦截用户的 alloc 和 copy 方法。当调用 alloc 底层会调用 allocWithZone 方法。 调用 copy 的时候底层调用 copyWithZone 方法。所以类还要遵循 NSCopying 协议

```
#import <Foundation/Foundation.h>

@interface Singleton : NSObject<NSCopying>

+(instancetype)sharedInstance;

@end
#import "Singleton.h"

@implementation Singleton

static Singleton *instance = nil;

+(instancetype)sharedInstance{
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken,^{
//        instance = [[self alloc]init];
        instance = [[super allocWithZone:NULL]init];
    });
    return instance;
}
+(id)allocWithZone:(struct _NSZone *)zone{
    return [Singleton sharedInstance];
}
-(id)copyWithZone:(NSZone *)zone{
    return [Singleton sharedInstance];
}
@end

作者：c4ibd3
链接：https://juejin.im/post/5a6141ebf265da3e303c8d23
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



