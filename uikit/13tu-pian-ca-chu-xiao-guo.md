# 图片擦除效果

### 思路

* 给图片开启手势交互
* 给图片添加滑动手势
* 获取手势点的位置
* 规定手指在屏幕上的大小
* 开启图片上下文，确定上下文的rect
* 将图片绘制到上下文中
* 在手势滑动过程中确定擦除区域
* 从当前上下文获取图片
* 将图片显示出来
* 关闭上下文

Demo

```
- (void)viewDidLoad {
    [super viewDidLoad];
    UIPanGestureRecognizer *panGesture = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(panGestureChange:)];
    self.imageView.userInteractionEnabled = YES;
    [self.imageView addGestureRecognizer:panGesture];
}


-(void)panGestureChange:(UIPanGestureRecognizer *)pan{

    //图片区域擦除效果

    //1、获取手势所在的点
    CGPoint position = [pan locationInView:self.imageView];
    //2、开启图片上下文，确定上下文所在的rect
    CGFloat rectWH = 44;
    CGRect rect = CGRectMake(position.x - rectWH/2, position.y - rectWH/2, rectWH, rectWH);
    UIGraphicsBeginImageContextWithOptions(self.imageView.bounds.size, NO, 0);
    CGContextRef ctx = UIGraphicsGetCurrentContext();


    //3、将图片绘制到上下文中
    [self.imageView.layer renderInContext:ctx];

    //4、将手势所在的区域裁剪掉
    CGContextClearRect(ctx, rect);
    //5、从当前上下文中得到图片
    UIImage *imageGot = UIGraphicsGetImageFromCurrentImageContext();
    //6、将新的图片显示出来
    self.imageView.image = imageGot;
    //7、关闭上下文
    UIGraphicsEndImageContext();
}
```



