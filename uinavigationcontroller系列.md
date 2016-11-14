###去除nagationBar下面的自带的一条线

 ```
 1、@property (nonatomic, strong) UIImageView *navBarHairlineImageView;
2、 - (void)viewDidLoad { 

 [super viewDidLoad];
 self.navBarHairlineImageView = [self findHairlineImageViewUnder:self.navigationController.navigationBar];

}

3、 -(void)viewWillAppear:(BOOL)animated{ 
 [super viewWillAppear:animated];
 self.navBarHairlineImageView.hidden = YES;

}

4、 - (void)viewWillDisappear:(BOOL)animated {
 [super viewWillDisappear:animated];
 self.navBarHairlineImageView.hidden = NO;
}

5、 - (UIImageView *)findHairlineImageViewUnder:(UIView *)view { 
 if ([view isKindOfClass:UIImageView.class] && view.bounds.size.height <= 1.0) {
     return (UIImageView *)view；
 }

 for (UIView *subview in view.subviews) {

     UIImageView *imageView = [self findHairlineImageViewUnder:subview];

     if (imageView) {
         return imageView;
     }
}
 return nil;
}

```


###其他
    1、让navigationController下面的某个VC禁止向左滑动返回上一界面。
```
-(void)viewDidAppear:(BOOL)animated
{
 [super viewDidAppear:animated];
 self.navigationController.interactivePopGestureRecognizer.enabled = NO;
}

-(void)viewDidDisappear:(BOOL)animated{
 [super viewDidDisappear:animated];
 self.navigationController.interactivePopGestureRecognizer.enabled = YES;
}
```
    2、让某个VC隐藏顶部电量状态栏
```
-(void)viewWillAppear:(BOOL)animated{
 [super viewWillAppear:animated];
 [[UIApplication sharedApplication] setStatusBarHidden:YES withAnimation:UIStatusBarAnimationFade];
}


-(void)viewWillDisappear:(BOOL)animated{
 [super viewWillDisappear:animated];
 [[UIApplication sharedApplication] setStatusBarHidden:NO withAnimation:UIStatusBarAnimationFade];
}
```

    3、让某个vc的电量等颜色为黑色
```

```