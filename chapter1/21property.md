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
      2. readwrite  ：会生成getter和setter方法
   5. 与生成getter、setter方法名字相关的参数

      1. getter

      2. setter



