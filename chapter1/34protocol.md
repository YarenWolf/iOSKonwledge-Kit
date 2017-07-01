# 协议的类型限制、

> 需求：请声明一个指针，这个指针可以指向任何对象，但是这个对象必须遵循DevelopProtocol协议，如果不遵循起码给出警告

```
//Peron
#import <Foundation/Foundation.h>
#import "DevelopProtocol.h"

@interface Person : NSObject<DevelopProtocol>


@end


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

-(void)learnObjectiveC{
    NSLog(@"学习OC");
}

-(void)learnSwift{
    NSLog(@"学习Swift");
}

-(void)developApp{
    NSLog(@"练习开发App");
}

@end


//StudyProtocol
#import <Foundation/Foundation.h>

@protocol StudyProtocol <NSObject>

@property (nonatomic, assign) NSInteger age;

@property (nonatomic, strong) NSString *name;

-(void)read;

-(void)scan;

@optional
-(void)see;

@end

//#import <Foundation/Foundation.h>
#import "StudyProtocol.h"

@protocol DevelopProtocol<StudyProtocol>

@required
-(void)learnObjectiveC;
-(void)learnSwift;

@optional
-(void)developApp;

@end




   //test   
NSObject<DevelopProtocol>  *p = [Person new];

id<DevelopProtocol> p1 = [Person new];


id<DevelopProtocol, StudyProtocol> p3 = [Person new];

id<DevelopProtocol, PlayProtocol> p4 = [Person new];
```

结论：

* NSObject&lt;协议名称&gt; \*指针名;

* * 这个时候这个指针可以指向遵循了指定协议的任何对象，否则就会报警告

  * 当然了，可以指针类型使用id
* id&lt;协议名称&gt;指针名称;

> //声明一个指针，该指针指向遵循学习协议的学生对象

```
//Student
#import <Foundation/Foundation.h>
#import "StudyProtocol.h"

@interface Student : NSObject<StudyProtocol>


@end

#import "Student.h"

@implementation Student

@end

//Developer
#import "Student.h"

@interface Developer : Student

@end


#import "Developer.h"

@implementation Developer

@end


//test
NSObject<StudyProtocol> *st = [Student new];

id<StudyProtocol> *st1 = [Student new];

NSObject<StudyProtocol> *st2 = [Developer new];


```

结论

* 声明一个指针，该指针指向遵循学习协议的学生对象**Student&lt;StudyProtocol&gt; \*st2 = \[Student new\];**
* 当然，也可以指向这个学生的子类的对象**Student&lt;StudyProtocol&gt; \*st3 = \[Developer new\];**

思考：为什么？

* 遵循了某个协议的类，相当于这个类拥有了这个协议所定义的行为

* 因为我们要调用这个对象的协议方法，只有遵循了协议。，这个类一定会有协议方法

