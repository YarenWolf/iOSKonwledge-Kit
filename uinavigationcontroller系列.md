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

    3、电池电量状态栏改变颜色

```
状态栏的字体为黑色：UIStatusBarStyleDefault
状态栏的字体为白色：UIStatusBarStyleLightContent

1、全局设置颜色为白色，只有启动页和登录页为黑色
步骤：1）、在info.plist中添加“View controller-based status bar apperance”，类型为Bool，值为NO
2）、在Appdelegate中写
[[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent animated:YES];
3）、在登录的VC中
-(void)viewWillAppear:(BOOL)animated{
    [super viewWillAppear:animated];
    [[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleDefault animated:YES];
}

-(void)viewWillDisappear:(BOOL)animated{
    [super viewWillDisappear:animated];
    [[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent animated:YES];
}

```

注意：

在info.plist中，将View controller-based status bar appearance设为YES，或者没有设置。
View controller-based status bar appearance的默认值就是YES。如果View controller-based status bar appearance为YES。
则[UIApplication sharedApplication].statusBarStyle 无效。


解决办法：
1、在vc中重写
-(UIStatusBarStyle)preferredStatusBarStyle{
   return UIStatusBarStyleDefault;
}

2、在viewDidLoad中调用[self setNeedsStatusBarAppearanceUpdate]
但当vc在navi中时vc中的preferredStatusBarStyle方法根本不用被调用。

原因是，[self setNeedsStatusBarAppearanceUpdate]发出后，
只会调用navigation controller中的preferredStatusBarStyle方法，vc中的preferredStatusBarStyle不会被调用。















