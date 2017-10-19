# 圆形图片裁剪

#### 步骤

1. 加载图片
2. 开启一个跟图片大小一样的图片上下文
3. 在图形上下文中添加一个圆形区域
4. 把路径设置为裁剪区域
5. 将图片绘制到上下文中
6. 生成一张图片（圆角）
7. 手动关闭上下文

Demo

```
    //1、加载一张图片
    UIImage *image = [UIImage imageNamed:@"onepiece"];

    //2、生成跟图片同样大小的上下文
    UIGraphicsBeginImageContextWithOptions(image.size, NO, 0);
    //3、在上下文添加一个圆形的裁剪区
    UIBezierPath *path  = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(0, 0, image.size.width, image.size.height)];

    //把路径设置成裁剪区域
    [path addClip];

    //4、将图片绘制到上下文
    [image drawAtPoint:CGPointZero];

    //5、生成一张图片
    UIImage *getImage = UIGraphicsGetImageFromCurrentImageContext();
    //6、手动关闭上下文
    UIGraphicsEndImageContext();

    self.imageView.image = getImage;
```



