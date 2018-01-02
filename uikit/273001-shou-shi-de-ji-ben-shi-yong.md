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





## 手势的一些注意事项

对于 **UITapGestureRecognizer** 来说我们一般需要知道该点击手势在屏幕中的位置 （locationInView:self）

对于 ** UIPanGestureRecognizer** 来说我们一般需要知道我们的滑动手势移动了多少距离 （translationInView:pan）

```
- (void)pan:(UIPanGestureRecognizer *)pan{
    
    CGPoint transP = [pan translationInView:pan.view];        //$1 = (x = 0.73990527317289434, y = 0)

    CGPoint pont1 = [pan locationInView:self];            //$2 = (x = 198.16665649414063, y = 342.33332824707031)
        
      CGPoint pont2 = [pan locationInView:self.imageV];        //$3 = (x = 198.12057060663793, y = 342.61609831987914)
    
    pan.view.transform = CGAffineTransformTranslate(pan.view.transform, transP.x, transP.y);
    //复位
    [pan setTranslation:CGPointZero inView:pan.view];
    
    
}
```



