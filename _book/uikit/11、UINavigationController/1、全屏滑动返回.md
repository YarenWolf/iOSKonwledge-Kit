### 自己动手写一个导航控制器全屏滑动返回效果

```
一、自定义导航控制器

目的：以后需要使用全屏滑动返回功能，就使用自己定义的导航控制器。


二、分析导航控制器侧滑功能

效果：导航控制器默认自带了侧滑功能，当用户在界面的左边滑动的时候，就会有侧滑功能。


分析：

1.导航控制器的view自带了滑动手势，只不过手势的触发范围只能在左边。

2.当用户在界面左边拖动，就会触发滑动手势方法，并且有滑动返回功能，说明系统手势触发的方法已经实现了滑动返回功能。

3.为什么说系统手势触发的方法已经实现了滑动返回功能？


原因：

创建滑动手势对象的时候，需要绑定监听者，当触发手势的时候会调用target的action。

UIPanGestureRecognizer *pan = [[UIPanGestureRecognizer alloc] initWithTarget:target action:action];

当用户在界面左边滑动，有滑动返回功能，这是因为触发手势了，调用target的action方法，说明action方法内部实现滑动返回功能，否则就不会有滑动返回效果。

三、实现全屏滑动功能分析

打印导航控制器自带的滑动手势，看下它的真实面目。

系统自带的滑动手势interactivePopGestureRecognizer

// self指向的导航控制器，在导航控制器的viewDidLoad方法打印


 NSLog(@"系统滑动手势:%@",self.navigationController.interactivePopGestureRecognizer);


   打印结果图片：
```

![](/assets/屏幕快照 2016-10-27 上午10.03.15.png)

```
由图中可知：

1.系统自带的手势是UIScreenEdgePanGestureRecognizer类型对象,屏幕边缘滑动手势

2.系统自带手势target是_UINavigationInteractiveTransition类型的对象

3.target调用的action方法名叫handleNavigationTransition:

分析：

UIScreenEdgePanGestureRecognizer，看名称就知道，这个手势的范围只能在屏幕的周边，就是因为这个手势，系统自带的滑动效果，只能实现侧边滑动。

四、如何实现全屏滑动功能

给自己的导航控制器，添加一个全屏的滑动手势，调用系统自带滑动手势的target的action方法，利用系统实现的滑动返回功能，加上自己全屏滑动手势，就有全屏滑动功能了。

问题：如何拿到系统自带的target对象?，action方法名已经知道，而且系统肯定在target对象实现了，只要拿到target对象，调用这个方法就行。

通过打印系统自带的滑动手势的代理，发现正好是_UINavigationInteractiveTransition对象，因此我猜测这个代理对象就是target对象,只要拿到它，就拿到系统自带滑动手势的target对象。

 // 打印系统自带滑动手势的代理对象

 NSLog(@"%@",self.navigationController.interactivePopGestureRecognizer.delegate);


导航控制器全屏滑动注意点:

1.禁止系统自带滑动手势使用。

2.只有导航控制器的非根控制器才需要触发手势，使用手势代理，控制手势触发。

3.应该封装起来供全局使用。

最后附上github地址：[](https://github.com/FantasticLBP/FullScreenNavigate)
```



