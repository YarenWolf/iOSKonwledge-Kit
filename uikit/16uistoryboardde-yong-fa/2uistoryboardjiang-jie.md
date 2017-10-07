##### UIStoryboardSegue:

UIStoryboard上每一根用来界面跳转的线，都是一个UIStoryboardSegue对象（简称Segue）



属性：

@property \(nonatomic, readonly\) NSString \*identifier;  唯一标识

@property \(nonatomic, readonly\) id sourceViewController;   来源控制器

@property \(nonatomic, readonly\) id destionViewController;   目标控制器



Segue 的类型

手动型:手动拖线到目的控制器，然后点击相应的按钮控制器会自动跳转

自动型：按住control键，从来源控制器拖线到目的控制器，在手动型的segue需要设置一个唯一标识。在恰当的时刻，使用perform方法执行跳转。

```
[self performSegueWithIdentifier:@"login" sender:nil];
```



