# 构造方法

* new方法的内部就是先调用alloc方法，再调用init方法

  * alloc方法：那个类接受alloc消息，那么该方法返回该接受类的对象，并把对象返回

  * init方法：是1个对象方法，作用：初始化对象

* 创建对象的步骤：先使用alloc创建1个对象，再使用init初始化这个对象，才可以使用这个对象

  * 使用1个未被初始化的对象是很危险的

* init方法：作用：初始化对象，为对象赋初始值，叫做构造方法

# 重写init构造方法

* 如果想创建出来的对象的属性值不是默认的初始化值，则需要重写init方法

* 重写init方法的规范：

  * 必须要先调用父类的init方法（因为当前类初始化就是通过\`\[父类init\]\`去初始化），然后将返回值赋值给self

  * 调用init方法有可能会失败，如果失败直接返回nil

  * 判断父类是否初始化成功。如果self != nil，则代表初始化成功

  * 如果初始化成功就去初始化当前对象的属性

  * 最后返回self

#### 解惑：

1. 为什么要调用父类的init方法？
   1. 当前类有isa指针，当前类的isa指针赋值是通过父类的init方法赋值的。
   2. 需要保证当前对象的父类属性同时被初始化
2. 重写init方法的规范：

```
-(instancetype)init{
    if (self = [super init]) {
        //todo：自定义属性的初始化
    }
    return self;
}
```

```
//Person

#import <Foundation/Foundation.h>

@interface Person : NSObject
@property NSString* name;
@property int age;

-(void)sayHi;

@end

#import "Person.h"
@implementation Person

-(void)sayHi{
    NSLog(@"Hi");
}

-(instancetype)init{
    self = [super init];
    if (self) {
        self.name = @"杭城小刘";
        self.age = 22;
    }
    return self;
}
@end


//测试

 Person *p1 = [[Person alloc] init];    //p1.name = "杭城小刘"，p1.age =22;
 Person *p2 = [Person new];             //p2.name = "杭城小刘"，p2.age =22;
```

如果2个类的关系为组合关系，且它的一个属性是另一个类的对象，那么当该类初始化的时候默认它的属性为nil，那么如何初始化？

```
-(instancetype)init{
    self = [super init];
    if (self) {
        self.name = @"lbp";
        self.age = 22;
        self.pig = [[Pig alloc] init];
    }
    return self;
}

//测试
  Person *p1 = [[Person alloc] init];        //p1.dog != nil
```

# 自定义构造方法

* 现状：虽然每次双肩的对象的属性值不是默认的，但是每次初始化的对象的值都是一样的。

* 需求：每次实例化的对象的属性值由调用者决定

* 解决办法：自定义构造方法

* 自定义构造方法规范：

  * 自定义构造方法的返回值为instancetype

  * 方法的命名必须以initWith开头

  * 方法的实现类似init的实现

**注意：此时不能使用new来调用。（因为new的实现是先alloc再init，默认init的实现是给属性赋默认值）**

```
-(instancetype)initWithName:(NSString *)name andAge:(int)age{
    if (self = [super init]) {
        self.name = name;
        self.age = age;
    }
    return self;
}
```

```
//Person
#import <Foundation/Foundation.h>
@interface Person : NSObject
@property NSString* name;
@property int age;

-(instancetype)initWithName:(NSString *)name andAge:(int)age;
@end

#import "Person.h"
@implementation Person

-(instancetype)init{
    self = [super init];
    if (self) {
        self.name = @"lbp";
        self.age = 22;
    }
    return self;
}

//不能在构造方法之外给self赋值
//编译器认为只有以initWith开头的方法是构造方法

-(instancetype)initWithName:(NSString *)name andAge:(int)age{
    if (self = [super init]) {
        self.name = name;
        self.age = age;
    }
    return self;
}

@end


//测试

Person *p1 = [[Person alloc] init];
Person *p2 = [Person new];    
Person *p3 = [[Person alloc] initWithName:@"杭城小刘2号" andAge:23];
```

![](/assets/屏幕快照 2017-05-23-5-56-53.png)

![](/assets/屏幕快照 2017-05-23-5-57-08.png)

关于“自定义构造方法必须以initWith开头”做个实验

![](/assets/屏幕快照 2017-05-23-6-01-29.png)

报错信息很明显：不能在构造方法之外给self赋值

因为，编译器认为只有以initWith开头的方法是构造方法

