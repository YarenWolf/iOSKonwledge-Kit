# 图形上下文状态栈



先来一个小例子。

需求：画一个十字架，横线用红色、宽10px，竖线用黑色、1px

按照以前的套路和现有的知识是可以实现的。思路：先画一条红色且宽度为10px的横线，再给上下文赋值，画一条1px的黑线

```
-(void)drawRect:(CGRect)rect{
    
    //1、得到图形上下文
    CGContextRef ctx =UIGraphicsGetCurrentContext();
    //2、绘制路径
    UIBezierPath *path = [UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(150, 10)];
    [path addLineToPoint:CGPointMake(150, 290)];
    
    CGContextSetLineWidth(ctx, 10);
    [[UIColor redColor] set];
    
    //3、将路径添加到图形上下文中
    CGContextAddPath(ctx, path.CGPath);
    //4、渲染上下文
    CGContextStrokePath(ctx);
    
    path = [UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(10, 150)];
    [path addLineToPoint:CGPointMake(290, 150)];

    [[UIColor blackColor] set];
    CGContextSetLineWidth(ctx, 1);
    //3、将路径天教导图形上下文中
    CGContextAddPath(ctx, path.CGPath);
    //4、渲染上下文
    CGContextStrokePath(ctx);
}
```

#### 缺点：

虽然实现了需求，但是目前只是设置了线条的宽度和颜色，假如属性太多当再次设置其他线条的属性的时候就会很复杂，需要设置为默认的属性。至此**图形上下文状态栈**就闪亮登场了。



图形上下文：由2部分组成，上部分是图形的路径信息，下部分是图形的属性信息（比如宽度、颜色等等）。

因此为了避免为了设置不同的属性的时候需要额外设置更多的属性为之前的值而产生不必要的麻烦。



```
-(void)drawRect:(CGRect)rect{
    
    //1、得到图形上下文
    CGContextRef ctx =UIGraphicsGetCurrentContext();
    //2、绘制路径
    
    UIBezierPath *path = [UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(10, 150)];
    [path addLineToPoint:CGPointMake(290, 150)];
    //保存当前的图形上下文状态栈
    CGContextSaveGState(ctx);
    
    
    [[UIColor redColor] set];
    CGContextSetLineWidth(ctx, 10);
    //3、将路径添加到图形上下文中
    CGContextAddPath(ctx, path.CGPath);
    //4、渲染上下文
    CGContextStrokePath(ctx);
    
   
    //3、将路径天教导图形上下文中
    CGContextAddPath(ctx, path.CGPath);
    //4、渲染上下文
    CGContextStrokePath(ctx);
    
    
    path = [UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(150, 10)];
    [path addLineToPoint:CGPointMake(150, 290)];
    //从图形上下文状态栈中恢复保存的上下文属性信息
    CGContextRestoreGState(ctx);
    //3、将路径天教导图形上下文中
    CGContextAddPath(ctx, path.CGPath);
    //4、渲染上下文
    CGContextStrokePath(ctx);
    
}
```



