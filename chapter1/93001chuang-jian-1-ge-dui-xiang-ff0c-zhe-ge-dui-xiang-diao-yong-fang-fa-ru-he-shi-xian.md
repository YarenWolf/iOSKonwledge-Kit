

# 创建1个对象，这个对象在内存中是如何分配的



* 子类对象中有自己的属性和所有父类的属性

* 代码段中的每1隔类都有1个叫做isa的指针，这个指针指向它的父类

* 一直指到NSObject



\[p1 sayHi\];

现根据p1指针找到p1所指向的对象，然后根据这个对象的isa指针找到person类。搜索person类中是否有这个sayHi方法，如果有就执行，如果没有，就根据类的isa指针找到父类...，NSObject中没有就报错。







```
//Person类
#import <Foundation/Foundation.h>

@interface Person : NSObject{
    @public
    NSString *_name;
    int _age;
    
}


-(void)help;

-(void)sleep;

@end
#import "Person.h"

@implementation Person
-(void)sleep{
    NSLog(@"我好困啊");
}
@end

//superman类
#import <Foundation/Foundation.h>
#import "Person.h"
@interface SuperMan : Person

@end

#import "SuperMan.h"

@implementation SuperMan
-(void)sleep{
    NSLog(@"超人好困啊");
}
@end


//超人类
#import "SuperMan.h"

@interface Chaoren : SuperMan

@end
#import "Chaoren.h"

@implementation Chaoren

@end




//实验
Chaoren *sp = [Chaoren new];
[sp sleep];
```

##### \[sp sleep\]的调用过程：

* 先根据sp指针找到sp指针所指向的对象，然后根据指向对象的isa指针找到Chaoren类，然后查看Chaoren类中是否有sleep方法，如果有就执行，如果没有就不执行且根据在代码段中的isa指针继续向上找父类，然后在父类中判断是否有sleep方法，有就执行，没有则继续向上找，直到找到NSObject类为止，如果NSObject类也没有实现这个方法则报错。
* 整个过程用1张图表示如下


![对象函数调用过程](https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/Untitled Diagram.png "对象函数调用过程")




