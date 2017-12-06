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



