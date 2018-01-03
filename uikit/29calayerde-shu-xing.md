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

#### 四、CALayer 的常见属性

* CALayer 可以直接显示图片，负责的是 contents 属性。

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

* CALayer 还有关于位置显示的2个属性，**position** 和 **anchorPoint**

  * position 默认的值 为 （0,0）：当前 layer 在父 layer 上的位置。距离父 layer 原点（左上角）的距离
  * anchorPoint 默认值为 \(0.5,0.5\) ：决定着 layer 的左上角顶点距离 position 点的距离。
    * $$（position.x - realPoint.x) = layer.bounds.width*anchorPoint.x$$
    * $$（position.y - realPoint.y) = layer.bounds.height*anchorPoint.y$$
  * * 当为0的时候代表 ：子 layer 的左上角顶点距 position 距离为 \(0,0\)
    * 当 1则代表子 layer 的右下角的顶点距离 position 距离为 \(0,0\)。
  * 根据我推导出的公式，如果知道一个 **layer** 的 **position** 和 **bounds** 就可以知道一个控件的精确位置
    * $$realPoint.x = position.x -  layer.bounds.width*anchorPoint.x$$
    * $$realPoint.y = position.y -  layer.bounds.height*anchorPoint.y$$

2者需要结合使用才可以控制 layer 的位置。

* 借助图理解
  ![](/assets/QQ20180103-165949@2x.png)

例如需要让一个 layer 显示在屏幕中间

```
 layer.position = CGPointMake(self.view.bounds.size.width/2, self.view.bounds.size.height/2);

 layer.anchorPoint = CGPointMake(0.5, 0.5);

 [self.view.layer addSublayer:layer];
```

#### 五、杂谈

* CALayer QuartzCore框架 - 可以跨平台 （iOS 、Mac OS）

* CGColorRef CGImageRef CoreGraphics框架 - 可以跨平台 （iOS 、Mac OS）

* UIColor UIImage UIKit框架



