# 从0开始设计一个日志系统

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
> 当函数返回true，它会阻止事件

知道系统有一个window.onerror方法，在此方法里面可以捕获系统的错误信息。下面

