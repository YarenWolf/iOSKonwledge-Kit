### 根据内容确定label的宽度

```
NSMutableParagraphStyle *paragraphstyle=[[NSMutableParagraphStyle alloc]init];

 paragraphstyle.lineBreakMode=NSLineBreakByCharWrapping;

 NSDictionary *dic=@{NSFontAttributeName:self.name.font,NSParagraphStyleAttributeName:paragraphstyle.copy};

 CGRect rect=[cellDo[@"memberName"] boundingRectWithSize:CGSizeMake(self.frame.size.width, self.frame.size.height) options:NSStringDrawingUsesLineFragmentOrigin attributes:dic context:nil];

 self.nameLabelWIdth.constant = rect.size.width;
```



