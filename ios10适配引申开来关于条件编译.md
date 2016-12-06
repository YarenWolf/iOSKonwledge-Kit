###iOS10 CAAnimationDelegate适配引申到条件编译

本文将围绕2个问题展开。
- iOS10 CAAnimationDelegate适配
- 条件编译


1、iOS10 CAAnimationDelegate适配
![](/assets/屏幕快照 2016-12-06 上午10.13.22.png)

原因是动画的代理没有遵循协议。解决如下：
![](/assets/屏幕快照 2016-12-06 上午10.15.00.png)

以为万事大吉？在X-code7打开运行编译报错。ios10之前写动画的协议方法，不需要去遵循系统的动画代理。

2、此问题引申开来的条件编译。利用__IPHONE_OS_VERSION_MAX_ALLOWED系统宏进行条件编译

在@interface前加上。

```
#if __IPHONE_OS_VERSION_MAX_ALLOWED >= __IPHONE_10_0
@interface ViewController () <CAAnimationDelegate>
#else
@interface ViewController ()
#endif
```















