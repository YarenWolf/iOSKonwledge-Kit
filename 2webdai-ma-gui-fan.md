# Web开发代码规范

1. 良好的网页开发习惯是为你的img标签添加alt属性，这对于弱网环境、屏幕阅读器、搜索引擎都有好处。下面的css代码帮助你找到没有添加alt属性的img标签

```
img[alt=""],img:not([alt]){
     border: 5px dashed red ;
}
```



```
<body>		
	<img src="../../img/B-scan/week-bscan-item.png" alt="我是一张教育类型的图片" />
	<img src="../../img/B-scan/frequent-bscan-item.png	" />	
</body>
```



