# 对象在内存中的存储

* 栈、堆、BSS、数据段、代码段是什么？

  * 栈（stack）：又称作堆栈，用来存储程序的局部变量（但不包括static声明的变量，static修饰的数据存放于数据段中）。除此之外，在函数被调用时，栈用来传递参数和返回值。

  * 堆（heap）：用于存储程序运行中被动态分配的内存段，它的大小并不固定，可动态的扩张和缩减。操作函数\\(malloc／free\\)

  * BSS段（bss segment）：通常用来存储程序中未被初始化的全局变量和静态变量的一块内存区域。BSS是英文Block Started by Symbol的简称。BSS段输入静态内存分配

  * 数据段（data segment）：通常用来存储程序中已被初始化的全局变量和静态变量和字符串的一块内存区域

  * 代码段（code segment）：通常是指用来存储程序可执行代码的一块内存区域。这部分区域的大小在程序运行前就已经确定，并且内存区域通常属于只读，某些架构也允许代码段为可写，即允许修改程序。在代码段中，也有可能包含一些只读的常数变量，例如字符串常量。

![](/assets/内存.png)











* ##### 搞清楚上面的概念再来研究下对象在内存中如何存储？

```
Person *p1 = [Person new]
```

看这行代码，先来看几个注意点：

1. new底层做的事情：

2. 在堆内存中申请1块合适大小的空间

3. 在这块内存上根据类模版创建对象。类模版中定义了什么属性就依次把这些属性声明在对象中；对象中还存在一个属性叫做\*\*isa\*\*，是一个指针，指向对象所属的类在代码段中地址

4. 初始化对象的属性。这里初始化有几个原则：a、如果属性的数据类型是基本数据类型则赋值为0；b、如果属性的数据类型是C语言的指针类型则赋值为NULL；c、如果属性的数据类型为OC的指针类型则赋值为nil。

5. 返回堆空间上对象的地址

6. 注意

7. 对象只有属性，没有方法。包括类本身的属性和一个指向代码段中的类isa指针

8. 如何访问对象的属性？指针名-&gt;属性名；本质：根据指针名找到指针指向的对象，再根据属性名查找来访问对象的属性值

9. 如何调用方法？\\[指针名 方法\\];本质：根据指针名找到指针指向的对象，再发现对象需要调用方法，再通过对象的isa指针找到代码段中的类，再调用类里面方法

10. 为什么不把方法存储在对象中？

11. 因为以类为模版创建的对象只有属性可能不相同，而方法相同，如果堆区的对象里面也保存方法的话就会很重复，浪费了堆空间，因此将方法存储与代码段

12. 所以一个类创建的n个对象的isa指针的地址值都相同，都指向代码段中的类地址

做个小实验

\`\`\`

\#import &lt;Foundation/Foundation.h&gt;

@interface Person : NSObject{

```
@public

int \_age;

NSString \*\_name;

int \*p;
```

}

* \(void\)sayHi;

@end

@implementation Person

* \(void\)sayHi{

  NSLog\(@"Hi, %@",\_name\);

}

@end

int main\(int argc, const char \* argv\[\]\) {

```
Person \*p1 = \[Person new\];

Person \*p2 = \[Person new\];

Person \*p3 = \[Person new\];

p1-&gt;\_age = 20;

p2-&gt;\_age = 20;





\[p1 sayHi\];

\[p2 sayHi\];

\[p3 sayHi\];

return 0;
```

}

\`\`\`

Person  \\*p1 = \\[Person new\\];这句代码在内存分配原理如下图所示

!\[p1\]\([https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/Untitled Diagram-2.png](https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/Untitled Diagram-2.png) "p1"\)

结论

!\[p1\]\([https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/屏幕快照 2017-05-15 下午5.35.17.png](https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/屏幕快照 2017-05-15 下午5.35.17.png) "p1"\)

!\[p3\]\([https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/屏幕快照 2017-05-15 下午5.35.34.png](https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/屏幕快照 2017-05-15 下午5.35.34.png) "p3"\)

可以 看到Person类的3个对象p1、p2、p3的isa的值相同。

