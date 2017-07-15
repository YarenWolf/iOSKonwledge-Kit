# UIApplication Delegate

* 很多原因会导致App受到干扰（打电话、内存不足），当App受到干扰时会产生一些系统事件。这时UIApplication会通知它的Delegate对象，让delegate代理来处理这些系统事件

* delegate事件包括：

  * 应用程序的生命周期（程序启动关闭）

  * 系统事件（来电话）

  * 内存警告

# UIApplication何时创建

> 我们都知道应用程序的主入口在main函数中，所以我们对main函数一探究竟

```
int main(int argc, char * argv[]) {
    @autoreleasepool {
        return UIApplicationMain(argc, argv, @"UIApplication",@"AppDelegate");
    }
}
```

* UIApplicationMain方法好像没见过哎。怎么办？查看Apple的官方文档：Help-&gt;Documentation and API Reference

* 参数说明

* principalClassName:String?,\_ delegateClassName:String?\) -&gt; Int32

| principalClassName（程序的主要类） | UIApplication类名或者子类的名称。 nil  或者 @"UIApplication" |
| :--- | :--- |
| principalClassName（代理类） | UIApplication的代理或者代理对象的名称 |

官方解释

This function instantiates the application object from the principal class and instantiates the delegate \(if any\) from the given class and sets the delegate for the application. It also sets up the main event loop, including the application’s run loop, and begins processing events. If the application’s`Info.plist`file specifies a main nib file to be loaded, by including the`NSMainNibFile`key and a valid nib file name for the value, this function loads that nib file.

Despite the declared return type, this function never returns. For more information on how this function behaves, see “[Expected App Behaviors](https://developer.apple.com/library/etc/redirect/xcode/content/1189/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/ExpectedAppBehaviors/ExpectedAppBehaviors.html#//apple_ref/doc/uid/TP40007072-CH3)” in[App Programming Guide for iOS](https://developer.apple.com/library/etc/redirect/xcode/content/1189/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007072).This function instantiates the application object from the principal class and instantiates the delegate \(if any\) from the given class and sets the delegate for the application. It also sets up the main event loop, including the application’s run loop, and begins processing events. If the application’s`Info.plist`file specifies a main nib file to be loaded, by including the`NSMainNibFile`key and a valid nib file name for the value, this function loads that nib file.

Despite the declared return type, this function never returns. For more information on how this function behaves, see “[Expected App Behaviors](https://developer.apple.com/library/etc/redirect/xcode/content/1189/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/ExpectedAppBehaviors/ExpectedAppBehaviors.html#//apple_ref/doc/uid/TP40007072-CH3)” in[App Programming Guide for iOS](https://developer.apple.com/library/etc/redirect/xcode/content/1189/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007072).、

* NSStringFromClass:把类名转换为字符串
* 好处：1、有提示功能；2、避免输入错误

* 根据官方文档可以看出：UIApplicationMain的底层实现：

  * 1、根据principalClassName传递的类名创建UIApplication对象
  * 2、创建UIApplication代理对象，给第一步初始化好的UIApplication对象设置代理
  * 3、开启主运行事件循环，处理事件、保持程序一直运行
  * 4、加载Info,plist文件，判断是否指定了main Xib，如果指定了就去加载

##### 因此我们知道UIApplication对象是在程序启动调用main函数的时候创建的，之后为UIApplication对象设了代理，应用程序启动好系统会让AppDelegate代理方法知道程序启动完毕

小实验

```
int main(int argc, char * argv[]) {
    @autoreleasepool {
        return UIApplicationMain(argc, argv, @"UIApplication",@"AppDelegate");
    }
}
```

这样应用程序启动也是没问题的

![](/assets/屏幕快照 2017-07-15 下午6.42.34.png)

