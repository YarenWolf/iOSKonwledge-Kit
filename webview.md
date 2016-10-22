###某些情况需要webview加载网页展示某些数据，但是数据可能长得类似电话号码或者url等，webview默认是会识别的，所以显示样式根网页的css样式不一致，这样只需要将webview的识别关闭即可。一行代码：
```
 web.dataDetectorTypes = UIDataDetectorTypeNone;
```


###1、uiwebview在viewwillappear方法中加载本项目工程中的网页资源会出现一个奇怪bug。第一次打开该页面可以加载出网页，离开该页面后再次进入该页面网页看不到。

必须写在viewdidload方法中。
此外如果urlrequest为网址（http://www.baidu.com)就没有问题