# UIApplication

作用：

* 设置应用程序角标

```
     //ios10之前
     UIApplication *app = [UIApplication sharedApplication];
    UIUserNotificationSettings *setting = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeBadge categories:nil];
    [app registerUserNotificationSettings:setting];
    app.applicationIconBadgeNumber = 10;
    //ios10之后
      UIApplication *app = [UIApplication sharedApplication];
    UNUserNotificationCenter *setting = [UNUserNotificationCenter currentNotificationCenter];
    [setting requestAuthorizationWithOptions:UNAuthorizationOptionBadge completionHandler:^(BOOL granted, NSError * _Nullable error) {
        if (!error) {
            NSLog(@"请求授权成功");
        }
    }];
    app.applicationIconBadgeNumber = 10;
```

* 设置应用程序状态栏

```
    UIApplication *app = [UIApplication sharedApplication];
    app.statusBarHidden = YES;
```

* 虽然设置了隐藏状态栏，但是还是不隐藏，为什么？点击statusBarHidden属性进去看看苹果的解释![](/assets/屏幕快照 2017-07-15 下午4.55.31.png)如果你的应用程序使用默认的基于ViewController来管理的状态栏系统的话，通过UIApplication设置statusBarHidden不起作用![](/assets/屏幕快照 2017-07-15 下午5.03.24.png)

原来ios9开始，状态栏默认交给每个VC去处理，看到系统ViewController里面有2个方法控制状态栏的颜色与显示隐藏的属性

```
-(BOOL)prefersStatusBarHidden{
    return YES;
}
```

如果想统一管理App的状态栏怎么办？直接在info.plist中处理

告诉系统App的状态栏不要交给控制器去管理![](/assets/屏幕快照 2017-07-15 下午5.10.43.png)所以此时在控制器中设置prefersStatusBarHidden属性已经不起作用了。

必须通过UIApplication去管理状态栏。还可以设置动画效果。

```
[[UIApplication sharedApplication] setStatusBarHidden:YES withAnimation:UIStatusBarAnimationSlide];
```

* 显示网络加载状态（某些App顶部会有转圈效果）

```
    [UIApplication sharedApplication].networkActivityIndicatorVisible = YES;
```

* 打开网页（打电话、发短信、邮件）



