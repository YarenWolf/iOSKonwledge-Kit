# KVO底层实现



## 一、KVO的基本使用

* 给要观察的对象的属性添加观察
* 实现 -\(void\)observeValueForKeyPath:\(NSString \*\)keyPath ofObject:\(id\)object change:\(NSDictionary&lt;NSKeyValueChangeKey,id&gt; \*\)change context:\(void \*\)context 方法
* 触发属性的改变

```
1、 [self.p addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionOld| NSKeyValueObservingOptionNew context:nil];
2、
-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context{
    NSLog(@"%@",change);
}

3、
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    static int a;
    self.p.name = [NSString stringWithFormat:@"%zd",a++]s;
}
```





