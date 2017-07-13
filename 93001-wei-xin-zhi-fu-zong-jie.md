# 微信支付总结

* 查看微信支付Demo，看到一句代码用来判断iphone和ipad的界面显示

```
 if ([[UIDevice currentDevice] userInterfaceIdiom] == UIUserInterfaceIdiomPhone) {
        self.viewController = [[[SendMsgToWeChatViewController alloc] initWithNibName:@"ViewController_iPhone" bundle:nil] autorelease];
    } else {
        self.viewController = [[[SendMsgToWeChatViewController alloc] initWithNibName:@"ViewController_iPad" bundle:nil] autorelease];
    }
```

![](/assets/1377427-359d12bff546cf2f.png)

图片引自：[http://www.jianshu.com/p/1c1c834b6d52](http://www.jianshu.com/p/1c1c834b6d52)

#### App端接入微信支付的步骤

* 去微信的平台申请商家的信息，需要Bundle id等信息
* 下载微信支付Demo运行查看开发流程
* 初次下载的微信支付Demo编译会报错，首先解决错误。![](/assets/屏幕快照 2017-07-04 下午3.27.04.png)解决方案：

* 选中Target，在Build Phases中Link Binary with Libraries中搜索CFNetwork.framework

* 再次运行会看到模拟器上熟悉的地球图片，然后又出错

![](/assets/屏幕快照 2017-07-04 下午3.30.58.png)

* 解决办法:选中Targets-&gt;Build Settings-&gt;Other Linker Flags-&gt;点击加入-all\_load和-Objc

* 这样微信支付Demo就顺利跑起来了

总结：其实这些错误信息在下载下来的资源包中的README.txt中早已写明了

正式开发步骤：

* 我们自己的工程中引入微信支付SDk
* 在Targets-&gt;Build Setting-&gt;Link Binary With Libraries添加所需要的依赖库
  * SystemConfiguration.framework
  * libz.tbd
  * libsqlite3.0.tbd
  * CoreTelephony.framework
  * QuartzCore.framework
* 设置URL Scheme

  * 在微信平台上注册App信息的时候会给我们的App分配一个唯一标识，然后将这个唯一标识填写在下图的这个地方![](/assets/QQ20170704-154424@2x.png)

* 在AppDelegate,m中注册AppID

```
    [WXApi registerApp:WX_APPID withDescription:@"微信支付"];
```

* 同时在AppDelegate.m中写

```
//微信支付
- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url {
    return  [WXApi handleOpenURL:url delegate:[WXApiManager sharedManager]];
}

- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    return [WXApi handleOpenURL:url delegate:[WXApiManager sharedManager]];
}
```

* 在需要支付功能的VC中设置代码，并支付

```

```



