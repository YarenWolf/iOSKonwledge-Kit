# NSObject指针

* 是OC中所有类的基类，根据LSP NSObject指针是所有类的父类，因此NSObject指针可以指向任意的OC对象。所以NSObject指针是1个万能指针可以指向任意OC对象
* 缺点：如果要调用指向的子类对象的独有方法，就必须做类型转换。

# 

# id指针

* id是一个万能指针，可以指向任意OC对象
* id是1个typedef自定义类型，在定义的时候已经加了\*。所以声明id指针的时候不需要加\*了
* id是一个万能指针，任意的OC对象都可以指

# 

# NSObject和id的异同

* 相同点：万能指针，都可以指向任意OC对象
* 不同点
  * 通过NSObject对象去调用对象的方法的时候，编译器会做编译检查，因此调用方法需要加类型转换
  * 通过id类的指针去调用对象的方法的时候，编译器会直接通过，无论你调用什么方法（可能是不做编译检查）
  * id指针只能调用对象方法，不能调用点语法，如果使用点语法会直接报编译错误





#### 总结：如果我们要声明1个万能指针，千万不要用NSObject，而是使用id



