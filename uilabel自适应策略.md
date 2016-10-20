###小屏幕时cell上需要显示很多信息
    此时label宽度定死，分2行显示，由上到小自适应。
```
 self.time.numberOfLines = 0;
 [self.time sizeToFit];
 self.time.textAlignment = NSTextAlignmentCenter;
```


###根据内容确定label的宽度
```
NSMutableParagraphStyle *paragraphstyle=[[NSMutableParagraphStyle alloc]init];
 paragraphstyle.lineBreakMode=NSLineBreakByCharWrapping;
 NSDictionary *dic=@{NSFontAttributeName:self.name.font,NSParagraphStyleAttributeName:paragraphstyle.copy};
 CGRect rect=[cellDo[@"memberName"] boundingRectWithSize:CGSizeMake(self.frame.size.width, self.frame.size.height) options:NSStringDrawingUsesLineFragmentOrigin attributes:dic context:nil];
 self.nameLabelWIdth.constant = rect.size.width;
```