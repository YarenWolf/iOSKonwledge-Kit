# 协议-Protocol

* 作用：专门声明一大堆方法，或者属性@property \(不能声明属性，也不能实现方法，只能用来写方法的声明\)
* 只要遵循了该协议，那么相当于拥有了这个协议中所有的方法声明
* 虽然不能声明属性，但是可以声明@property，但是协议不会自动生成私有属性（下划线开头的实例变量\_xxx），只会生成setter getter的声明但是不会生成setter getter的实现，因此如果遵循该协议的类要利用这些属性，那么必须自己实现协议中属性的setter getter方法
* 协议的声明

```
@protocol 协议名称<NSObject>

//方法的声明

@end
```

* 协议文件的新建：New File-&gt;Objective-C File -&gt;Protocol 
* 协议的文件名： .h 并且只有一个.h文件
* 类遵循协议：如果想要一个类拥有协议中所有的方法声明，那么就让这个类遵循这个协议

```
@interface 类名:父类名<协议名称>

@end
```

#### 例子

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
* 在协议中声明的方法，意味着遵循该协议的类中需要实现相应的方法，如果不实现也不会报错。如果该方法用@required声明，那么编译只会报警告，如果用@optional声明的方法，在该类中不实现也不会报警告

#### 怎么办呢：

* 虽然协议中声明了property，声明了setter和getter，如果在遵循该协议的类中要去使用，我们必须在该类中声明私有属性，自己去实现setter getter方法
* 虽然协议中约定了方法，如果遵循本协议的类不实现该方法是不会报错，但是当调用这个方法且这个方法没有实现就会报错。为此我们可以加个判断，判断当前的类是否实现了该方法

#### 改进代码：

```
//Person.m
#import "Person.h"


@interface Person (){
    NSString *_name;
    NSInteger _age;
}

@property (nonatomic, strong) NSString *sports;
@end

@implementation Person
-(void)setName:(NSString *)name{
    if (_name != name) {
        _name = name;
    }
}

-(NSString *)name{
    return _name;
}

-(void)setAge:(NSInteger)age{
    if (_age != age) {
        _age = age;
    }
}

-(NSInteger)age{
    return _age;
}


-(void)scan{
    NSLog(@"我在阅读");
}

-(void)read{
   NSLog(@"我在读书");
}


@end


//test
    Person *person = [Person new];

    person.age = 20;
    person.name = @"杭城小刘";
    NSLog(@"我叫%@，我今年%zd了",person.name,person.age);
    [person scan];

    [person read];

    if ([person respondsToSelector:@selector(see)]) {
        [person see];
    }
```

测试结果

![](/assets/屏幕快照 2017-06-27 下午2.43.43.png)

