
```
@property (nonatomic, assign, getter = isContentHorizontalCenter) BOOL contentHorizontalCenter;

- (void)setContentHorizontalCenter:(BOOL)contentHorizontalCenter {
    _contentHorizontalCenter = contentHorizontalCenter;
    if (!_contentHorizontalCenter) {
        self.verticalOffset = 0;
        self.spacing = 0;
    }
    if (self.superview) {
        [self layoutSubviews];
    }
}
```
