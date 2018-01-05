# UIImageview按照比例裁剪

```
//压缩图片
- (UIImage *)cutImage:(UIImage*)image{
    CGSize newSize;
    CGImageRef imageRef = nil;

    if ((image.size.width / image.size.height) < (61 / 61)) {
        newSize.width = image.size.width;
        newSize.height = image.size.width * 61 / 61;

        imageRef = CGImageCreateWithImageInRect([image CGImage], CGRectMake(0, fabs(image.size.height - newSize.height) / 2, newSize.width, newSize.height));

    } else {
        newSize.height = image.size.height;
        newSize.width = image.size.height * 61 / 61;

        imageRef = CGImageCreateWithImageInRect([image CGImage], CGRectMake(fabs(image.size.width - newSize.width) / 2, 0, newSize.width, newSize.height));
    }

    return [UIImage imageWithCGImage:imageRef];
}
```

---





# 一、网络下载的图片适配UIImageView

```
UIImage * image = [UIImage imageWithData: [NSData dataWithContentsOfURL: [NSURL URLWithString: [NSString stringWithFormat: @ "%@%@", UserAdatarUrl, self.lessonModel.introPic]]]];
image = image ? image : [UIImage imageNamed: @ "live_play_place"];
CGFloat actualHeight = (image.size.height * BoundWidth) / image.size.width;        //图片的宽高比
self.lessonImageView.image = image;
self.lessonImageView.contentScaleFactor = 2;
self.lessonImageView.contentMode = UIViewContentModeTop;
self.lessonImageView.clipsToBounds = YES;
```



