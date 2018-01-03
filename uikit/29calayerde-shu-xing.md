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



