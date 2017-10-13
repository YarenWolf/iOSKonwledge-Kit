# WebView与JS通信探索（一）

#### UIWebView加载网页内容

可以通过本地文件、url等方式。

```
 NSString *htmlPath = [[NSBundle mainBundle] pathForResource:@"index" ofType:@"html"];   
 NSURLRequest *request = [NSURLRequest requestWithURL:[NSURL fileURLWithPath:htmlPath]];
 [self.webView loadRequest:request];
```





#### Native调用JavaScript

Native调用JS是通过UIWebView的stringByEvaluatingJavaScriptFromString 方法实现的，该方法返回js脚本的执行结果。

```
[webView stringByEvaluatingJavaScriptFromString:@"Math.random();"];
```

实际上就是调用了网页的Window下的一个对象。如果我们需要让native端调用js方法，那么这个js方法必须在window下可以访问到。



#### JavaScript调用Native



反过来，JavaScript调用Native，并没有现成的API可以调用，而是间接地通过一些其它手段来实现。UIWebView有个代理方法：在UIWebView内发起的任何网络请求都可以通过delegate函数在Native层得到通知。由此思路，我们就可以在UIWebView内发起一个自定义的网络请求，通常是这样的格式：**jsbridge://methodName?param1=value1&param2=value2...**



在UIWebView的delegate函数中，我们判断请求的scheme，如果request.URL.scheme是jsbridge，那么就不进行网页内容的加载，而是去执行相应的方法。方法名称就是request.URL.host。参数可以通过request.URL.query得到。

问题来了？？

发起这样1个网络请求有2种方式。1:location.href .2：iframe。通过location.href有个问题，就是如果js多次调用原生的方法也就是location.href的值多次变化，Native端只能接受到最后一次请求，前面的请求会被忽略掉。



使用ifrmae方式，以调用Native端的方法。



```
 var iFrame;
 iFrame = document.createElement("iframe");
 iFrame.style.height = "1px";
 iFrame.style.width = "1px";
 iFrame.style.display = "none";
 iFrame.src = url;
 document.body.appendChild(iFrame);
 setTimeout(function(){
    iFrame.remove();
 },100);
```









