# ARC

1. ARC机制下当2个对象互相引用的时候如果2边都使用strong，那么就会造成内存泄漏  
   1. 解决方案：一端使用strong，另一端使用weak

2. ARC机制下如果属性的类型是OC对象类型绝大多数情况使用strong。

3. 在ARC机制下，如果属性的类型不是OC对象，使用assign

4. strong 和weak都是应用在属性的类型是OC对象的情况下，属性的类型不是OC对象则使用assign。

5. 在ARC机制下将MRC下的retain转换为ARC的strong

6. @property \(nonmatic ,strong \) Car \*car;代表生成的私有属性是1个强类型

7. @property \(nonmatic ,weak \) Car \*car;代表生成的私有属性是1个弱类型

8. 如果 对属性不写strong 和weak中的任一属性，默认就是strong（强类型）

9. ARC机制下对象什么时候被回收？

   1. 指向对象的指针被回收

   2. 指向对象的指针被赋值为nil

   3. 对象被回收的表象：没有1个强指针指向这个对象

   4. 对象被回收的本质：对象的引用计数器为0

10. ARC机制下不能将创建的对象用1个弱指针去存储这个对象的地址。（一初始化马上就被释放掉）

11. 强指针：默认情况下我们声明的指针就是1个强指针，或者使用\_\_strong 来显式声明的指针

12. 弱指针：使用\_\_weak修饰的指针



# ARC可能遇到的问题 

* 现状：你在ARC机制下写工程项目，但是你需要用到一个类（前人大牛留下的），但是这个类是一个MRC模式的类

* 解决办法：在Xcode中找到项目对应的“TARGETS”，选择“Build Phases”下的“Compiles Sources”，找到需要将文件设置为支持“ARC”做法为，在文件后面点击添加“-fobjc-arc”
* 将文件设置为不支持ARC，在文件后面点击添加“-fno-objc-arc”

```

//Dog类
#import <Foundation/Foundation.h>

@interface Dog : NSObject{
    NSString *_name;
}

-(void)setName:(NSString *)name;

-(NSString *)name;

-(void)bark;
@end

#import "Dog.h"

@implementation Dog

-(void)dealloc{
    NSLog(@"啊，我这条狗死掉了...");
    [_name release];
    [super dealloc];
}

-(void)setName:(NSString *)name{
    if (_name != name) {
        [_name release];
        _name = [name retain];
    }
    _name = name;
}

-(NSString *)name{
    return _name;
}

-(void)bark{
    NSLog(@"嗨，大家好，我是明星狗狗%@",_name);
}

@end


//Person类
#import <Foundation/Foundation.h>
#import "Dog.h"



@interface Person : NSObject

@property (nonatomic, strong) Dog *dog;

-(void)sayHi;

-(void)playWithDog;
@end
#import "Person.h"

@implementation Person

-(void)sayHi{
    NSLog(@"大家好");
}


-(void)playWithDog{
    [_dog bark];
    NSLog(@"狗狗，我们走");
}

-(void)dealloc{
    NSLog(@"啊， 我死了");
    [super dealloc];
}

@end

//test

    Person *p = [[Person alloc] init];
    
    Dog *d = [[Dog alloc] init];
    d.name = @"啸天犬";
    p.dog = d;
    
    [p playWithDog];
    
    [d release];
    [d release];
    [p release];
```



