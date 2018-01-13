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



* 实验3

```
CATransition *animation = [CATransition animation];
animation.type = @"pageCurl";
animation.subtype = kCATransitionFromLeft;
animation.duration = 0.5;
[self.imageView.layer addAnimation:animation forKey:nil];
```

**说明**

subtype 属性可以设置转场的方向

* 实验4

```
CATransition *animation = [CATransition animation];
animation.type = @"pageCurl";
animation.subtype = kCATransitionFromTop;
animation.duration = 1.5;
animation.startProgress = 0.2;
animation.endProgress = 0.8;
[self.imageView.layer addAnimation:animation forKey:nil];
```

![](/assets/2018-01-13 12_09_57.gif)

**说明**

* startProgress 设置转场动画的开始位置， endProgress 设置转场动画的结束位置。发现一个问题只要在动画的场合设置值都是从 \[0-1\] 
* 而且转场动画的代码必须和动画代码放在一个代码块内

  


## 二、UIView 的转场动画

```
[UIView transitionWithView:self.imageView duration:1 options:UIViewAnimationOptionTransitionCurlUp animations:^{
    _i++;
    if (_i > 3) {
        _i = 1;
    }
} completion:^(BOOL finished) {

}];
```

**说明**



UIView 的转场动画的效果比较少，主要有

```
    UIViewAnimationOptionTransitionNone            = 0 << 20, // default
    UIViewAnimationOptionTransitionFlipFromLeft    = 1 << 20,
    UIViewAnimationOptionTransitionFlipFromRight   = 2 << 20,
    UIViewAnimationOptionTransitionCurlUp          = 3 << 20,
    UIViewAnimationOptionTransitionCurlDown        = 4 << 20,
    UIViewAnimationOptionTransitionCrossDissolve   = 5 << 20,
    UIViewAnimationOptionTransitionFlipFromTop     = 6 << 20,
    UIViewAnimationOptionTransitionFlipFromBottom  = 7 << 20,
```



CA 的转场动画效果特别多，主要有

```
/* Common transition types. */

CA_EXTERN NSString * const kCATransitionFade
    CA_AVAILABLE_STARTING (10.5, 2.0, 9.0, 2.0);
CA_EXTERN NSString * const kCATransitionMoveIn
    CA_AVAILABLE_STARTING (10.5, 2.0, 9.0, 2.0);
CA_EXTERN NSString * const kCATransitionPush
    CA_AVAILABLE_STARTING (10.5, 2.0, 9.0, 2.0);
CA_EXTERN NSString * const kCATransitionReveal
    CA_AVAILABLE_STARTING (10.5, 2.0, 9.0, 2.0);

/* Common transition subtypes. */

CA_EXTERN NSString * const kCATransitionFromRight
    CA_AVAILABLE_STARTING (10.5, 2.0, 9.0, 2.0);
CA_EXTERN NSString * const kCATransitionFromLeft
    CA_AVAILABLE_STARTING (10.5, 2.0, 9.0, 2.0);
CA_EXTERN NSString * const kCATransitionFromTop
    CA_AVAILABLE_STARTING (10.5, 2.0, 9.0, 2.0);
CA_EXTERN NSString * const kCATransitionFromBottom
    CA_AVAILABLE_STARTING (10.5, 2.0, 9.0, 2.0);
```



