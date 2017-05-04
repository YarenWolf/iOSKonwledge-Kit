###某些情况需要webview加载网页展示某些数据，但是数据可能长得类似电话号码或者url等，webview默认是会识别的，所以显示样式根网页的css样式不一致，这样只需要将webview的识别关闭即可。一行代码：
```
 web.dataDetectorTypes = UIDataDetectorTypeNone;
```

http://blog.csdn.net/lwjok2007/article/details/47058101/
http://blog.csdn.net/lwjok2007/article/details/47058795


1、原生调用js
```
-(void)webViewDidFinishLoad:(UIWebView *)webView

{
//网页加载完成调用此方法
//首先创建JSContext 对象（此处通过当前webView的键获取到jscontext）
JSContext *context=[webView valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
NSString *alertJS=@"alert('test js OC')"; //准备执行的js代码
 [context evaluateScript:alertJS];//通过oc方法调用js的alert
}
```

2、
###1、uiwebview在viewwillappear方法中加载本项目工程中的网页资源会出现一个奇怪bug。第一次打开该页面可以加载出网页，离开该页面后再次进入该页面网页看不到。

必须写在viewdidload方法中。
此外如果urlrequest为网址（http://www.baidu.com)就没有问题

###2、UIWebView与H5交互
    1、H5页面上有个按钮，当点击了网页上按钮需要调用原生端的方法去显示一个View。

```
原生端：
__weak typeof(self) WeakSelf = self;

 [self.bridge registerHandler:@"openEntrance" handler:^(id data, WVJBResponseCallback responseCallback) {
 [WeakSelf chooseOperation];
 responseCallback(nil);
 }];


-(void)chooseOperation{
 self.webView.userInteractionEnabled = YES;
 self.operationView.frame = CGRectMake(0, BoundHeight, BoundWidth, BoundHeight);
 self.operationView.alpha = 0;
 [self.view addSubview:self.operationView];
 [UIView animateWithDuration:ShowOperationDuration animations:^{
 self.operationView.frame = CGRectMake(0, 0, BoundWidth, BoundHeight);
 self.operationView.alpha = 1;
 self.webView.userInteractionEnabled = NO;
 } completion:^(BOOL finished) {
 }];
}
```

```
H5:
$("#addHealthData").click(function () {
 if (isiOS) {
 console.log(1);
 setupWebViewJavascriptBridge(function(bridge) {
 bridge.callHandler("openEntrance",
 function(response) {
 $("#addHealthData").addClass("hide");
 });
 });
 }

 if (isiOS) {
 setupWebViewJavascriptBridge(function(bridge) {
 bridge.registerHandler("needHideButton",
 function(data) {
 if (data.status === "1") {
 $("#addHealthData").removeClass("hide");
 }
 });
 });
 }

```

当弹出的View出来后网页上按钮需要隐藏，那么网页注册一个隐藏的方法，原生端需要callHandler一下。见上述代码。


此外当点击原生端一个RightBarButton时需要弹出View，这个方式是原生做的，但是此时需要隐藏网页上的按钮，因此需要利用stringByEvaluatingJavaScriptFromString，找出网页的那个元素并隐藏掉。（Jquery写法:$("#addHealthData").addClass("hide");）

```
[self.webView stringByEvaluatingJavaScriptFromString:@"$(\"#addHealthData\").addClass(\"hide\");"]; //原生端利用stringByEvaluatingJavaScriptFromString执行js代码，隐藏webview的按钮
```

3、禁止网页检测任何url。某些网页加载好后会莫名其妙的显示某些文字为蓝色，这里需要设置UIWebView.webView.dataDetectorTypes = UIDataDetectorTypeNone;

