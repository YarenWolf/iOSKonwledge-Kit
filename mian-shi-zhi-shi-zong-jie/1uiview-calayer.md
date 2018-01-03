# CALayer 和 UIView

区别： 1.UIView可以响应事件，CALayer不可以响应事件；CALayer继承的是NSObject,UIView继承自UIResponder

2.UIView着重于内容管理，CALayer着重于内容绘制.

3.一个CALayer的frame是由其anchorPoint, position, bounds, transform共同决定的, 而一个UIView的的frame只是简单地返回CALayer的frame, 同样UIView的center和bounds也只是简单返回CALayer的Position和Bounds对应属性.

关系：

UIView是CALayer的CALayerDelegate, 在代理方法内部\[UIView\(CALayerDelegate\) drawLayer:inContext\]调用UIView的drawRect方法, 从而绘制出UIView的内容. UIView的显示内容由内部的CALayer:display方法来实现.

![](/assets/屏幕快照 2017-09-11 下午4.11.34.png)

结果：打印视图的宽高结果不会是100，100；而是200，200.

CALayer功能：由于上面说了着重于绘制，所以，关于视图的动画，变形也就主要由它来做

功能如下：

1，阴影，圆角，带颜色的边框 2，3D变换 3，非矩形范围 4，多级非线性动画。 好吧，接下来主要说一下我认为比较实用的一些属性。 cornerRadius position contents borderWidth/borderColor maskToBounds

contentRect

anchorPoint

什么是anchorPoint?

好吧，它就是一颗图钉，按住layer，让layer围着它转。这里需要说一点的是，这可图钉直接影响着view的位置。所以一般来说，都是先设置anchorePoint之后再设置frame.从而可以简单的让它不变形。

2、  timer的invalidate到底该在什么地方调用，因为有退出vc再回来的场景



 因为timer本身被注册到runloop了，如果你再强引用一次就会导致无法释放

3、gcd往一个队列里面插入一个任务 比如说已经插入了三个 现在想把新任务放在第一个后面执行

