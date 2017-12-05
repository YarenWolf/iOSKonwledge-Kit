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

