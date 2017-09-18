# WebViewJavascriptBridge

做过混合开发的很多人都知道Ionic和PhoneGap之类的框架，这些框架在web基础上包了一层Native，然后通过Bridge技术使得js可以调用视频、位置、音频等功能。本文就是介绍这层Bridge的交互原理，通过阅读本文你可以了解到js与ios及android底层的通讯原理及JSBridge的封装技术及调试方法。



## 一、原理篇 {#-}

下面分别介绍IOS和Android与Javascript的底层交互原理

### IOS {#ios}

在讲解原理之前，首先来了解下iOS的UIWebView组件，先来看一下苹果官方的介绍：

> You can use the UIWebView class to embed web content in your application. To do so, you simply create a UIWebView object, attach it to a window, and send it a request to load web content. You can also use this class to move back and forward in the history of webpages, and you can even set some web content properties programmatically.



上面的意思是说UIWebView是一个可加载网页的对象，它有浏览记录功能，且对加载的网页内容是可编程的。说白了UIWebView有类似浏览器的功能，我们使用可以它来打开页面，并做一些定制化的功能，如可以让js调某个方法可以取到手机的GPS信息。

但需要注意的是，Safari浏览器使用的浏览器控件和UIwebView组件并不是同一个，两者在性能上有很大的差距。幸运的是，苹果发布iOS8的时候，新增了一个WKWebView组件，如果你的APP只考虑支持iOS8及以上版本，那么你就可以使用这个新的浏览器控件了。

原生的UIWebView类提供了下面一些属性和方法

**属性：**

* loading：是否处于加载中
* canGoBack：A Boolean value indicating whether the receiver can move backward. \(只读\)
* canGoForward：A Boolean value indicating whether the receiver can move forward. \(只读\)
* request：The URL request identifying the location of the content to load. \(read-only\)

**方法：**

* loadData：Sets the main page contents, MIME type, content encoding, and base URL.
* loadRequest：加载网络内容
* loadHTMLString：加载本地HTML文件
* stopLoading：停止加载
* goBack：后退
* goForward：前进
* reload：重新加载
* stringByEvaluatingJavaScriptFromString：执行一段js脚本，并且返回执行结果

#### Native（Objective-C或Swift）调用Javascript方法 {#native-objective-c-swift-javascript-}

Native调用Javascript语言，是通过`UIWebView`组件的`stringByEvaluatingJavaScriptFromString`方法来实现的，该方法返回js脚本的执行结果。

```
// Swift

webview.stringByEvaluatingJavaScriptFromString(
"Math.random()"
)

// OC

[webView stringByEvaluatingJavaScriptFromString:@
"Math.random();"
];
```

从上面代码可以看出它其实就是调用了`window`下的一个对象，如果我们要让native来调用我们js写的方法，那这个方法就要在`window`下能访问到。但从全局考虑，我们只要暴露一个对象如JSBridge对native调用就好了，所以在这里可以对native的代码做一个简单的封装：

```
//下面为伪代码

webview.setDataToJs(somedata);
webview.setDataToJs = function(data) {
 webview.stringByEvaluatingJavaScriptFromString(
"JSBridge.trigger(event, data)"
)
}
```

#### Javascript调用Native（Objective-C或Swift）方法 {#javascript-native-objective-c-swift-}

反过来，Javascript调用Native，并没有现成的API可以直接拿来用，而是需要间接地通过一些方法来实现。UIWebView有个特性：在UIWebView内发起的所有网络请求，都可以通过delegate函数在Native层得到通知。这样，我们就可以在UIWebView内发起一个自定义的网络请求，通常是这样的格式：jsbridge://methodName?param1=value1&param2=value2

于是在UIWebView的delegate函数中，我们只要发现是jsbridge://开头的地址，就不进行内容的加载，转而执行相应的调用逻辑。

发起这样一个网络请求有两种方式：1. 通过localtion.href；2. 通过iframe方式；  
通过location.href有个问题，就是如果我们连续多次修改window.location.href的值，在Native层只能接收到最后一次请求，前面的请求都会被忽略掉。

使用iframe方式，以唤起Native APP的分享组件为例，简单的封闭如下：

```
var url = 'jsbridge://doAction?title=分享标题&desc=分享描述&link=http%3A%2F%2Fwww.baidu.com';
var iframe = document.createElement('iframe');
iframe.style.width = '1px';
iframe.style.height = '1px';
iframe.style.display = 'none';
iframe.src = url;
document.body.appendChild(iframe);
setTimeout(function() {
    iframe.remove();
}, 100);
```

然后Webview就可以拦截这个请求，并且解析出相应的方法和参数。如下代码所示：

```
func webView(webView: UIWebView, shouldStartLoadWithRequest request: NSURLRequest, navigationType: UIWebViewNavigationType) -> Bool {
        print("shouldStartLoadWithRequest")
        let url = request.URL
        let scheme = url?.scheme
        let method = url?.host
        let query = url?.query

        if url != nil && scheme == "jsbridge" {
            print("scheme == \(scheme)")
            print("method == \(method)")
            print("query == \(query)")

            switch method! {
                case "getData":
                    self.getData()
                case "putData":
                    self.putData()
                case "gotoWebview":
                    self.gotoWebview()
                case "gotoNative":
                    self.gotoNative()
                case "doAction":
                    self.doAction()
                case "configNative":
                    self.configNative()
                default:
                    print("default")
            }

            return false
        } else {
            return true
        }
    }
```



