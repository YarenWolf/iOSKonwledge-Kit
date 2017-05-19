# SEL

* SEL全称叫做selector选择器

  * SEL是1个数据类型，所以要在内存中申请空间存储数据

  * SEL其实是1个类，SEL对象是用来存储1个方法的。

* 类是以Class对象的形式存储在代码段中，存储3种信息

  * 类名：存储这个类的类名。NSString

  * 属性s：

  * 方法s：还要将方法存储在这个类中。

* 如何将方法存储在类对象中

  * 先创建1个SEL对象

  * 将方法的信息存储在SEL对象中

  * 再将这个SEL对象作为类对象的属性

* 拿到存储方法的SEL对象

  * 因为SEL是一个typedef类型的，在自定义的时候已经加了\*。所以在声明SEL指针的时候不需要加\*

  * 取到存储方法的对象

    * SEL s1 = @selector\(sayHi\);

* 调用方法的本质

  * \[p1 sayHi\];

  * 内部原理：

    * 先拿到存储sayHi方法的SEL对象，也就是拿到存储sayHi方法的SEL数据（SEL消息）

    * 将SEL消息发送给（p1）对象

    * p1对象接收到这个SEL消息后就知道要调用方法

    * 根据对象的isa指针找到存储类的类对象

    * 找到类对象以后，在这个类对象中去搜寻是否有和传入SEL数据相匹配的方法，如果有则属执行，如果没有则继续再找父类，直到NSObject，如果NSObject类也没有相应的方法，则报错

**OC最重要的机制就是消息机制。**

**调用方法的本质就是为对象发送SEL消息**

![](/assets/类对象的方法存储.png)

1. 第一次访问类的时候将类的信息加载进代码段，称为类加载
2. 现在栈中存储一个Person类型的p1指针
3. 在堆中初始化1个Person对象
4. 类在代码段存储是1个Class对象，这个Class对象至少有3种信息，类名称、类属性s、类方法s
5. 在调用\[p1 sleep\]的时候先用@selector\(sleep\)找到存储sleep方法的SEL对象
6. 然后SEL向堆中的对象发送消息（图上的2）
7. 然后堆中的对象利用isa指针在代码段中查找类信息（图上的3）
8. 再在代码段中的类中查找是否有方法实现，如果有则实现，没有则继续在代码段中的类的isa指针继续向父类查找继续找方法实现直到找到NSObject类，如果还没找到方法实现则报错

```
    Person *p1 = [Person new];
    [p1 sleep];

    //底层实现原理就是下面的代码

    Person *p1 = [Person new];
    SEL s1 = @selector(sleep);
    [p1 performSelector:s1];
```





  
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 18.0px 'PingFang SC'; color: \#4dbf56}  
p.p2 {margin: 0.0px 0.0px 0.0px 0.0px; font: 18.0px Menlo; color: \#4dbf56}  
span.s1 {font: 18.0px Menlo; font-variant-ligatures: no-common-ligatures}  
span.s2 {font-variant-ligatures: no-common-ligatures}  
span.s3 {font: 18.0px 'PingFang SC'; font-variant-ligatures: no-common-ligatures}  


调用方法有2种实现：

* \[对象名方法名\];
* SEL s1 = @selector\(方法名\);\[对象名performSelector:s1\];





