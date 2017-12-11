# 从0开始设计一个日志系统

## 前言

> An [event handler](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Event_handlers) for the   [`error`](https://developer.mozilla.org/en-US/docs/Web/Events/error)  event. Error events are fired at various targets for different kinds of errors.
>
> ## Syntax {#Syntax}
>
> For historical reasons, different arguments are passed to`window.onerror`and`element.onerror`handlers \(as well as on error-type[`window.addEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/Window/addEventListener)handlers\).
>
> `window.onerror = function(message, source, lineno, colno, error) { ... }`
>
> Function parameters:
>
> * `message`: error message \(string\). Available as`event`\(sic!\) in HTML`onerror=""`handler.
> * `source`: URL of the script where the error was raised \(string\)
> * `lineno`: Line number where error was raised \(number\)
> * `colno`: Column number for the line where the error occurred \(number\)
> * `error`:[Error Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)\(object\)
>
> 当函数返回true，它会阻止触发默认的事件处理程序
>
> ```
> window.addEventListener("error",function(event){ ...  })
> ```
>
> element.onerror
>
> element.erroe = function\(event\){  ... }

## 实验

### 实验1

```
    <body>
        <h1>测试H1</h1>

        <div style="margin: 20px;">
            <p>我是段落1</p>
            <p>我是段落2</p>
            <p>我是段落3</p>
        </div>
    </body>

    <script>
        window.onerror = function(errorMessage, ScriptURI, lineNumber, colunmNumber, errorObj) {
            console.log("错误信息:" + errorMessage);
            console.log("错误文件:" + ScriptURI);
            console.log("错误行号:" + lineNumber);
            console.log("错误列号:" + colunmNumber);
            console.log("错误对象:" + errorObj);
        };

        window.onload = function() {
            document.getElementsByTagName("p")[0].style = "color:red;";
            document.getElementsByTagName("p")[1].css("font-size", "30px");

        };
    </script>
```

![](/assets/屏幕快照 2017-12-11 上午10.23.33.png)结论：正常发生的error信息是可以在window.onerror方法里面捕捉到。

### 实验2

```
    <script>
        window.onerror = function(errorMessage, ScriptURI, lineNumber, colunmNumber, errorObj) {
            console.log("错误信息:" + errorMessage);
            console.log("错误文件:" + ScriptURI);
            console.log("错误行号:" + lineNumber);
            console.log("错误列号:" + colunmNumber);
            console.log("错误对象:" + errorObj);
        };

    </script>
    <script src="error.js"></script>
```

结论：自定义的异常也是可以捕捉到的



### 实验3

```
	<script>
		window.onerror = function(errorMessage, ScriptURI, lineNumber, colunmNumber, errorObj) {
			console.log("错误信息:" + errorMessage);
			console.log("错误文件:" + ScriptURI);
			console.log("错误行号:" + lineNumber);
			console.log("错误列号:" + colunmNumber);
			console.log("错误对象:" + errorObj);
		};
		
	</script>
	<script src="https://cn.bing.com/rms/rms%20answers%20Homepage%20ZhCn$Mobile$MobileRichHomepageV2/cj,nj/0b7b8145/637a1b58.js"></script>
```



