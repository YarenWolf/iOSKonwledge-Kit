心跳

```
CABasicAnimation *animation = [CABasicAnimation animation];
    animation.toValue = @0.2;
    animation.keyPath = @"transform.scale";
    animation.repeatCount = MAXFLOAT;
    animation.duration = 0.5;
    animation.autoreverses = YES;
    [self.imageView.layer addAnimation:animation forKey:nil];
```



