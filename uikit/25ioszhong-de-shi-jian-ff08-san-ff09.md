# hitTest和pointInside方法

### hittest方法

* 就是用来寻找最合适的view
* 当一个事件传递给一个控件，就会调用这个控件的hitTest方法
* 点击了白色的view： 触摸事件 -&gt; UIApplication -&gt; UIWindow 调用 \[UIWindow hitTest\] -&gt; 白色view \[WhteView hitTest\]

实验1:

定义 BaseView，在里面实现方法touchBegan，监听当前哪个类调用了该方法。

定义KeyWindow，在里面实现hitTest方法，监听哪个类调用了该方法，用来追踪判断哪个view是最合适的view

在控制器的界面上加5个颜色不同的view，每个view自定义view去实现，因此在不同的view上的手势就可以由不同的view拦截到。

![](https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/Simulator%20Screen%20Shot%20-%20iPhone%206s%20Plus%20-%202017-10-11%20at%2010.14.37.png)

```
//KeyWindow

-(UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event{
    UIView *view = [super hitTest:point withEvent:event];
    NSLog(@"fittest->%@",view);
    return view;
}
```

结果：

点击了白色1:

```
2017-10-11 16:48:52.882547+0800 主流App框架[16295:358790] BrownView--hitTest withEvent
2017-10-11 16:48:59.646610+0800 主流App框架[16295:358790] GreenView--hitTest withEvent
2017-10-11 16:48:59.647145+0800 主流App框架[16295:358790] fittest-><UIView: 0x7f8f23406510; frame = (0 0; 414 736); autoresize = W+H; layer = <CALayer: 0x60c000221840>>
2017-10-11 16:48:59.647575+0800 主流App框架[16295:358790] BrownView--hitTest withEvent
2017-10-11 16:48:59.647702+0800 主流App框架[16295:358790] GreenView--hitTest withEvent
2017-10-11 16:48:59.647880+0800 主流App框架[16295:358790] fittest-><UIView: 0x7f8f23406510; frame = (0 0; 414 736); autoresize = W+H; layer = <CALayer: 0x60c000221840>>
```

点击了蓝色3:

```
2017-10-11 16:49:56.331024+0800 主流App框架[16295:358790] BrownView--hitTest withEvent
2017-10-11 16:49:56.331335+0800 主流App框架[16295:358790] BView--hitTest withEvent
2017-10-11 16:49:56.331617+0800 主流App框架[16295:358790] BlueView--hitTest withEvent
2017-10-11 16:49:56.331968+0800 主流App框架[16295:358790] YellowView--hitTest withEvent
2017-10-11 16:49:56.333206+0800 主流App框架[16295:358790] fittest-><BlueView: 0x7f8f23406f10; frame = (19 21; 240 128); autoresize = RM+BM; layer = <CALayer: 0x60c0002218c0>>
2017-10-11 16:49:56.333633+0800 主流App框架[16295:358790] BrownView--hitTest withEvent
2017-10-11 16:49:56.333762+0800 主流App框架[16295:358790] BView--hitTest withEvent
2017-10-11 16:49:56.333893+0800 主流App框架[16295:358790] BlueView--hitTest withEvent
2017-10-11 16:49:56.334005+0800 主流App框架[16295:358790] YellowView--hitTest withEvent
2017-10-11 16:49:56.334185+0800 主流App框架[16295:358790] fittest-><BlueView: 0x7f8f23406f10; frame = (19 21; 240 128); autoresize = RM+BM; layer = <CALayer: 0x60c0002218c0>>
2017-10-11 16:49:56.334644+0800 主流App框架[16295:358790] BlueView
```

那么看出来hitTest方法的作用就是找出最合适的view，那么我们可以指定任何事情的最合适的view为特定的view

实验2:

在KeyWindow中hitTest方法中返回BlueView，那么点击任何色块的view那么都会交给BlueView去处理事件。

```
//KeyWindow
-(UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event{
    return self.subviews.firstObject.subviews.firstObject;
}
```

结果：

