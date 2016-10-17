###ios8 毛玻璃Api---UIBlurEffect

```
let blurEffect:UIBlurEffect = UIBlurEffect(style: UIBlurEffectStyle.light)
 let effectView:UIVisualEffectView = UIVisualEffectView(effect:blurEffect)
 effectView.frame = self.bgImage.frame
 self.bgImage.addSubview(effectView)
 effectView.alpha = 0.8
```