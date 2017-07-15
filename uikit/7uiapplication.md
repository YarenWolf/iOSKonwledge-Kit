# UIApplication

作用：

* 设置应用程序角标

```
     //ios8之前
     UIApplication *app = [UIApplication sharedApplication];
    UIUserNotificationSettings *setting = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeBadge categories:nil];
    [app registerUserNotificationSettings:setting];
    app.applicationIconBadgeNumber = 10;
    //ios8之后
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
* 显示网络加载状态（某些App顶部会有转圈效果）
* 打开网页（打电话、发短信、邮件）



