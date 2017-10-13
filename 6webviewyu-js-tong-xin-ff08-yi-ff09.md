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







举个🌰：

需求：

原生端提供一个UIWebView，加载一个网页内容。还有1个按钮，按钮点击一下网页增加一段段落文本。网页上有2个输入框，用户输入数字，点击按钮，js将用户输入的参数告诉native端，native去执行加法，计算完成后将结果返回给js



```
//index.html


<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf8">
            <script language="javascript">
                function loadURL(url){
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
                }
            	

            	function receiveValue(value){
            		alert("从原生拿到加法结果为："+value);
            	}
            
            	function check() {
            		var par1 = document.getElementById("par1").value;
            		var par2 = document.getElementById("par2").value;
                	loadURL("JSBridge://plus?par1=" + par1 +"&par2=" + par2);
            	}

            </script>
            </head>
    
    <body>
        <input type="text" placeholder="请输入数字"  id="par1"／> + <input type="text" placeholder="请输入数字"  id="par2"／> 
        <input type="button" value="=" onclick="check()" />
    </body>
</html>



//ViewController.m

-(void)addContentToWebView{
    NSString *jsString = @" var pNode = document.createElement(\"p\"); pNode.innerText = \"我是由原生代码调用js后将一段文件添加到html上，也就是注入\";document.body.appendChild(pNode);";
    [self.webView stringByEvaluatingJavaScriptFromString:jsString];
}


-(NSInteger)plusparm:(NSInteger)par1 parm2:(NSInteger)par2{
    return par1 + par2;
}
```







