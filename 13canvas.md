# Canvas

## 支持性

由于浏览器对 Canvas 的支持标准不一致，所以通常 &lt;canvas&gt; 内部添加一些说明行的 HTML 代码，如果浏览器支持 Canvas，它将忽略 &lt;canvas&gt; 内部的 HTML，如果浏览器不支持 Canvas，它将显示 &lt;canvas&gt; 内部的HTML。



## 一、基础使用

使用 Canvas 前，用 canvas.getContext 来判断浏览器是否支持 Canvas

```
var canvas = document.getElementById("test-canvas");
if (canvas.getContext) {
    console.log("你的浏览器支持canvas");
} else {
    console.log("你的浏览器不支持canvas");
}
```

getContext\('2d'\) 方法拿到一个 CanvasRenderingContext2D 对象，所有的绘图操作都需要通过这个对象完成。

```
var ctx = canvas.getContext("2d");
```

如果需要绘制 3D图形，我们可以通过

```
var gl = canvas.getContext("webgl");
```





### 1、绘制形状

```
var ctx = canvas.getContext("2d");
ctx.fillStyle = "rgb(200,0,0)";
ctx.fillRect(10,10,50,50);
		
ctx.fillStyle = "rgba(0,0,200,0.5)";
ctx.fillRect(30,30,50,50);
```



