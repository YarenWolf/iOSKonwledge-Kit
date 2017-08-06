# 滑动星级评分

> 整体5张图片，整体思路利用手势事件监听位置，通过位置判断更换图片

![](/assets/屏幕快照 2017-08-06 下午4.48.00.png)

```
-(void)touchesMoved:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    CGPoint point = [[touches anyObject] locationInView:self.view];    
    CGFloat startX = BoundWidth/2 - 200/2;
    CGFloat x = point.x;

    if(x <= startX){            //0
        self.starImageView.image = [UIImage imageNamed:@"consult_star0"];
    }
    else if (x > startX && x < (startX+200/5)) {       //1
        self.starImageView.image = [UIImage imageNamed:@"consult_star1"];
        self.star = @"1";
    }
    else if (x >= (startX+200/5) && x < (startX+(200*2)/5)) {       //2
        self.starImageView.image = [UIImage imageNamed:@"consult_star2"];
        self.star = @"2";
    }
    else if (x >= (startX+(200*2)/5) && x < (startX+(200*3)/5)) {       //3
        self.starImageView.image = [UIImage imageNamed:@"consult_star3"];
        self.star = @"3";
    }
    else if (x >= (startX+(200*3)/5) && x < (startX+(200*4)/5)) {       //4
        self.starImageView.image = [UIImage imageNamed:@"consult_star4"];
        self.star = @"4";
    }
    else if(x > startX + 200){                     //5
        self.starImageView.image = [UIImage imageNamed:@"consult_star5"];
        self.star = @"5";
    }

}
```



