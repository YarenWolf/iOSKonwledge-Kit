# 点语法

* OC对象如果要为属性赋值或者取值，就要调用相应的getter或者setter方法

* 点语法在编译器编译的时候会将点语法转换为调用getter和setter方法的代码

  * 当使用点语法赋值的实现就是调用setter

  * 当使用点语法取值的实现就是调用getter

##### @property只能生成getter和setter的声明，实现还要靠我们自己来，而实现也没什么技术含量，方法的实现代码能不能自动生成？

### @synthesize

* 作用：自动生成getter和setter的实现，必须写在类的实现中
* 语法：@synthesize property名称

* ```
  @interface Person:NSObject{

  int _age;

  }

  @property int age;

  @end

  @implementation Person

  @synthesize age;

  @end
  ```
* @synthesize做的事情

```
@interface Person:NSObject{
    int _age;
}
@property int age;
@end

@implementation Person{
    int age;
}

@synthesize age;

-(void)setAge:(int)age{
    self->age = age;
}
@end
```

* a、生成1个真私有的属性，属性的类型和@synthesize对应的@property类型一致。属性的名字和@synthesize对应的@property名字一致
* b、自动生成setter方法的实现

### 实现的方式：将参数直接赋值给自动生成的私有属性

* 自动生成getter方法的实现

* 实现的方式：将生成的私有属性返回

### **希望@synthesize不去自动生成新的私有属性?**直接在getter和setter方法中去操作我们已经写好的属性就可以了

* 语法：

  * @synthesize @property名称=已经存在的属性名；

  * @synthesize age = \_age;

    * 1）不会再去生成私有属性

    * 2）、直接生成getter setter的实现

* setter的实现：把参数值直接赋值给指定的属性

* getter的实现：直接返回指定的属性值

* 批量声明

  * 如果多个@property的类型一致，可以批量声明

    * @property float height,weight;

  * @synthesize的也可以批量声明

    * @synthesize name = \_name,age = \_age;

---

### 以上都是很早的Xcode版本实现

### @property类型属性名

* 1、自动生成1个私有属性

* 2、自动生成getter和setter的声明

* 3、自动生成getter和setter的实现

* 4、可以批量声明相同类的peoperty

* 5、@peoperty生成的getter和setter是直接返回的，没有做任何逻辑处理

我们可以重写setter来自定义验证逻辑，如果重写了setter会自动生成getter

如果重写了getter会自动生成setter

如果重写setter和getter会出问题

6、如果想为类写一个属性，并且为属性封装setter和getter

1个@property属性搞定

7、继承

父类的@property一样可以被继承

@property生成的属性是私有的，在子类的内部是无法直接访问生成的私有属性（什么叫无法直接访问生成的私有属性，就是self-&gt;name）

但是可以通过setter和getter来访问（self.name）

