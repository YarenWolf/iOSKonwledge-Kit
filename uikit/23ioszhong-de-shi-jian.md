# iOS中的事件

* 用户在使用App的时候会产生各种事件
* 触摸事件、重力加速计事件、远程遥控事件
* 只有继承自UIResponder才可以响应事件
* UIView、UIApplication、UIViewController都可以响应事件
* ## UIResponder
* UIResponder内部提供了一些方法处理事件

```
//触摸事件
-(void)touchBegan:(NSSet *)touches withEvent:(UIEvent *)event;
-(void)touchMoved:(NSSet *)touches withEvent:(UIEvent *)event;
-(void)touchEnded:(NSSet *)touches withEvent:(UIEvent *)event;
-(void)touchCanceled:(NSSet *)touches withEvent:(UIEvent *)event;

//加速计事件
-(void)motionBegan:(UIEventSubtype)motion withEvent:(UIEvent *)event;
-(void)motionEnded:(UIEventSubtype)motion withEvent:(UIEvent *)event;
-(void)motionCanceled:(UIEventSubtype)motion withEvent:(UIEvent *)event;

//远程控制事件
-(void)remoteControlReceivedWithEvent:(UIEvent *)event;
```

# 事件的产生和传递

* 发生触摸事件后，系统会将该事件加入到一个由UIApplication管理的事件队列中去
* UIApplication会从事件队列中取出最前面的事件，并将事件分发下去以便处理，通常先分发事件给应用程序的主窗口（keyWindow）
* 主窗口会在视图层次结构中寻找到一个最合适的视图来处理触摸事件，这也是整个事件处理过程中最重要的一步。



找到合适的视图控件后，就会调用视图控件的touch方法来做具体的事件处理逻辑



## UIView不接收事件的3种情况

1. 不接收用户交互。view.userInteractionEnabled = NO
2. 隐藏。view.hidden = YES
3. 透明度很低。view.alpha = 0.0 ~ 0.01



注意：UIImageView的userInteractionEnabled默认为NO，因此UIImageView及其它上面的子控件默认是不能接受触摸事件的。



