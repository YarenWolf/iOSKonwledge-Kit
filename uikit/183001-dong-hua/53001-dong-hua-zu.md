# 动画组

> 我们在平时的开发中经常遇到这样一种需求，让一个控件在做平移动画的时候伴随着让它做旋转动画，这时候我们可以会写2个 CABasicAnimation 设置好相应的属性后再 分别添加到控件上，这样子没毛病。但是情况二就是当你在做完动画的时候不需要让动画恢复原状，fillMode、removedOnCompletion 这2个属性大家都知道，可以让动画保持最后的一个状态，做2个动画还要，如果要多个动画的话，我们要写这样的代码多次，这可不是程序员的写法。





# 一、直蹦主题

![](/assets/2018-01-13 17_11_46.gif)



* 普通动画叠加效果

```
    //方法1:
    CABasicAnimation *scaleAnimation = [CABasicAnimation animation];
    scaleAnimation.keyPath = @"transform.scale";
    scaleAnimation.toValue = @0.4;
    
    scaleAnimation.fillMode = kCAFillModeForwards;
    scaleAnimation.removedOnCompletion = NO;
    
    [self.animationView.layer addAnimation:scaleAnimation forKey:nil];
    
    
    CABasicAnimation *positionAnimation = [CABasicAnimation animation];
    positionAnimation.keyPath = @"position.y";
    positionAnimation.toValue = @500;
    
     //下面2行代码让动画停留在动画结束的位置
    positionAnimation.fillMode = kCAFillModeForwards;
    positionAnimation.removedOnCompletion = NO;
    
    [self.animationView.layer addAnimation:positionAnimation forKey:nil];
```

* 动画组的写法

```
    CAAnimationGroup *animationGroup = [CAAnimationGroup animation];
    
    CABasicAnimation *positionAnimation = [CABasicAnimation animation];
    positionAnimation.keyPath = @"position.y";
    positionAnimation.toValue = @500;
    
    CABasicAnimation *scaleAnimation = [CABasicAnimation animation];
    scaleAnimation.keyPath = @"transform.scale";
    scaleAnimation.toValue = @0.5;
    
    //设置动画组
    animationGroup.animations = @[positionAnimation,scaleAnimation];
    //集中设置动画状态属性
    animationGroup.fillMode = kCAFillModeForwards;
    animationGroup.removedOnCompletion = NO;
    
    [self.animationView.layer addAnimation:animationGroup forKey:nil];
```



