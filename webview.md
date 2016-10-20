###某些情况需要webview加载网页展示某些数据，但是数据可能长得类似电话号码或者url等，webview默认是会识别的，所以显示样式根网页的css样式不一致，这样只需要将webview的识别关闭即可。一行代码：
```
 web.dataDetectorTypes = UIDataDetectorTypeNone;
```