# 上下文的矩阵操作

Demo

```
- (void)drawRect:(CGRect)rect {
    // Drawing code

    //1、得到图形上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    //2、绘制路径
    UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(-100, -50, 200, 100)];

    [[UIColor brownColor] set];

    //位移
    CGContextTranslateCTM(ctx, 100, 50);

    //缩放
    CGContextScaleCTM(ctx, 0.5, 0.5);

    //旋转
    CGContextRotateCTM(ctx, M_PI_4);

    //3、添加路径到上下文
    CGContextAddPath(ctx, path.CGPath);

    //4、渲染上下文
    CGContextFillPath(ctx);

}
```





#### 注意点

* 上下文的矩阵操作必须在添加到上下文之前进行





