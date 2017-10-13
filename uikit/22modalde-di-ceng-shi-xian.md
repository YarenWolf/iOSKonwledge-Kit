# Modal

> 控制器之间的跳转出了push之外还有modal。表现为从屏幕底部往上钻，直到覆盖上一界面为止。

### 为了研究Modal的工作原理，我们打算手动开始模仿系统的Modal效果实现。

#### 实验1

```
//FirstViewController

    ViewController *vc = [ViewController new];
    vc.view.backgroundColor = [UIColor brownColor];

    //模仿系统的modal
    [[UIApplication sharedApplication].keyWindow addSubview:vc.view];
    vc.view.transform = CGAffineTransformMakeTranslation(0, self.view.frame.size.height);

    [UIView animateWithDuration:0.25 animations:^{
        vc.view.transform = CGAffineTransformIdentity;
    } completion:^(BOOL finished) {
        [self.view removeFromSuperview];
    }];

//ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    [self.view addSubview:self.button];
}

-(void)dismiss{
    [self dismissViewControllerAnimated:YES completion:nil];
}

#pragma mark -- lazy load
-(UIButton *)button{
    if (!_button) {
        _button = [UIButton buttonWithType:UIButtonTypeCustom];
        _button.frame = CGRectMake(100, 100, self.view.frame.size.width - 200, 50);
        _button.backgroundColor = [UIColor redColor];
        [_button setTitle:@"返回" forState:UIControlStateNormal];
        [_button setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];
        [_button addTarget:self action:@selector(dismiss) forControlEvents:UIControlEventTouchUpInside];
        _button.userInteractionEnabled = YES;

    }
    return _button;
}
```

结果：

* 模仿系统Modal方法做出来了从底部弹出一个界面占满了屏幕，且之前的view从窗口上移除掉。
* 但是在弹出的控制器的view上有一个按钮，点击按钮发现不响应按钮的点击事件

#### 实验 2

```
//FirstViewController

//...

@interface FirstViewController ()

@property (nonatomic, strong) ViewController *vc;

@end

@implementation FirstViewController
- (IBAction)dismiss:(id)sender {
    self.vc = [ViewController new];
    self.vc.view.backgroundColor = [UIColor brownColor];

    //模仿系统的modal
    [[UIApplication sharedApplication].keyWindow addSubview:self.vc.view];
    self.vc.view.transform = CGAffineTransformMakeTranslation(0, self.view.frame.size.height);

    [UIView animateWithDuration:0.25 animations:^{
        self.vc.view.transform = CGAffineTransformIdentity;
    } completion:^(BOOL finished) {
        [self.view removeFromSuperview];
    }];
}

//ViewController的条件不变
```

结果：

* 模仿系统Modal方法做出来了从底部弹出一个界面占满了屏幕，且之前的view从窗口上移除掉。

* 但是在弹出的控制器的view上有一个按钮，点击按钮发现响应按钮的点击事件

问题？那么被模打出的控制器系统肯定是强引用了，那么被谁强引用了？

#### 实验3

```
//FirstViewController

    self.vc = [ViewController new];
    self.vc.view.backgroundColor = [UIColor brownColor];
    [self presentViewController:self.vc animated:YES completion:^{
        NSLog(@"%@",self.presentedViewController);    //<ViewController: 0x7face6811a20>
    }];
```

结果：

打印 出的self.presentedViewController就是ViewController。

#### 结论：

1. 只要控制器的view显示出来，那么这个控制器就一定不能被销毁
2. 一个控制器的view可以存在，但是这个控制器不一定存在。（比如一个控制器的view被添加到window上，那么控制器可能被销毁了，但是view被window对象的subViews数组强引用着，此时存在一个很严重的问题就是，虽然用户可以看到这个界面，但是这个界面的控制器不存在，也就是所有的界面交互逻辑都不能相应。）
3. 哪个控制器模打出了那个控制器，那么被模打出的控制器就是被哪个控制器所强引用。



