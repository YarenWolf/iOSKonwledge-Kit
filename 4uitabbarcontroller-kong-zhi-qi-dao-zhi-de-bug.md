# UITabBarController 导致的 Bug

1、某些需求需要获取 App Window的 主控制器，对于我的项目就是 UITabBarController ，之前不仔细的一种写法，写完后要上线给测试人员安装了 App 去测试



Bug版本

```
MainViewController *mainVC = [UIApplication sharedApplication].keyWindow.rootViewController;
HLNavigationController *naviVC = mainVC.viewControllers.firstObject;
            
UIViewController *mainViewController = (UIViewController *)naviVC.viewControllers.firstObject;
            
CGRect mainRect = mainViewController.view.frame;
```

修复的版本

```
AppDelegate *appdelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;    
MainViewController *mainVC = (MainViewController *)appdelegate.window.rootViewController;
HLNavigationController *naviVC = mainVC.viewControllers.firstObject;
CGRect mainRect = naviVC.view.frame;
```



