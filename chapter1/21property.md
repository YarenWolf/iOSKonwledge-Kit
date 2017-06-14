# property

1. @property可以带参数：@property \(参数1,参数2,参数3......\) 数据类型 名称；
2. 介绍一下@property的4组参数

3. 1. 与多线程相关的参数  ：建议使用nonatomic，要效率不要安全
      1. atomic  ：默认值、如果写atomic，这个时候会为生成的setter方法添加一个线程安全锁。（特点：安全、效率低下）
      2. nonatomic  ：如果写nonatomic，这个时候生成的setter方法不会添加一个线程安全锁。（特点：不安全、效率高）
   2. 与生成的setter方法相关的参数  ：当属性的类型是OC对象的时候使用retain；如果属性是非OC对象那么则使用assign

   3. 1. assign  ：生成的setter方法直接赋值
      2. retain  ：生成的setter方法是标准的MRC模式下内存管理代码。（先判断新旧对象是否是同一个值，如果是同一个值不做处理，如果不是同一个值则release旧的值，retain新的值）
   4. 与生成只读、读写相关的参数  
      1. readonly  ：只会生成getter方法
      2. readwrite  ：默认属性值。会生成getter和setter方法
   5. 与生成getter、setter方法名字相关的参数。默认情况下@property生成的getter、setter方法的名字都是最标准的名字。其实我们可以通过参数来指定@property生成的方法的名字

      1. getter：getter=getter方法的名字，用来指定@property生成getter方法的名字

      2. setter：setter=setter方法的名字，用来指定@property生成的setter方法的名字

      注意：（1）、无论什么时候都不要修改setter方法的名字，因为默认情况下生成的setter方法名字都是标准的。（2）、什么时候修改getter方法的名字，当属性的类型是1个Bool值的时候可以修改这个getter方法的名字，并以“is”开头，提高代码的阅读性。

注意：同一组属性值只可以出现1次，但是setter和getter可以同时存在，；参数的顺序可以任意

# property参数总结

* 程序开发分为：ARC、MRC
* 与多线程相关参数：atomic：安全，但效率低。nonatomic：不安全，但效率高

  * 不管MRC或者ARC，属性都用nonmatic

* retain

  * 只能用在MRC模式下，代表生成的setter方法是标准的MRC内存管理代码
  * 当属性是OC对象的时候用retain，只有出现了循环引用的情况时1边使用assign

* assign
  * 在ARC和MRC模式下都可以使用
  * 在属性不是OC对象的时候使用assign
  * 
* strong

  * 只能使用在ARC机制下

  * 当属性的类型为OC对象，绝大多数情况使用strong

  * 当出现循环引用的时候，一端使用weak，一端使用strong

6、weak

在存在于ARC机制下，当属性的类型是OC对象的时候使用weak

当出现循环引用的情况下一端使用weak，另一端使用strong

7、readonly、readwrite

ARC、MRC模式下都可以使用

8、getter、setter

MRC、ARC下都可以使用，用来修改getter和setter方法的名字

setter不推荐修改；getter：当属性是布尔时候getter = isShow；

---

在ARC机制下，（MRC：retain）-&gt;strong

在ARC机制下，（MRC：一边retain，一边assign）-&gt;一边strong，一边weak

