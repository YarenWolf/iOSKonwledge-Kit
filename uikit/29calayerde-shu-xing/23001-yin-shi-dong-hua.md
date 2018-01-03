# 隐式动画

一、概念

* 每个 UIView 内部都默认关联着一个 CALayer ，我们称这个 Layer 为 Root Layer （根层）
* 所有的非 Root Layer ，也就是手动创建的 Layer 对象，都存在着隐式动画
* 当对非 Root Layer 的某些属性做修改时，会产生一些动画效果，这样的属性叫做 Animatable Properties \(可动画属性\):bounds、BackgroundColor、position



