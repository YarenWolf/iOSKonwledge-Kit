# 动态类型检查

#### 前言

1. 苹果Xcode的编译器叫做LLVM，可以编译C、C++、OC、Swift代码为二进制代码
2. 编译器在编译的时候可以判断指针是否可以调用指针指向对象的方法。编译的时候判断准则就是指针类型
3. 为了骗过编译器，程序在运行的时候还会做运行检查。运行的时候的判断准则：通过指针所指向的对象里面的isa找到代码段中的类，判断是否有这个方法可以执行
4. 可以写代码先判断一下对象中是否有这个方法再去执行

---

* 判断指定对象能否响应指定的方法
  * BOOL result = \[p1 respondsToSelector:@selector\(sayHi\)\];
* 判断指定的对象是否为指定类的对象或者指定类的子类对象
  * BOOL b2 = \[p1 isKindOfClass:\[Person class\]\];
* 判断对象是否指定类的对象，不包括子类
  * BOOL b4 = \[st isMemberOfClass:\[Person class\]\];
* 判断类是否为另一个类的子类
  * BOOL b5 = \[Student isSubclassOfClass:\[Person class\]\];
* isSubclassOfClass加上isMememberOfClass等于isKindOfClass

**总结：**

* \[指针respondsToSelector：@selector\(方法名\)\]

* \[对象名respondsToSelector：@selector\(方法名\)\]

* 用来判断对象能否调用对象方法或者类能否调用类方法。

* respondsToSelector：可以跟在对象或者类名后面，分别判断的是对象能否调用该对象方法和该类能否调用该类方法。

* instancesRespondToSelector：只能跟在类名后面，但是后面方法可以跟对象方法和类方法，作用是判断该类的对象能否调用该类里面的对象方法



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

@end


//Student
#import "Person.h"

@interface Student : Person
-(void)eat;
+(void)study;
@end

#import "Student.h"

@implementation Student
-(void)eat{
    NSLog(@"吃饭");
}

+(void)study{
    NSLog(@"学习");
}
@end



//测试
    Person *p1 = [[Person alloc] init];
    
    
    p1.name = @"lbp";
    p1.age = 20;
    
    //判断p1对象能否响应sayHi方法
    BOOL result = [p1 respondsToSelector:@selector(sayHi)];
    if (result) {
        [p1 sayHi];
    } else {
        NSLog(@"没有sayHi方法");
    }
    
    //判断指定的对象是否为指定类的对象或者指定类的子类对象
    BOOL b2 = [p1 isKindOfClass:[Person class]];
    NSLog(@"b2:%d",b2); //1
    
    
    Student *st = [[Student alloc] init];
     BOOL b3 = [st isKindOfClass:[Person class]];
    NSLog(@"b3:%d",b3); //1
    
    
    //判断对象是否指定类的对象，不包括子类
    BOOL b4 = [st isMemberOfClass:[Person class]];
    NSLog(@"b4:%d",b4);//0
    
   //判断类是否为另一个类的子类
    BOOL b5 = [Student isSubclassOfClass:[Person class]];
    NSLog(@"b5:%d",b5); //1
   
    //[指针 respondsToSelector：@selector(方法名)]
    //[对象名 respondsToSelector：@selector(方法名)]
    //用来判断对象能否调用对象方法或者类能否调用类方法。
    
    
   BOOL b6 = [Student respondsToSelector:@selector(study)];
    NSLog(@"b6:%d",b6);     //1
    
    BOOL b8 = [Student respondsToSelector:@selector(eat)];
    NSLog(@"b8:%d",b8);   // 0
    

    BOOL b7 = [st respondsToSelector:@selector(eat)];
   NSLog(@"b7:%d",b7);  //1
    
    
    
    BOOL b9 = [Student instancesRespondToSelector:@selector(study)];
    NSLog(@"b9:%d",b9);  //0
    
    BOOL b10 = [Student instancesRespondToSelector:@selector(eat)];
    NSLog(@"b10:%d",b10);  //1
```





