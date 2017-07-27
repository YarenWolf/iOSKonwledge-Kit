# Load方法

```
//Appdelegate
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    NSLog(@"%s",__func__);
    // Override point for customization after application launch.
    return YES;
}

//Person
+(void)load{
    NSLog(@"%s",__func__);
}
```

* load方法调用时机：iOS工程先调用load方法再调用App的didFinishLaunchingWithOptions方法



