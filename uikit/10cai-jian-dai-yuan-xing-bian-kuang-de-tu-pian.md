# 裁剪带圆形边框的图片

> 思路步骤：
>
> 1、加载图片
>
> 2、开启画图的上下文
>
> 3、先按照目的图片的大小画一个大圆，给大圆填充圆形边框的颜色
>
> 4、画一个小圆，设置裁剪
>
> 5、将图片绘制到上下文中
>
> 6、从当前上下文中得到一张图片
>
> 7、关闭之前开始的图形上下文



![](/assets/tupiancaijian)



写个裁剪带有边框的图片分类，只需要传递一些参数就好

# UIImage+ClipBorderColor

```
#import <UIKit/UIKit.h>

@interface UIImage (ClipBorderColor)



/**
 裁剪带有边框的图片

 @param borderWidth 边框宽度
 @param borderColor 边框的颜色
 @param image 需要裁剪的图片
 @return 裁剪后带有边框的图片
 */
+(UIImage *)imageWithBorderWidth:(CGFloat)borderWidth borderColor:(UIColor *)borderColor image:(UIImage *)image;
@end


#import "UIImage+ClipBorderColor.h"

@implementation UIImage (ClipBorderColor)
+(UIImage *)imageWithBorderWidth:(CGFloat)borderWidth borderColor:(UIColor *)borderColor image:(UIImage *)image{
    
    //3、开启图片上下文
    CGRect imageSize = CGRectMake(0, 0, image.size.width + 2*borderWidth, image.size.height + 2*borderWidth);
    UIGraphicsBeginImageContextWithOptions(CGSizeMake(image.size.width + 2*borderWidth, image.size.height + 2*borderWidth), NO, 0);
    
    //4、绘制大圆（带有背景颜色的底层大圆）
    UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:imageSize];
    [borderColor set];
    [path fill];
    
    //5、绘制小圆
    path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(borderWidth, borderWidth, image.size.width , image.size.height)];
    //设置裁剪
    [path addClip];
    //5、把图片给绘制上下文.
    [image drawInRect:CGRectMake(borderWidth, borderWidth, image.size.width , image.size.height)];
    //6、从当前上下文得到一张新的图片
    UIImage *gotImage = UIGraphicsGetImageFromCurrentImageContext();
    
    //7、关闭上下文
    UIGraphicsEndImageContext();
    return gotImage;
}
@end
```











