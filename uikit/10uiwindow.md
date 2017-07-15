# UIWindow

> MainStoryBoard设置过，为什么程序启动过就可以看到一个界面？
>
> MainStoryBoard 如果在info.plist设置过，iOS会默认创建一个UIWindow对象，然后初始化一个控制器。然后将控制器赋值给UIWindow对象的根控制器，然后将窗口显示出来

# MakeKeyAndVisible的底层实现

* 1、self.window.hidden = NO;

* 2、appliction.window = self.window;

# UIWindow的层级

UIWindowLevelAlert &gt; UIWindowLevelStatusBar &gt; UIWindowLevelNormal \(NSInteger\)

# 模仿系统去创建VC

加载MainStoryBoard的实现：

* 1、创建窗口
* 2、加载MainStoryBoard,并且加载MainStoryBoard所指定的控制器
* 3、把加载出来的控制器作为窗口的控制器，并让窗口显示出来

手动模仿系统实现加载MainStoryBoard

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor yellowColor];
    
    UIStoryboard *sb = [UIStoryboard storyboardWithName:@"Main" bundle:nil];
    UIViewController *vc =  [sb instantiateInitialViewController];
    
    //instantiateInitialViewController ：默认加载storyboard中箭头所指向的控制器
    //instantiateViewControllerWithIdentifier:根据StoryBoardID 创建控制器
    self.window.rootViewController = vc;
    
    [self.window makeKeyAndVisible];
    
    
    return YES;
}
```



