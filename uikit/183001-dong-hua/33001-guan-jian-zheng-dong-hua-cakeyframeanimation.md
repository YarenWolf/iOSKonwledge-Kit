# 关键帧动画面

> iOS 开发中经常涉及到很多的动画效果开发，有些动画只需要一些简单的操作就可以完成。举个例子普通的平移、旋转、缩放。但是有些动画需要多个变化点（类似于拐点）。举个例子就想一个 UIImageView 从 a-&gt;b-&gt;c-&gt;d 四个不同的位置用一个动画完成，这时候用关键帧动画再合适不过。

## 一、先看个实验

完成以下效果有几种办法？

![](/assets/2018-01-11 23_41_53.gif)

### 方法一

```
CABasicAnimation * animation = [CABasicAnimation animation];
animation.keyPath = @ "transform.rotation.z";
animation.duration = 0.2;
animation.fromValue = @(angle2Rad(5));
animation.toValue = @(angle2Rad(-5));
animation.repeatCount = HUGE_VALF;
animation.autoreverses = YES;
[self.imageView.layer addAnimation: animation forKey: nil];
```

### 方法二

```
CAKeyframeAnimation * animation = [CAKeyframeAnimation animation];

animation.keyPath = @ "transform.rotation.z";


//方法1
animation.values = @[@(angle2Rad(-5)), @(angle2Rad(5)), @(angle2Rad(-5))];
animation.repeatCount = HUGE_VALF;

animation.duration = 0.5;
[self.imageView.layer addAnimation: animation forKey: nil];
```

### 方法三

```
CAKeyframeAnimation * animation = [CAKeyframeAnimation animation];

animation.keyPath = @ "transform.rotation.z";
//方法2
//  animation.values = @[@(angle2Rad(-5)),@(angle2Rad(5))];
//  animation.autoreverses = YES;
animation.repeatCount = HUGE_VALF;

animation.duration = 0.5;
[self.imageView.layer addAnimation: animation forKey: nil];
```

### 说明

* @\[@\(angle2Rad\(-5\)\),@\(angle2Rad\(5\)\),@\(angle2Rad\(-5\)\)\] 如果是2个值，动画执行完全按照 values 数组里面的值去执行，刚开始从 -5 开始执行旋转动画到 5，然后马上回归到初始位置再次执行动画。所以从 -5 到 5 的时候不是执行动画过去的，会有抽搐的效果，解决办法有2个。方法1:设置动画的 autoreverses 属性为 YES，让系统自动完成从 5 回去到 -5 的动画。 方法2:手动添加回去的动画 从 5 到 -5
* 关键帧动画主要有2种实现思路。一种是 values ，该参数为一个数组记录了关键帧的信息。另一种是 path 属性，是一个 CGPathRef 类型的属性，我们可以通过 UIBezierPath 创建一个路径，然后传递 path.CGPath 。动画会绕着 path 做一个动画效果



## 二、关键帧动画





### path 动画

![](/assets/2018-01-12 00_24_02.gif)

```
CAKeyframeAnimation * animation = [CAKeyframeAnimation animation];
animation.keyPath = @ "position";

UIBezierPath * path = [UIBezierPath bezierPath];
[path moveToPoint: CGPointMake(50, 50)];
[path addLineToPoint: CGPointMake(300, 300)];

animation.path = path.CGPath;
animation.duration = 1;
animation.repeatCount = HUGE_VALF;
animation.autoreverses = YES;

[self.imageView.layer addAnimation: animation forKey: nil];
```



### values 动画

```
CAKeyframeAnimation * animation = [CAKeyframeAnimation animation];

animation.keyPath = @ "transform.rotation.z";

animation.values = @[@(angle2Rad(-5)), @(angle2Rad(5))];
animation.autoreverses = YES;
animation.repeatCount = HUGE_VALF;

animation.duration = 0.5;
[self.imageView.layer addAnimation: animation forKey: nil];
```



