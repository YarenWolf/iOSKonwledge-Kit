# UINavagationController重写push和pop方法

> 有个需求就是在App的Tab的首页需要显示浮动着的交互动画的机器人，该机器人具有机器学习的特点，因此可以不断的与用户交互，怎么样实现只浮动在App的5个tab首页，当点击跳转不是首页的时候不需要显示

因为5个tab上是5个自定义的导航控制器，所以我们可以监听导航控制器的push和pop事件，并且在push和pop的事件中判断当前控制器的字控制器的数量来判断窗口上的机器人是否需要显示，其实这里要说的就是如何监听push和pop事件。

```
/**
 *  重写这个方法的目的:为了拦截整个push过程,拿到所有push进来的子控制器
 *
 *  @param viewController 当前push进来的子控制器
 */
- (void)pushViewController:(UIViewController *)viewController animated:(BOOL)animated
{
    //    if (viewController != 栈底控制器) {
    if (self.viewControllers.count > 0) {

        for (UIView *view in [UIApplication sharedApplication].keyWindow.subviews) {
            if ([view isKindOfClass:[XLRobotImageView class]]) {
                if (self.viewControllers.count > 0) {
                    self.robotView = (XLRobotImageView *)view;
                    [view removeFromSuperview];
                }
            }
        }


        // 当push这个子控制器时, 隐藏底部的工具条
        viewController.hidesBottomBarWhenPushed = YES;

        UIButton *backButton = [UIButton buttonWithType:UIButtonTypeCustom];
        backButton.frame = CGRectMake(0, 0, 44, 44);
        [backButton setImage:[UIImage imageNamed:@"backArror"] forState:UIControlStateNormal];
        [backButton setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];

        backButton.adjustsImageWhenHighlighted = NO;
        backButton.contentHorizontalAlignment = UIControlContentHorizontalAlignmentLeft;

        backButton.titleLabel.font = [UIFont systemFontOfSize:16];

        [backButton addTarget:self action:@selector(back) forControlEvents:UIControlEventTouchUpInside];
        [backButton setImageEdgeInsets:UIEdgeInsetsMake(0, 5 * BoundWidth/375, 0, 0)];
        viewController.navigationItem.leftBarButtonItem = [[UIBarButtonItem alloc] initWithCustomView:backButton];
    }

    // 将viewController压入栈中
    [super pushViewController:viewController animated:animated];
}


-(UIViewController *)popViewControllerAnimated:(BOOL)animated{
    //在5个tab的首页需要显示
    NSArray *vcs = self.viewControllers;
    UIViewController *topVC = vcs[vcs.count - 2];
    if (self.viewControllers.count >= 2) {
        if ([topVC isKindOfClass:[MZPregnancyHomeController class]] ||
            [topVC isKindOfClass:[HLSettingViewController class]] ||
            [topVC isKindOfClass:[BBXEditViewController class]] ||
            [topVC isKindOfClass:[HLFriendTopicController class]] ||
            [topVC isKindOfClass:[MZBookViewController class]]
            ) {
            [[UIApplication sharedApplication].keyWindow addSubview:self.robotView];
        }
    }
    return [super popViewControllerAnimated:animated];
}
```

**敲黑板，注意啦**

因为我做的一个全局的机器人只需要浮动在App的5个模块的首页，所以当页面进入第二层的时候就需要隐藏机器人，当App的顶层控制器是最外层的首页的时候再显示机器人，用导航控制器的push和pop监听就可以实现这个需求，但是遇到的一个问题就是当App从首页进入到第二层页面，用于手动右滑且滑到一半停止，这样子页面还是停留在第二层但是此时也会触发pop方法上面的代码就有点问题

因此想办法需要监听导航控制器里面每个控制器的出现事件，找到一个方法 ` - (void)navigationController:(UINavigationController *)navigationController didShowViewController:(UIViewController *)viewController animated:(BOOL)animated;`恰好满足需求，以前没用过记录下来

```
- (void)navigationController:(UINavigationController *)navigationController didShowViewController:(UIViewController *)viewController animated:(BOOL)animated{
    self.interactivePopGestureRecognizer.enabled = [self.viewControllers count] > 1 ;

    if ([viewController isKindOfClass:[MZPregnancyHomeController class]] ||
        [viewController isKindOfClass:[HLSettingViewController class]] ||
        [viewController isKindOfClass:[BBXEditViewController class]] ||
        [viewController isKindOfClass:[HLFriendTopicController class]] ||
        [viewController isKindOfClass:[MZBookViewController class]]
        ) {
        [[UIApplication sharedApplication].keyWindow addSubview:self.robotView];
    }
}

```

 

