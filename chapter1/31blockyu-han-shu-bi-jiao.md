# Block作为方法返回值

当Block作为返回值的时候，必须使用typedef来将Block数据类型起别名（Xcode不认识这种数据类型）

```
void(^)() test(){
    OurBlock block = ^{
        NSLog(@"你好");
    };
    return block;
}
```

```
typedef void(^OurBlock)();
OurBlock test(){
    OurBlock block = ^{
        NSLog(@"你好");
    };
    return block;
}
```

# 

# Block与函数

* 相同点：都是封装1段代码
* 不同点
  * Block是一个数据类型，函数是1个函数
  * 我们可以声明Block类型的变量，而不能声明函数类型的函数
  * Block可以作为函数的参数和返回值（因为Block是一种数据类型）



