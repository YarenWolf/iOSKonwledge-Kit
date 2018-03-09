# Web 与 H5 交互的坑



1、遇到一个问题 ，一个功能在 iOS 手机上正常工作，但是在 Android 上不正常，依照经验来看无非就是2个原因：（1）、URL参数少传递了；（2）、JS 在移动端的 webview 上报错了，所以我让远程对接的人员将 url 打印出来，发现没错。继续让他打印查看下 js 错误日志，发现 “Cannot read property "getItem" of null”。 代码出错行数在 259。看了下具体代码就是读取 localstorage

![](/assets/Andoid_Webview_Localstroage_erroe.jpg)

想了想，在我们的项目中 Android 原生的代码在使用 webview 的时候额外设置了代码具体如下



```
mWebView.getSettings().setDomStorageEnabled(true);
```



