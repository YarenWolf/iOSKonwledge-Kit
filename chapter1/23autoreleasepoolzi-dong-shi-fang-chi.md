# 自动释放池

1. 原理：存入到自动释放池中的对象，在自动释放池被销毁的时候会自动调用存储在该自动释放池中所有对象的release方法
2. 如何创建自动释放池

```
@autoreleasepool{




}
```

3、如何将对象存储到自动释放池中：在自动释放池当中调用对的autorelease方法。该方法会返回对象本身，因此我们可以这么写



```
@autorelease{
    Person *p = [[Person alloc] init] autorelease];
    
}
```



