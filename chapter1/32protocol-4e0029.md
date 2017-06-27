# 协议-Protocol

* 作用：专门声明一大堆方法，或者属性@property \(不能声明属性，也不能实现方法，只能用来写方法的声明\)
* 只要遵循了该协议，那么相当于拥有了这个协议中所有的方法声明
* 虽然不能声明属性，但是可以声明@property，但是协议不会自动生成私有属性（下划线开头的实例变量\_xxx），只会生成setter getter的声明但是不会生成setter getter的实现，因此如果遵循该协议的类要利用这些属性，那么必须自己实现协议中属性的setter getter方法

```
//StudyProtocol.h
#import <Foundation/Foundation.h>

@protocol StudyProtocol <NSObject>

@property (nonatomic, assign) NSInteger age;

@property (nonatomic, strong) NSString *name;

-(void)read;

-(void)see;

-(void)scan;

@end


//Person.h
#import <Foundation/Foundation.h>
#import "StudyProtocol.h"

@interface Person : NSObject<StudyProtocol>


@end

#import "Person.h"


@interface Person ()

@property (nonatomic, strong) NSString *sports;
@end

@implementation Person

-(void)scan{
    NSLog(@"我在阅读");
}

-(void)read{
   NSLog(@"我在读书");
}


@end
```

* 图1![](/assets/屏幕快照 2017-06-27 上午9.07.25.png)
* 图2![](/assets/屏幕快照 2017-06-27 上午9.34.31.png)

#### 结论：

* 虽然在协议上声明了一个@property，但是不会生成私有属性,所以看不到在协议中的age和name的私有属性
* 


