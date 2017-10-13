# 事件响应者链

实验1:

定义 BaseView，在里面实现方法touchBegan，监听当前哪个类调用了该方法。

在控制器的界面上加5个颜色不同的view，每个view自定义view去实现，因此在不同的view上的手势就可以由不同的view拦截到。

![UI效果图](https://github.com/FantasticLBP/iOSKonwledge-Kit/blob/master/assets/Simulator Screen Shot - iPhone 6s Plus - 2017-10-11 at 10.14.37.png?raw=true)

```
//BaseView
#import "BaseView.h"

@implementation BaseView
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    NSLog(@"%@",[self class]);
}
```

结果：点击不同的View打印出不同的类名。

结论：

* 触摸事件是从父控件传递到子控件的。
* 点击了绿色（图上的2级）的view：UIApplication-&gt; UIWindow -&gt; UIViewController的view -&gt; 绿色的view
* 点击了蓝色（图上的3级）的view：UIApplication-&gt; UIWindow -&gt; UIViewController的view -&gt; 红棕色的view -&gt; 蓝色的view
* 点击了黄色（图上的4级）的view：UIApplication -&gt; UIWindow -&gt; UIViewController的view -&gt; 红棕色的view -&gt; 蓝色的view -&gt; 黄色的view

注意：如果父控件不能接收触摸事件，那么这个父控件的子控件也不能接收触摸事件

#### 如何找到最合适的控件来接收触摸事件？

* 自己能否接收触摸事件？
* 触摸点是否在自己身上？
* 从后往前遍历子控件，重复前面2个步骤
* 如果没有符合条件的子控件，那么就自己最适合处理



# 事件响应原理

产生的touch方法的默认做法是将事件顺着响应者链条向上传递，将事件交给上一个响应者处理。

#### 响应者链条

![响应者链条](https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/响应者链条.png)

#### 事件传递的完整过程

1. 先将事件对象由上往下传递（父控件传递给子控件），找到最合适的控件来处理
2. 调用最合适控件的touch方法
3. 如果调用了\[super touches...\]方法就会将事件顺着响应者链条向上传递，传递给上一个响应者
4. 接着就会调用上一个响应者的touches...方法

#### 事件响应者

##### 如何判断该控件的上一个响应者？

1. 如果当前这个view是控制器的view，那么上一个响应者就是控制器
2. 如果当前这个view不是控制器的view，那么上一个响应者就是父控件。

事件传递给UIApplication后如果不处理的话，该事件会销毁掉。

控制器view上的子控件的touch...方法如果子控件不处理那么都会顺着响应者链条向上传递给上一层响应者对象，比如可以交给控制器处理。



