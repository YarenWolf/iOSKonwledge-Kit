# 绘图的一些tips

1. 在绘图时一般要在控件的drawRect方法内部获取上下文，所以有些情况如果给一个控件赋值后再去根据值绘制图形，就要调用 setNeedsDisplay方法，这个方法的底层会调用控件的drawRect方法。如果手动调用drawRect方法是拿不到上下文的

下载进度条Demo

![](/assets/Simulator Screen Shot - iPhone 6s Plus - 2017-10-15 at 21.49.58.png)

```
//VC
- (void)viewDidLoad {
    [super viewDidLoad];
    [self.slider addTarget:self action:@selector(downloadProgressChange:) forControlEvents:UIControlEventValueChanged];
}

-(void)downloadProgressChange:(UISlider *)sender{
    self.progressView.progress = sender.value;
    self.label.text = [NSString stringWithFormat:@"%.2f%%",sender.value*100];
}



//ProgressView

#import "ProgressView.h"

@implementation ProgressView


-(void)drawRect:(CGRect)rect{
    CGPoint center = CGPointMake(rect.size.width * 0.5, rect.size.height * 0.5);
    CGFloat radius = rect.size.width * 0.5 - 10;
    CGFloat startA = -M_PI/2;
    CGFloat endA = -M_PI/2 + self.progress*2*M_PI;
    UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center radius:radius startAngle:startA endAngle:endA clockwise:YES];

    [path stroke];
}

-(void)setProgress:(CGFloat)progress{
    _progress = progress;
    //手动调用控件的drawRect方法不会去绘制图形，因为拿不到当前的上下文
//    [self drawRect:self.bounds];
    //调用控件的setNeedsDispaly方法，系统底层会调用drawRect方法，且可以拿到上下文
    [self setNeedsDisplay];
}

@end
```



