# UINavigationController

* UIBarButtonItem如果设置了图片，那么默认的话系统会把图片颜色设置为蓝色

```
    UIImage *image = [UIImage imageNamed:@"test"];
    self.navigationItem.leftBarButtonItem = [[UIBarButtonItem alloc] initWithImage:image style:UIBarButtonItemStyleDone target:nil action:nil]; 
```

![](/assets/屏幕快照 2017-07-27 上午9.02.12.png)



* 如果自己想需要图片本来的颜色，有2种办法
* 设置UIImage的UIImageRenderingMode

```
 UIImage *image = [UIImage imageNamed:@"test"];
 image = [image imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 self.navigationItem.leftBarButtonItem = [[UIBarButtonItem alloc] initWithImage:image style:UIBarButtonItemStyleDone target:nil action:nil];
```

* 通过在Assets.xcassets中设置图片的RenderingMode

![](/assets/屏幕快照 2017-07-27 上午9.06.00.png)

