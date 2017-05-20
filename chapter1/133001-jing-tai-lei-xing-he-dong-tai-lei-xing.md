# OC是1门弱类型语言

* 编译器在编译的时候，检查不是很严格。

* 不管怎么写，检查不是很严格。int num = 12.12;NSString \*str = \[Pig new\]; \[str length\];

* 优点：灵活。缺点：太灵活

# 强类型语言：编译器在编译的时候做语法检查的时候，如果语法不通过编译就报错。

* 静态类型：1个指针指向的对象是1个本类对象

* 动态类型：1个指针指向的对象不是本类对象

# 编译检查

* 编译器在编译的时候，通过指针调用指针所指向的对象的方法会进行判断

  * 判断原则：通过指针去堆区找指针所指向的对象，再通过对象的isa指针在代码段找类，查看类中是否有方法实现，如果有则通过编译。如果没有则编译失败

* 注意：编译时期能否通过关键看指针的类型



# 运行检查



编译检查只是骗过了编译起，但是这个方法能不能真正执行会在运行的时候去对象中是否真的有这个方法，有就执行，没有就报错

5、LSP

父类指针指向子类对象

实际上任意指针可以指向任意对象，编译器不会报错

\#import &lt;Foundation/Foundation.h&gt;

@interface Animal : NSObject

@property NSString \*name;

-\(void\)run;

@end

\#import "Animal.h"

@implementation Animal

-\(void\)run{

NSLog\(@"run"\);

}

@end

\#import "Animal.h"

@interface Pig : Animal

-\(void\)eat;

@end

\#import "Pig.h"

@implementation Pig

-\(void\)eat{

NSLog\(@"吃完就死"\);

}

@end

Pig \*pig = \[Animal new\];

\[pig eat\];

当1个子类指针指向父类对象的时候，编译器运行子类指针调用子类独有的方法，但是在运行的时候就会报错

