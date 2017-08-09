# 动画

* 最原始的动画

```
[UIView animateWithDuration:0.25 animations:^{
        CGRect frmae = self.redView.frame;
        frmae.origin.x += 200;
        self.redView.frame = frmae;
}];
```

* transform动画
  * 平移
  * 旋转
  * 缩放





