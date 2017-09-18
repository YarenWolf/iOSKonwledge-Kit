# H5性能优化方面的探索

> H5很重要，很重要，很重要，重要的事情必须重复多遍，H5的优点：跨平台、迭代快、开发体验好。缺点：加载慢，用户体验差。所以在接下来很长一段时间内我将会从H5的几个缺点发面去研究如何优化。

## 

## 一、缓存问题及其解决办法

经常遇到一个问题，H5页面由于缓存问题经常在H5发布新版本之后客户端App看不到最新的效果，之前由于杂七杂八的问题项目工期紧没好好研究，最近抽空研究了下缓存问题。

缓存问题具体表现为：UIWebview首次打开加载慢；第二次加载速度明显快；H5资源更新过后在App上看不到更改的效果

为此我认为是缓存造成的问题，我进入App目录下，看到Library下的Caches下面有很多文件名称很长的文件，点击预览可以看到是图片、css等，本来我想着找出H5资源缓存到App中的特点，然后用NSFileManager删除掉缓存文件，发现此路不通。

#### 我想通过控制变量法研究缓存是否存在。

#### 做了一个实验。步骤如下：

* 用HBuilder（一个编辑器，开启后本机端口8020就可以访问网页）打开H5工程
* 在App的一个UIWebview页面上通过和电脑在同一个局域网的方式加载网页
* 在App上查看效果，观察某个元素的样式
* 在HBuilder编辑器中修改元素样式
* 在App上将UIWebView返回上一界面，再次进入查看该元素的样式
* 确定有没有变化，来确定有没有缓存

结论：页面实时效果变化的，没有缓存

对比实验：

* 用HBuilder（一个编辑器，开启后本机端口8020就可以访问网页）打开H5工程
* git提交到服务端
* 在App的一个UIWebview页面上通过公网IP的方式加载网页
* 在App上查看效果，观察某个元素的样式
* 在HBuilder编辑器中修改元素样式
* git提交后发布到服务器上
* 在App上将UIWebView返回上一界面，再次进入查看该元素的样式
* 确定有没有变化，来确定有没有缓存

结论：页面没有看到最新的效果，明显缓存了。但是我很想知道为什么本地局域网的方式请求网页不会缓存，而通过公网IP的方式会缓存。

为此，我做了进一步的实验，用谷歌浏览器分别请求本地局域网和公网ip查看资源加载的情况。

1、公网IP
![](https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/屏幕快照%202017-09-15%20下午5.56.28.png)

2、本地局域网

![](https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/屏幕快照%202017-09-15%20下午6.27.16.png)

关键词Status Code

结论：从图上可以看出本地局域网不管首次加载还是刷新都是直接请求；而通过局域网的方式请求：首次请求是从服务器上获取，在此刷新的时候是从（from memory cache）中获取的。

#### 猜想

局域网 的方式网速都比较快所以不会缓存；

公网IP的方式可能由于网速问题会将首次请求到的资源缓存下来。

所以确定缓存存在了，那么如何避免缓存？

* App在启动后请求一个接口，这个接口的目的是获取当前H5资源的版本号
* 将获得的版本号保存下来（App本地保存）
* 由于UIWebView上加载网页，发起网络请求都可以通过一个代理方法所拦截，所以我们可以在这个代理方法中判断url的参数，可能是[http://www.a.com/login、http://www.a.com/login.html、http://www.a.com/login.html?name=geek、http://www.a.com/login\#readme等等，所以我们判断过url后考虑如何将版本号加到url里面](http://www.a.com/login、http://www.a.com/login.html、http://www.a.com/login.html?name=geek、http://www.a.com/login#readme等等，所以我们判断过url后考虑如何将版本号加到url里面)
* 由于我们的App使用了不同模块的UIWebView，但是都是在UIWebView上需要大量的JS交互，所以使用了WebViewJavascriptBridge这个库。UIWebView本身的代理方法不会执行，所以修改这个库里面的WebViewJavascriptBridge.m文件的代码，差不多是下面的方式

```
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {
    if (webView != _webView) { return YES; }
    NSURL *url = [rntity Tag 的资源直接访问equest URL];
    if ([request.URL.absoluteString containsString:@"http"] || [request.URL.absoluteString containsString:@"https"]) {
        if ([request.URL.absoluteString containsString:@"?"]) {
            url = [NSURL URLWithString:[NSString stringWithFormat:@"%@&h5V=%@",request.URL.absoluteString,[ProjectUtil getH5VersionString]]];
        }else{
            url = [NSURL URLWithString:[NSString stringWithFormat:@"%@?h5V=%@",request.URL.absoluteString,[ProjectUtil getH5VersionString]]];
        }
    }
    LBPLOG(@"url->%@",[url absoluteString]);
    __strong WVJB_WEBVIEW_DELEGATE_TYPE* strongDelegate = _webViewDelegate;
    if ([_base isCorrectProcotocolScheme:url]) {
        if ([_base isBridgeLoadedURL:url]) {
            [_base injectJavascriptFile];
        } else if ([_base isQueueMessageURL:url]) {
            NSString *messageQueueString = [self _evaluateJavascript:[_base webViewJavascriptFetchQueyCommand]];
            [_base flushMessageQueue:messageQueueString];
        } else {
            [_base logUnkownMessage:url];
        }
        return NO;
    } else if (strongDelegate && [strongDelegate respondsToSelector:@selector(webView:shouldStartLoadWithRequest:navigationType:)]) {
        return [strongDelegate webView:webView shouldStartLoadWithRequest:request navigationType:navigationType];
    } else {
        return YES;
    }
}
```

总结：

App的缓存问题暂时研究到这里，后期会继续研究其他方面的问题

# 拓展

通过浏览器我们知道有的缓存是200 OK（from cache ），有的缓存是304 Not modified。如果运维移除了Entity Tag就一直是200（from cache）。如果没有移除的话2者是交替出现的。



为什么2者会有区别？

* 200 OK（from cache）是直接点击链接或者在浏览器地址栏中输入网址敲回车键的结果
* 而304 modified是我们刷新了浏览器页面时触发或者设置了长缓存、但Entity Tags没有移除时触发

做了 实验得出结论：

* 直接访问有缓存的网站都触发 200 OK \(from cache\)

* 刷新浏览器则会触发304

* 同一域名下，没有 Entity Tag 的资源直接访问，是 200 OK \(from cache\) 的结果

* 同一域名下，有Entity Tag 的资源直接访问，是出现304 Not Modified



