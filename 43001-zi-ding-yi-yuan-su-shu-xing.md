# 自定义元素属性

# 一、方式一

在很早以前我们自定义元素的属性要通过 `user-defined-attribute="value"`的方式来设置自己需要的属性

设置自定义属性

```
<h1 user-defined-attribute="share">杭城小刘</h1>
```

获取自定义属性

```
document.getElementsByTagName("h1")[0].getAttribute("user-defined-attribute")
```

# 二、方式二

现在H5为我们提供了一个data属性  **"data-" **作为前缀，可以让所有的HTML元素都支持自定义的属性，只要在标签里面以 **"data-"**

为前缀定义需要的属性即可

设置自定义属性

```
<h1 data-share="true">杭城小刘</h1>
```

获取自定义属性（使用H5自定义属性对象Dataset来获取）

```
var myDiv = document.getElementsByTagName("h1")[0];
var theValue = myDiv.dataset;    //DOMStringMap对象

document.getElementsByTagName("h1")[0].dataset.share
document.getElementsByTagName("h1")[0].dataset["share"]
```

```
document.getElementsByTagName("h1")[0].getAttribute("data-share")
```

`DOMStringMap`是HTML5一种新的含有多个名-值对的交互变量

# 三、H5 dataset的操作

删掉一个data属性

```
delete myDiv.dataset.share
```

增加一个属性

```
myDiv.dataset.happy="ok"
```

# 四、dataset兼容性处理

如果不支持dataset，有必要做一下兼容性处理

```
if(myDiv.dataset){
    myDiv.dataset.sad = "false";
    var thevalue = myDiv.dataset.sad;
}else{
    myDiv.setAttribute("data-attribute","sad");
    var theValue = myDiv.getAttribute("data-attribute"); // 获取自定义属性  
}
```





做一个实验：

```
<!DOCTYPE html>
<html>

	<head>
		<title>我的标题</title>
		<meta charset="utf-8" />
	</head>

	<body>
		<h1 data-share="true">杭城小刘</h1>
	</body>
	<script>
		console.log(document.getElementsByTagName("h1")[0].getAttribute("user-defined-attribute"));
	</script>

</html>
```

然后利用chrome调试，在console命令行分别输入3条指令，结果如下图
png
![实验结果](https://raw.githubusercontent.com/FantasticLBP/iOSKonwledge-Kit/master/assets/屏幕快照%202017-12-05%20下午10.19.04.png)可以看出来，dataset后跟的属性是驼峰命名原则，如果多个单词第二个单词首字母需要大写，检查元素可以看到神奇的变化。

