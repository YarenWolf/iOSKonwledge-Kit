# 图片添加水印

步骤

1. 开启图片上下文
2. 绘制图片
3. 绘制文字（加水印）
4. 根据上下文生成新图片
5. 手动关闭上下文

```
    UIImage *image = [UIImage imageNamed:@"onepiece"];
    /*
     *siz:开启一个多大的上下文
     *opaque: 不透明度
     *scale: 0
     */
    //1、开启和图片相同大小的上下文     
    UIGraphicsBeginImageContextWithOptions(image.size, YES, 0);
    //2、绘制图片
    [image drawAtPoint:CGPointZero];
    
    
    NSString *iconStr = @"@one piece 路飞";
    NSMutableDictionary *dict = [NSMutableDictionary dictionary];
    dict[NSFontAttributeName] = [UIFont systemFontOfSize:17];
    dict[NSForegroundColorAttributeName] = [UIColor redColor];
    dict[NSBackgroundColorAttributeName] = [UIColor whiteColor];
   //3、绘制文字（加水印）
    [iconStr drawInRect:CGRectMake(image.size.width - 130, image.size.height - 20, 130, 20) withAttributes:dict];
    
    //4、生成图片：从上下文信息中得到图片
    UIImage *getImage = UIGraphicsGetImageFromCurrentImageContext();
    
    self.imageView.image = getImage;
    
    //5、手动关闭上下文
    UIGraphicsEndImageContext();
```



