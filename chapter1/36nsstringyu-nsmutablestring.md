# NSString 

```
NSString *str = @"杭城小刘";
NSString *str2 = @"杭城小刘";
```

* NSString 在对象中创建的时候先在数据段创建一个对象@"杭城小刘"，然后在堆中创建一个对象str，将数据段@"杭城小刘"的地址赋值给str
* 因为NSString创建对象都是根据数据段中的对象地址去创建，所以当2个字符串的值一样的时候NSString的地址是一样的，所以str和str2的地址是一样的

![](/assets/A17E097F-4C3A-4461-A7CA-EDF75BA24780.png)





# NSMutableString

NSMutableString是继承自NSString的对象，特点是字符串变量可以改变打破字符串的恒定性

#### ![](/assets/9971B1F7-0893-4B0D-AF70-65FE0786F3E0.png)说明：

* 该段程序很耗时间，因为变量的类型是NSString类型，每次为str赋不同的新值的时候其本质就是在数据段创建不同的对象，然后将对的地址赋值给指针变量，所以很耗时（对象创建本来就很耗时）



