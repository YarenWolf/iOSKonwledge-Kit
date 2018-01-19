# 从一个 UIImage 的基础上截取指定位置的图片为 UIImage



> 最近在写一个抽奖的转盘动画，转盘是由按钮旋转做成的。按钮上面的内容都不一样，是根据一张大图，裁剪成每个按钮需要的图片然后设置给按钮的 image。然后再封装按钮（内部确定图片的位置）。所以这次主要想说的就是一个图片在指定区域的裁剪





**关键代码**

```
CGFloat scale = [UIScreen mainScreen].scale;

CGFloat buttonImageOriginX = 0;
CGFloat buttonImageOriginY = 0;
CGFloat buttonImageWidth = scale * contentImage.size.width / 12;
CGFloat buttonImageHeight = scale * contentImage.size.height;
buttonImageOriginX = i * buttonImageWidth;
//裁剪区域
CGRect contentImageRect = CGRectMake(buttonImageOriginX, buttonImageOriginY, buttonImageWidth, buttonImageHeight);

//裁剪图片
CGImageRef contentImageRef = CGImageCreateWithImageInRect(contentImage.CGImage, contentImageRect);
CGImageRef contentSelectedImageRef = CGImageCreateWithImageInRect(contentSelectedImage.CGImage, contentImageRect);

[button setImage: [UIImage imageWithCGImage: contentImageRef] forState: UIControlStateNormal];
```

说明：由于 Apple 的 CoreGraphics 是跨平台的框架，底层是 C 语言写的，所以 UIKit 框架下图片的尺寸不能直接使用，必须利用当前设备的**“像素点分辨率” **去设置截取区域。

