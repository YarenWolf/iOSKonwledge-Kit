# Block访问变量



#  1、Block不允许修改外部变量的值，但是可以访问



这里需要注意一下。

&gt;外部变量是指存储在栈区（声明在函数内部等的局部变量），只可读不可写

&gt;如果需要在Block内部修改存储在栈区的局部变量，那么需要在变量前面加上\_\_block修饰

&gt;如果是生命在函数外部的全局变量，那么在block内部是可读可写的

&gt;我们观察变量的内存地址可以发现，对于局部变量，那么在block打印地址发现是存储在栈区，在block内部是堆区，在block结束后打印是在栈区

&gt;对于全局变量来说，打印地址：堆区-&gt;堆区-&gt;堆区

&gt;对于局部变量来说，打印地址：栈区-&gt;栈区-&gt;栈区



* block与局部变量

```
typedef  void (^TestBlock)();
int main(int argc, const char * argv[]) {
    @autoreleasepool {
     
      //1、block与局部变量
        int a = 0;
        
        NSLog(@"a->%p",&a);     //a->0x7fff5fbff6bc
        
        TestBlock block = ^{
        
            a = 1;        
            NSLog(@"a->%zd",a);     //a->0
            NSLog(@"a->%p",&a);     //a->0x100403650
        };
        block();
        NSLog(@"a->%p",&a);         //a->0x7fff5fbff6bc
    

    }
    return 0;
}
```

观察到：1、0x7fff5fbff6bc是在栈区，0x100403650是在堆区，因为栈的地址比堆的地址高。2、block内修改a会编译出错，加上\_\_block修饰，就可以修改局部变量的值

* block与全局变量

```
int a = 0;
typedef  void (^TestBlock)();
int main(int argc, const char * argv[]) {
    @autoreleasepool {
     
   
        
        NSLog(@"a->%p",&a);     //a->0x1000010c8

        TestBlock block = ^{
            a = 1;
            NSLog(@"a->%zd",a);     //a->1
            NSLog(@"a->%p",&a);     //a->0x1000010c8
        };
        block();
        NSLog(@"a->%p",&a);         //a->0x1000010c8
    

    }
    return 0;
}
```

观察到：block对于全局变量都是可读可写的，也不需要加上\_\_block修饰







