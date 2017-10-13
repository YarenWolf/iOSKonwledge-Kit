# UIGestureRecognizer

为了完成手势识别必须借助手势识别器-UIGestureRecognizer.利用它可以识别用户在某个view上的一些常见手势

UIGestureRecognizer是一个抽象类，定义了手势的基本行为，使用它的子类才可以具体处理手势。

* UITapGestureRecoginzer\(敲击\)
* UIPinchGestureRecoginzer\(捏合、用于缩放\)
* UIPanGestureRecoginzer\(拖拽\)
* UISwipeGestureRecoginzer\(清扫\):一个手势只可以监听1个方向轻扫；如果要为控件监听多个方向的轻扫，则可以添加多个手势
* UIRotationGestureRecoginzer\(旋转\)
* UILongPressGestureRecoginzer\(长按）





```
- (void)viewDidLoad {
    [super viewDidLoad];
    //[self setupTapGestue];
//    [self setupLongPress];
    [self setupSwipe];
}

-(void)setupTapGestue{
    UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tapEvent:)];
    tap.delegate = self;
    self.imageView.userInteractionEnabled = YES;
    [self.imageView addGestureRecognizer:tap];
}

-(void)tapEvent:(UITapGestureRecognizer *)gesture{
    NSLog(@"%s",__func__);
}


-(void)setupLongPress{
    UILongPressGestureRecognizer *longpress = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(longPress:)];
    self.imageView.userInteractionEnabled = YES;
    [self.imageView addGestureRecognizer:longpress];
}


-(void)longPress:(UILongPressGestureRecognizer *)gesture{
    if (gesture.state == UIGestureRecognizerStateBegan) {
        NSLog(@"%s",__func__);
    }
}


-(void)setupSwipe{
    //系统默认一个UISwipeGestureRecognizer手势只有1个方向
    //如果要为1个控件监听多个不同方向的轻扫，则可以添加多个手势
    self.imageView.userInteractionEnabled = YES;
    
    UISwipeGestureRecognizer *swipeGest1 = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(swipe:)];
    swipeGest1.direction = UISwipeGestureRecognizerDirectionUp;
    [self.imageView addGestureRecognizer:swipeGest1];
    
    UISwipeGestureRecognizer *swipeGest2 = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(swipe:)];
    swipeGest2.direction = UISwipeGestureRecognizerDirectionDown;
    [self.imageView addGestureRecognizer:swipeGest2];
    
    UISwipeGestureRecognizer *swipeGest3 = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(swipe:)];
    swipeGest3.direction = UISwipeGestureRecognizerDirectionLeft;
    [self.imageView addGestureRecognizer:swipeGest3];
    
    UISwipeGestureRecognizer *swipeGest4 = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(swipe:)];
    swipeGest4.direction = UISwipeGestureRecognizerDirectionRight;
    [self.imageView addGestureRecognizer:swipeGest4];
}

-(void)swipe:(UISwipeGestureRecognizer *)gesture{
    NSLog(@"%s",__func__);
}


#pragma mark -- UIGestureRecognizerDelegate

-(BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldReceiveTouch:(UITouch *)touch{
    return YES;
}

@end
```



