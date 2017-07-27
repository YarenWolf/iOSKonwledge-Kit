# MRC模式下的Setter方法

完美setter方法：判断新旧对象是否是同一个对象，如果是同一个则什么都不做。

只有新旧对象不是同一个对象的时候，才release旧对象retain新对象



同时在对象dealloc的时候也要将属性release掉



```
-(void)setAAA:(AAA *)aaa{
    if(_aaa != aaa){
        [_aaa release];
        _aaa = [aaa retain];
    }
}


-(void)dealloc{
    [_aaa release];
    [super dealloc];
}
```



