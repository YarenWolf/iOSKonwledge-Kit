# 滑动星级评分

> 整体5张图片，整体思路利用手势事件监听位置，通过位置判断更换图片

![](/assets/屏幕快照 2017-08-06 下午4.48.00.png)

```
- (void)viewDidLoad {
    [super viewDidLoad];
    self.title = self.doctorName;
    self.star = @"5";
    
    UITapGestureRecognizer *signaltap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(clickEvaluate:)];
    signaltap.cancelsTouchesInView = YES;
    [self.view addGestureRecognizer:signaltap];
}


-(void)clickEvaluate:(UITapGestureRecognizer *)tap{
    CGPoint point = [tap locationInView:self.view];
    CGFloat startX = BoundWidth/2 - 200/2;
    
    struct StarSpan span1 = {startX,startX + 24};
    struct StarSpan span2 = {startX + 24 + 20, startX + 20 + 24 + 24};
    struct StarSpan span3 = {startX + 20 + 24 + 24 + 20, startX + 20 + 24 + 24 + 20 + 24};
    struct StarSpan span4 = {startX + 20 + 24 + 24 + 20 + 24 + 20, startX + 20 + 24 + 24 + 20 + 24 + 20 + 24};
    struct StarSpan span5 = {startX + 20 + 24 + 24 + 20 + 24 + 20 + 24 + 20, startX + 20 + 24 + 24 + 20 + 24 + 20 + 24 + 20 + 24};
    if(point.x > 0 && point.x < span1.start ){
        
        self.starImageView.image = [UIImage imageNamed:@"Consult_star0-1"];
        self.star = @"0";
    }else if (point.x >= span1.start  && point.x < span2.start) {
        self.starImageView.image = [UIImage imageNamed:@"Consult_star1-1"];
        self.star = @"1";
    } else if(point.x >= span2.start && point.x < span3.start){
        self.starImageView.image = [UIImage imageNamed:@"Consult_star2-1"];
        self.star = @"2";
    }else if(point.x >= span3.start && point.x < span4.start){
        self.starImageView.image = [UIImage imageNamed:@"Consult_star3-1"];
        self.star = @"3";
    }else if(point.x >= span4.start && point.x < span5.start){
        self.starImageView.image = [UIImage imageNamed:@"Consult_star4-1"];
        self.star = @"4";
    }else if(point.x >= span5.start && point.x < BoundWidth){
        self.starImageView.image = [UIImage imageNamed:@"Consult_star5-1"];
        self.star = @"5";
    }
}



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



