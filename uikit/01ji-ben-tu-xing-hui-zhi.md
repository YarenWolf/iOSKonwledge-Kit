# 基本图形绘制

---

#### 图形上下文

1. Graphics Context：是一个**CGContextRef**类型的数据
2. 作用：
   1. 保存绘图信息、绘图状态
   2. 决定绘制的输出目标（绘制到什么地方去？）输出目标可以是PDF文件、Bitmap、打印机、显示器窗口、Layer、

绘制好的图形-&gt;保存到上下文-&gt;渲染出来。

相同的一套绘图序列，制定不同的CGContextRef，就会可以将相同的图像绘制到不同的目标上。

---

#### 自定义View

* 如何利用Quartz2D绘制东西到view上？
  * 首先要有图形上下文，因为上下文保存了绘图的信息以及决定绘制到什么地方去
  * 上下文必须跟view相关联，才可以将内容绘制到view上
* 自定义view的步骤
  * 新建一个类继承自UIView
  * 实现 -\(void\)drawRect:\(CGRect\)rect方法，
  * 然后在这个方法中取得跟当前view相关的上下文
  * 绘制相应的图形内容
  * 利用图形上下文奖绘制的所有内容绘制到view上

举个🌰

需要绘制一条曲线

```
    //1、获得上下文
    CGContextRef ctx =  UIGraphicsGetCurrentContext();
    
    //2、绘制路径
    UIBezierPath *path = [UIBezierPath bezierPath];
    
    [path moveToPoint:CGPointMake(100, 200)];
    [path addQuadCurveToPoint:CGPointMake(200, 200) controlPoint:CGPointMake(150, 50)];
    
    CGContextSetLineWidth(ctx, 5);
    
    //3、将绘制的路径添加到上下文
    CGContextAddPath(ctx, path.CGPath);
    //4、把上下文的内容显示到view上（渲染）：fill、stroke
    CGContextStrokePath(ctx);
```

注意：CGContextStrokePath\(\)是将上下文绘制成线条填充，而CGContextFillPath\(\)是以图形的形式填充。



