###天气预报App
    使用地理定位来获取天气预报
1、导入import CLLocation
2、继承CLLocationManagerDelegate
3、初始化CLLocationManager对象,在viewdidload方法中设置定位精度。KLLcationAccuracyBest
4、设置delegate为当前控制器
5、判断系统版本，UIDevice.current.systemVersion
6、请求用户定位权限。startUploadLocation
7、获取经纬度。
