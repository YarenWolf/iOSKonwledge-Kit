###在IOS中导航栏所管理的视图控制器放入一个堆栈数组中，代码中可以来管理这个堆栈，今天我们就来学习一下。
    场景：我从A到B，逻辑操作后跳到C，但是想到c返回的话会跳到B，但是不需要到B而直接到A，所以我从B到C的时候我想把self.navigationController里的B移除。
c在2个地方用到。所以点击左上角返回按钮，就需要pop。


遇到的坑：
![](/assets/314647BC-EA02-4133-B0CF-CB84120A23AD.png)

原来自己理解错了setViewControllers方法，set了之后就已经入栈了。所以不需要再次push一次。

修改后的代码：


```
 SSICAViewController *ssicaVC = [[SSICAViewController alloc] init];
 NSArray *vcs = self.navigationController.viewControllers;
 NSMutableArray *currentControllers = [NSMutableArray arrayWithArray:vcs];
 [currentControllers removeLastObject];
 [currentControllers addObject:ssicaVC];
 [self.navigationController setViewControllers:currentControllers animated:YES];

```



