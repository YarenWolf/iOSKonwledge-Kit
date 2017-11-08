# 模仿滴滴打车-拖动地图大头针定位到当前位置



> 平时使用多很多次的滴滴App，其中有一点就是定位，默认定位到用户当前的位置，不过默认定位不准确用户也可以自己手动定位。



今天需要实现这个功能，先说一下自己走的弯路。

1. 因为看了滴滴App的操作，用于滑动界面大头针就定位到那个位置，于是就想着给地图加入一个滑动手势
2. 加入手势发现不走进方法内，各种调试还是不行
3. 最后发现MKMapViewDelegate的代理里面就有拖动地图的方法
4. 有2个代理方法。分别是：- \(void\)mapView:\(MKMapView \*\)mapView regionWillChangeAnimated:\(BOOL\)animated;和-\(void\)mapView:\(MKMapView \*\)mapView regionDidChangeAnimated:\(BOOL\)animated;分别在将要改变和已经改变的时候触发

```
- (void)mapView: (MKMapView *)mapView regionDidChangeAnimated:(BOOL)animated {
    if (_isSendLocation) {
        CLLocationCoordinate2D currentCordinate = mapView.region.center;
        _currentLocationCoordinate = currentCordinate;
        [self removeToLocation:currentCordinate];
        self.addressString = @"未知位置";
    }
}
```



