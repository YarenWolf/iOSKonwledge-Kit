# 双列表联动

> 用过了那么多的外卖App，总结出一个规律，那就是“所有的外卖App都有双列表联动功能”。哈哈哈哈，这是一个玩笑。
>
> 这次我也需要开发具有联动效果的双列表。也是首次开发这种类型的UI，记录下步骤与心得



#### 一、关键思路

* 懒加载左右2个UITableView
* 根据需要自定义Cell
* 2个UITableView加载到界面上的时候注意下部剧就好
* 因为需要联动效果，所有左侧的UITableView一般是大的分类，右边的UITableView一般是大分类小的小分类，所以有了这样的特点
  * 左边的UITableView是只有1个section和n个row
  * 右边的UITableView具有n个section（这里的section 个数恰好是左边UITableView的row数量），且每个section下的row由对应的数据源控制



二、第一版代码

```

```





