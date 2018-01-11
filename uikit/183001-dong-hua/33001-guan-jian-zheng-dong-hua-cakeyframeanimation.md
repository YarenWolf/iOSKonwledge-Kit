<h1>关键帧动画</h1>

呜呜呜



> iOS 开发中经常涉及到很多的动画效果开发，有些动画只需要一些简单的操作就可以完成。举个例子普通的平移、旋转、缩放。但是有些动画需要多个变化点（类似于拐点）。举个例子就想一个 UIImageView 从 a-&gt;b-&gt;c-&gt;d 四个不同的位置用一个动画完成，这时候用关键帧动画再合适不过。




<h2>一、先看个实验</h2>


<p>需要模仿系统长按App实现抖动效果，尝试几种实现办法</p>


<p>方法1:（CABasciAnimation）</p>

```objective-c
CABasicAnimation * animation = [CABasicAnimation animation];
animation.keyPath = @ "transform.rotation.z";
animation.duration = 0.2;
animation.fromValue = @(angle2Rad(5));
animation.toValue = @(angle2Rad(-5));
animation.repeatCount = HUGE_VALF;
animation.autoreverses = YES;
[self.imageView.layer addAnimation: animation forKey: nil];
```


<p>方法2：（CAKeyframeAnimation）</p>

```objective-c


```





