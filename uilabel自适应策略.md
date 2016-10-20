###label在大屏幕和小屏幕上的处理原则：

 小屏幕时cell上需要显示很多信息。此时label宽度定死，分2行显示，由上到小自适应。
 大屏幕上的时候需要从左到右展示，第一个label的宽度由内容决定，第二个label的宽度由整个屏幕宽度减去目前已经计算好的几个宽度，剩下的展示就好了。

```

 self.time.numberOfLines = 0;

 [self.time sizeToFit];

 self.time.textAlignment = NSTextAlignmentCenter;

```

![](/assets/屏幕快照 2016-10-20 下午2.37.30.png)

![](/assets/屏幕快照 2016-10-20 下午2.37.40.png)



###根据内容确定label的宽度

```

NSMutableParagraphStyle *paragraphstyle=[[NSMutableParagraphStyle alloc]init];

 paragraphstyle.lineBreakMode=NSLineBreakByCharWrapping;

 NSDictionary *dic=@{NSFontAttributeName:self.name.font,NSParagraphStyleAttributeName:paragraphstyle.copy};

 CGRect rect=[cellDo[@"memberName"] boundingRectWithSize:CGSizeMake(self.frame.size.width, self.frame.size.height) options:NSStringDrawingUsesLineFragmentOrigin attributes:dic context:nil];

 self.nameLabelWIdth.constant = rect.size.width;

```
