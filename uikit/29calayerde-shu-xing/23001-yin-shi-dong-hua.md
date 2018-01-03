# 隐式动画

## 一、概念

* 每个 UIView 内部都默认关联着一个 CALayer ，我们称这个 Layer 为 Root Layer （根层）
* 所有的非 Root Layer ，也就是手动创建的 Layer 对象，都存在着隐式动画
* 当对非 Root Layer 的某些属性做修改时，会产生一些动画效果，这样的属性叫做 Animatable Properties \(可动画属性\):bounds、BackgroundColor、position

## 二、基本用法

* 动画被包装为一个事务（类似数据库的概念）
* 开启 layer 的隐式动画 setDisableActions:NO
* 设置隐式动画的时间 setAnimationDuration：10    

```
    [CATransaction begin];
    [CATransaction setDisableActions:NO];
    [CATransaction setAnimationDuration:3];

    self.layer.bounds = CGRectMake(100, 100, 200, 200);
    self.layer.backgroundColor = [UIColor yellowColor].CGColor;
    self.layer.cornerRadius = 40;

    [CATransaction commit];
```



