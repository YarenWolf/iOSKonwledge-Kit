#### CALayer

### 一、 基本概念

1、所有写在 **layer** 对象后的属性都只作用在根层上

self.operateView.layer.cornerRadius =5;

2、超过根层以外的东西都会被裁减掉

self.operateView.layer.masksToBounds = YES;

3、层级结构：

Layer:根层

content：内容层（UIImageView的图片显示在内容层）

mask层：

初始化好的view对应的mask层和layer层一样大，当设置过layer的属性后mask层也做出了相应的修改，当内容层超过根层后，mask层会裁减掉。

### 二、layer 层上的动画

1. #### 旋转

   1. 解释下：CATransform3D CATransform3DMakeRotation \(CGFloat angle, CGFloat x, CGFloat y, CGFloat z\)的4个参数：
   2. 参数1 angle ：单位弧度，正值代表顺时针旋转，负值代表逆时针旋转
   3. 参数2 x ： 代表x轴方向
   4. 参数3 y ：代表y轴方向
   5. 参数4 z ： 代表z轴方向
   6. 这里的x、y、z 取值和数学中的坐标系一样，计算的结果就是方向向量

```
 [UIView animateWithDuration:0.5 animations:^{
        self.operateView.layer.transform = CATransform3DMakeRotation(M_PI, -1, 1, 0);
    }];
```

#### 2、平移

后面的参数代表向量

```
self.operateView.layer.transform = CATransform3DMakeTranslation(10, 10, 10);
```

#### 3、缩放

```
self.operateView.layer.transform = CATransform3DMakeScale(0.5, 0.5, 0.5);
```

#### 三、CALayer的一些快速动画设置

**KVC** 可以给属性赋值，同样可以给 layer 的transform 属性赋值，像下面的样子

```
[self.operateView.layer setValue:[NSValue valueWithCATransform3D:CATransform3DMakeScale(0.5, 0.5, 0.5)] forKey:@"transform"];
```

但是很多人觉得直接给 layer 的 transform 属性赋值更快，为什么此处还要介绍 KVC？

KVC 的使用场景？

利用 layer 的 transform 属性 结合 KVC 做一些快速动画

```
//缩放
[self.operateView.layer setValue:@0.5 forKeyPath:@"transform.scale.x"];
//平移
[self.operateView.layer setValue:@20 forKeyPath:@"transform.translation.x"];
//旋转
[self.operateView.layer setValue:@(M_PI) forKeyPath:@"transform.rotation.x"];
```





#### 四、CALayer 的常见问题

CALayer 可以直接显示图片，负责的是 contents 属性。

```
    CALayer *layer = [CALayer layer];
    layer.frame = CGRectMake(50, 50, 100, 100);
    layer.backgroundColor = [UIColor redColor].CGColor;
    [self.view.layer addSublayer:layer];
    
    //用layer显示图片
    UIImage *image = [UIImage imageNamed:@"temp"];
    layer.contents = (id)image.CGImage;
    
    //同样能显示图片，选择标准？
    CALayer 可以显示图形，但是不可以接受手势交互
    UIView 可以显示图形，可以接收手势交互  （性能会差一点）
```



#### 五、杂谈



* CALayer QuartzCore框架

*  CGColorRef CGImageRef CoreGraphics框架

*  UIColor UIImage UIKit框架



