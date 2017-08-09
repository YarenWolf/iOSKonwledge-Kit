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

  ```
    [UIView animateWithDuration:0.25 animations:^{
          //CGAffineTransformMakeTranslation()会回到控件原始位置再去做动画
          self.redView.transform = CGAffineTransformMakeTranslation(200, 0);
          //CGAffineTransformTranslate()在控件当前的位置上做动画
          self.redView.transform = CGAffineTransformTranslate(self.redView.transform, 100, 200);
     }];
  ```

  * 旋转

  ```
    [UIView animateWithDuration:0.25 animations:^{
          //CGAffineTransformMakeRotation():回到控件的原始位置去旋转
          self.redView.transform = CGAffineTransformMakeRotation(M_2_PI);
          //CGAffineTransformRotate()：当前控件的当前位置上做旋转动画
          self.redView.transform = CGAffineTransformRotate(self.redView.transform, M_2_PI);
      }];
  ```

  * 缩放

  ```
  [UIView animateWithDuration:0.25 animations:^{
          //CGAffineTransformMakeScale()：回到控件原始位置在进行缩放
          self.redView.transform = CGAffineTransformMakeScale(0.5, 0.5);
          //CGAffineTransformScale():在控件当前的位置基础上进行缩放
          self.redView.transform = CGAffineTransformScale(self.redView.transform, 0.9, 0.9);
    }];
  ```



#### 注意：如果需要在控件最初始的位置坐动画，那么就用CGAffineTranformMake+动画类型\(Translation、Scale、Rotate\)



