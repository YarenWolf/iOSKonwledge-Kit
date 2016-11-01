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