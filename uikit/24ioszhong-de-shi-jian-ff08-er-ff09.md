# 事件响应者链

实验1:

定义 BaseView，在里面实现方法touchBegan，监听当前哪个类调用了该方法。

在控制器的界面上加5个颜色不同的view，每个view自定义view去实现，因此在不同的view上的手势就可以由不同的view拦截到。

![](/assets/Simulator Screen Shot - iPhone 6s Plus - 2017-10-11 at 10.14.37.png)

```
//BaseView
#import "BaseView.h"

@implementation BaseView
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    NSLog(@"%@",[self class]);
}
```

结果：点击不同的View打印出不同的类名。





