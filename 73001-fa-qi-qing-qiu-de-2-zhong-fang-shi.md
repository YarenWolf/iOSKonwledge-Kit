# JS 发起请求的2种方式



> 经常在 H5 端有一种需求就是点击页面的某个按钮实现发送短信或者拨打电话的功能，有些人知道 window.location.href 还好，不然有些人就会写一个 iframe 就去发起请求



* window.location.href

```
window.location.href =  "tel:" + momTel;
```

* iframe

```
var WVJBIframe = document.createElement('iframe');
WVJBIframe.style.display = 'none';
WVJBIframe.src = "tel:" + momTel;
document.documentElement.appendChild(WVJBIframe);
setTimeout(function() {
         document.documentElement.removeChild(WVJBIframe)
}, 0)；
```



