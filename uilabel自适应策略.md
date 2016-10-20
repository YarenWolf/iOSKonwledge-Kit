###小屏幕时cell上需要显示很多信息
    此时label宽度定死，分2行显示，由上到小自适应。
```
 self.time.numberOfLines = 0;
 [self.time sizeToFit];
 self.time.textAlignment = NSTextAlignmentCenter;
```