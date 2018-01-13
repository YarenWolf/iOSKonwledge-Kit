# 转场动画

> 有种情况就是当你做了某件事情后界面需要切换，这时候就需要一个转场动画来实现。
>
> 转场 动画 CATransition ，不同于 CATransaction \(layer层的的动画事务\)





## 一、简单使用

* 实验1

```
static int _i = 1;

-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    _i++;
    if (_i > 3) {
        _i = 1;
    }
    NSString *imageName = [NSString stringWithFormat:@"%d",_i];
    self.imageView.image = [UIImage imageNamed:imageName];
    
    CATransition *animation = [CATransition animation];
    animation.type = @"cube";
    [self.imageView.layer addAnimation:animation forKey:nil];
}
```



![](/assets/2018-01-13 10_50_41.gif)



* 实验2

```
CATransition *animation = [CATransition animation];
animation.type = @"suckEffect";
[self.imageView.layer addAnimation:animation forKey:nil];
```

![](/assets/2018-01-13 10_58_42.gif)

**说明**

转场动画的 suckEffect 效果是从父控件的左上角缩进去。为了转场动画看上去符合交互习惯，所以一般使用 suckEffect 效果的时候，我们偷偷在 控件底部添加一个父控件，大小等大。





