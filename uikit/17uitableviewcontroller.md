# UITableViewController

* UITableViewController下的footerView、headerView、cell开发者职能设置大小，不能设置位置（只可以设置高度）
* 在viewDidLoad方法中打印view的尺寸不可靠，因为在该方法中界面可能还会重新绘制（viewDidload-&gt;viewWillLayoutSubviews-&gt;viewDidLayoutSubviews-&gt;viewDidAppear）。所以打印view的尺寸在viewDIdAppear方法中进行
* 在ios7之后。苹果会自动给UINavigationController里面的UIScrollview顶部会添加额外的滚动区域64。

* 为什么界面上UITableview看起来不是从坐标（0,0）处开始布局，因为给tableview设置了contentInset

* 只有UItableview设置了contentInset才会自动往下偏移到合适的位置

  ```
  NSLog(@"contentInset->%@",NSStringFromUIEdgeInsets(self.tableView.contentInset));
  ```

* 不让UINavigationController下的UIScrollView自动偏移

```
 self.automaticallyAdjustsScrollViewInsets = NO;
```



