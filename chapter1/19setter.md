MRC模式下的Setter方法

```
-(void)setAAA:(AAA *)aaa{
    if(aaa != _aaa){
        [_aaa release];
        _aaa = [aaa retain];
    }
}
```



