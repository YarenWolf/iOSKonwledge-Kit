# UIViewController  view的生命周期

##### 实验

```
@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    NSLog(@"%s",__func__);
}

-(void)viewWillAppear:(BOOL)animated{
    [super viewWillAppear:animated];
    NSLog(@"%s",__func__);
}

-(void)viewWillLayoutSubviews{
    [super viewWillLayoutSubviews];
    NSLog(@"%s",__func__);
}

-(void)viewDidLayoutSubviews{
    [super viewDidLayoutSubviews];
    NSLog(@"%s",__func__);
}

-(void)viewDidAppear:(BOOL)animated{
    [super viewDidAppear:animated];
    NSLog(@"%s",__func__);
}

-(void)viewWillDisappear:(BOOL)animated{
    [super viewWillDisappear:animated];
    NSLog(@"%s",__func__);
}

-(void)viewDidDisappear:(BOOL)animated{
    [super viewDidDisappear:animated];
    NSLog(@"%s",__func__);
}

-(void)touchesEnded:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    WebViewController *vc = [[WebViewController alloc] init];
    vc.urlStr = @"http://www.baidu.com";
    [self.navigationController pushViewController:vc animated:YES];
}

@end
```

##### 结果

**2017-07-27 09:21:06.846 test\[942:129755\] -\[ViewController viewDidLoad\]**

**2017-07-27 09:21:06.847 test\[942:129755\] -\[ViewController viewWillAppear:\]**

**2017-07-27 09:21:06.848 test\[942:129755\] -\[ViewController viewWillLayoutSubviews\]**

**2017-07-27 09:21:06.848 test\[942:129755\] -\[ViewController viewDidLayoutSubviews\]**

**2017-07-27 09:21:06.848 test\[942:129755\] -\[ViewController viewWillLayoutSubviews\]**

**2017-07-27 09:21:06.848 test\[942:129755\] -\[ViewController viewDidLayoutSubviews\]**

**2017-07-27 09:21:06.850 test\[942:129755\] -\[ViewController viewDidAppear:\]**

**2017-07-27 09:21:44.817 test\[942:129755\] -\[ViewController viewWillDisappear:\]**

**2017-07-27 09:21:46.544 test\[942:129755\] -\[ViewController viewDidDisappear:\]**



##### 结论



* ARC:控制器view的生命周期调用顺序：**viewDidLoad-&gt;viewWillAppear-&gt;viewWillLayoutSubview-&gt;viewDidLayoutSubviews-&gt;viewDidAppear-&gt;viewWillDisappear-&gt;viewDidDisappear**
* 在非ARC模式下view的生命周期还会有2个方法
* viewWillUnload：控制器的view将要销毁
* viewDidUnload：控制器的view已经销毁：释放一些资源，晴空一些没必要的数据

![](/assets/2017-07-27-01.png)



