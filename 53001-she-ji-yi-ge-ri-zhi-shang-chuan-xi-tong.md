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

![](/assets/屏幕快照 2017-12-11 上午10.28.53.png)结论：如果是引入不同源的js文件，报错信息必为“Script error”。

查找后：相当于window.onerror方法只捕获到了一个errorMessage，而且是固定字符串，毫无参考价值。查了点资料（[Webkit源码](http://trac.webkit.org/browser/branches/chromium/648/Source/WebCore/dom/ScriptExecutionContext.cpp?rev=77122#L301)），发现在浏览器实现script资源加载的地方，是进行了同源策略判断的，如果是非同源资源，errorMessage就被写死为“Script error”了：

查找 **MDN **文档后发现可以使用 “**crossorigin**” 属性来覆盖浏览器的行为。

# 总结

try...catch...

* 无法捕捉语法错误。只能捕捉运行时错误
* 可以拿到出错的信息，堆栈，出错的文件、行号、列号
* 需要借助工具将所有的function块以及文件加入try...catch...中

window.onerror

* 可以捕捉语法错误、运行时错误
* 可以拿到出错的信息，堆栈，出错的文件、行号、列号
* 在当前页面出错的js都会捕捉到，包括浏览器插件的js、flash抛出的异常信息
* 跨域的js文件资源，需要加特殊的处理
* 


