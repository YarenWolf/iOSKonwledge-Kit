# iOS11适配

* UITableView reloadData视图漂移或者闪动解决办法

  * .视图漂移或者闪动原因：  
    因为iOS 11后系统默认开启Self-Sizing，首先要知道Self-Sizing是个什么东东。官方文档是这样解释的：大概就是说我们不用再自己去计算cell的高度了，只要设置好这两个属性，约束好布局，系统会自动计算好cell的高度。  
    IOS11以后，Self-Sizing默认开启，包括Headers, footers。如果项目中没使用estimatedRowHeight属性，在IOS11下会有奇奇怪怪的现象，因为IOS11之前，estimatedRowHeight默认为0，Self-Sizing自动打开后，contentSize和contentOffset都可能发生改变。  
    所以可以通过以下方式禁用：

    在tableView初始化的地方加入下面代码

    ```
     self.tableView.estimatedRowHeight = 0;
     self.tableView.estimatedSectionHeaderHeight = 0;
     self.tableView.estimatedSectionFooterHeight = 0;
    ```

    现在在reloadData视图漂移或者闪动就没有了

* iOS11 的时候你的页面如果是 UIWebview，那么可能在页面的顶部出现一段距离。也就是顶部白屏。这是由于 iOS11 的 SafeArea 引起的。可能引起的原因是设置了 UIWebview 的高度。有2种解决办法

  * 设置 webview 的高度为 ScreenHeight -  BarHeight - 34。 34 为底部安全距离

  * 添加如下代码

    ```
    if (@available(iOS 11.0, *)) {
        webView.scrollView.contentInsetAdjustmentBehavior = UIScrollViewContentInsetAdjustmentNever;
    }
    ```