```
2017-10-11 22:48:46.102793+0800 主流App框架[21498:749663] GreenView
2017-10-11 22:48:46.668595+0800 主流App框架[21498:749663] GreenView
```

因为事件的响应者链条就是当用户操作屏幕会产生一个事件，该事件被系统加入到事件队列中去，UIApplication对象会将事件队列中最早加入进去的事件传递给window，然后window找到最合适的view去处理事件。因此任何事件都会先通过KeyWindow对象去判断并找到最合适的view

## 2个重要的方法

* -\(BOOL\)pointInside:\(CGPoint\)point withEvent:\(UIEvent \*\)event： 用来判断触摸点是否在控件上

* -\(UIView \*\)hitTest:\(CGPoint\)point withEvent:\(UIEvent \*\)event： 用来判断控件是否接受事件以及找到最合适的view

## 模仿系统实现找出最合适的view

```
//KeyWindow

/**
 模仿系统实现寻找最合适的view步骤
 1、控件接收事件
 2、触摸点在自己身上
 3、从后往前遍历子控件，重复前面2个步骤
 4、如果没有符合条件的子控件，那么就自己最合适

 */

-(UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event{
    if (self.userInteractionEnabled == NO || self.hidden == YES || self.alpha <= 0.01) {
        return nil;
    }

    if (![self pointInside:point withEvent:event]) {
        return nil;
    }

    for (NSUInteger index = self.subviews.count - 1; index >= 0; index--) {
        CGPoint childViewPoint = [self convertPoint:point toView:self.subviews[index]];
        UIView *fitestView = [self.subviews[index] hitTest:childViewPoint withEvent:event];
        if (fitestView) {
            return fitestView;
        }
        return nil;
    }

    return self;
}
```

给出 一个Demo地址：[https://github.com/FantasticLBP/BlogDemos/tree/master/模仿系统找出事件的最佳响应者](https://github.com/FantasticLBP/BlogDemos/tree/master/模仿系统找出事件的最佳响应者 "模仿系统找出事件的最佳响应者")

实验：

在控制器（ViewController）的view上先添加一个UIButton，再添加一个自定义的UIView\(ShelterView\),盖在button的上面。

需求：点击ShelterView上的点，如果点也在UIButton范围上则交给UIButton处理事件，如果不在UIButton上则交给ShelterView处理，如果点击屏幕上除了ShelterView之外的点则交给控制器的view处理。

```
//ViewController
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    NSLog(@"viewController->%s",__func__);
}


//ShelterView
#import "ShelterView.h"

@implementation ShelterView

-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    NSLog(@"%s",__func__);
}

-(UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event{
    NSLog(@"%s",__func__);
    /**
     需求：不管点击按钮还是view都交给button处理
    思路：在view的hitTest方法中寻找最合适的view，那么返回nil告诉系统view不是最合适的view，那么系统则认为按钮是最合适的view
    return nil;
     */

    //需求，在view上点击，如果点击范围在button上则由button进行处理事件；否则交给view处理事件

    UIView *button = nil;
    for (UIView *subView in self.superview.subviews) {
        //判断事件的点是否在按钮上
        if ([subView isKindOfClass:[UIButton class]]) {
            button =subView;
        }


        CGPoint btnPoint = [self convertPoint:point toView:button];
        if ([button pointInside:btnPoint withEvent:event]) {
            return button;
        }else{
            //此时代表事件触摸点不在button上，但是也不能写nil，写nil的话点击屏幕上的其他地方系统会寻找最合适的view，此时返回nil（ return nil;），则代表view不是最合适的view,那么此时点击屏幕上除了按钮之外的区域，最合适的view就是控制器上面的view
            return [super hitTest:point withEvent:event];
        }
    }
    return nil;
}

@end
```

要看完整Demo，地址为：[https://github.com/FantasticLBP/BlogDemos/tree/master/hitTest的神奇效果（一）](https://github.com/FantasticLBP/BlogDemos/tree/master/hitTest的神奇效果（一） "hitTest的神奇效果")

