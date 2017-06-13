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



