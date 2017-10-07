##### UIStoryboardSegue:

UIStoryboard上每一根用来界面跳转的线，都是一个UIStoryboardSegue对象（简称Segue）

属性：

@property \(nonatomic, readonly\) NSString \*identifier;  唯一标识

@property \(nonatomic, readonly\) id sourceViewController;   来源控制器

@property \(nonatomic, readonly\) id destionViewController;   目标控制器

Segue 的类型

自动型：手动从来源控制器的按钮上拖线到目的控制器，然后点击相应的按钮控制器会自动跳转

手动型:按住control键，从来源控制器（控制器的顶部）拖线到目的控制器，在手动型的segue需要设置一个唯一标识。在恰当的时刻，使用perform方法执行跳转。

```
[self performSegueWithIdentifier:@"login" sender:nil];
```





##### performIdentifier的底层实现



* 根据标识符去storyboard中查找对应的线，创建UIStoryboardSegue对象
* 根据storyboard中线的关系，创建segue的来源控制器 segue.sourceVC = self
* 根据storyboard中线的关系，创建segue的的目的控制器 segue.destionVC = vc
* 通知来源控制器，我的线准备好了，可以进行跳转了。怎么通知？？调用\[self prepareForSegue:segue sender:nil\]
* \[segue perform\] 跳转



##### perform 方法的底层

1、获取导航控制器，self.navigationController

2、\[self.navigationController push:segue.destionVC animated:YES\]



