# iOS中的事件

1. 用户在使用App的时候会产生各种事件
2. 触摸事件、重力加速计事件、远程遥控事件
3. 只有继承自UIResponder才可以响应事件
4. UIView、UIApplication、UIViewController都可以响应事件

## UIResponder

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

* 


