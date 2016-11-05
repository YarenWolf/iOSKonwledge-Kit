1.在工程中找到 info.plist 文件，点击“+”号，选择 View controller-based status bar appearance 并设为 NO
2.在 AppDelegate.m添加一段代码：
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
 //添加的代码
 [[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent animated:YES];
 return YES;
}

这里的状态显示的是白色。
这里我们要清楚 ：
状态栏的字体为黑色： UIStatusBarStyleDefault
状态栏的字体为白色： UIStatusBarStyleLightContent
根据个人的需求选择黑色或白色。
