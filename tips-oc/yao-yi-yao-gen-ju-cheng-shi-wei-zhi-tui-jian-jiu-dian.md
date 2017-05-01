###摇一摇根据城市位置推荐酒店###

1、实现摇一摇并震动需要导入头文件。#import <AudioToolbox/AudioToolbox.h>
2、当前城市定位，可以看我之前的文字[快速定位](https://my.oschina.net/u/1778933/blog/884823)
3、让vc支持摇一摇。

```
    [self becomeFirstResponder];
    [UIApplication sharedApplication].applicationSupportsShakeToEdit = YES;
```

4、关键代码


```
#pragma mark - UIResponder
-(void)motionBegan:(UIEventSubtype)motion withEvent:(UIEvent *)event{
    AudioServicesPlaySystemSound(kSystemSoundID_Vibrate);
    self.bgView.alpha = 0;
    self.bgView.hidden = YES;
    [UIView animateWithDuration:1.0 animations:^{
        [self getRandomHotel];
        self.bgView.alpha = 1;
        self.bgView.hidden = NO;
        self.hotelImage.image = [UIImage imageNamed:@"My_about"];
        self.label.text = @"您已经成功摇到一个酒店，不喜欢？换个姿势再来一次";
        [self.label sizeToFit];
        
        self.hotelImage.contentMode =  UIViewContentModeScaleAspectFill;
        self.hotelImage.autoresizingMask = UIViewAutoresizingFlexibleHeight;
        self.hotelImage.clipsToBounds  = YES;
        
        //动态设置uilabel的高度
        self.hotelLabel.numberOfLines = 0;
        self.hotelLabel.lineBreakMode = NSLineBreakByWordWrapping;
        
    } completion:^(BOOL finished) {

    }];
    NSLog(@"摇一摇开始");
    return ;
}

-(void)motionCancelled:(UIEventSubtype)motion withEvent:(UIEvent *)event{
    NSLog(@"取消摇一摇");
    return ;
}

-(void)motionEnded:(UIEventSubtype)motion withEvent:(UIEvent *)event{
    if (motion ==UIEventSubtypeMotionShake ){
        NSLog(@"摇一摇结束");
    }
    return ;
}
```



