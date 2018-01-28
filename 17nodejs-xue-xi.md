# Node.js

## 一、遇到的坑1

在学习 Node.js 的 Express 框架的时候编写了很简单的代码，但是在命令行终端运行的时候却报错了，代码如下

```
var express = require("express");
var app =  express();

app.get('/',function(req,res){
    res.send("Hello world!");
});

app.listen(3000,function(){
    console.log("The sever is running http://127.0.0.1:3000/");
});
```

#### 报错信息如下

![](/assets/屏幕快照 2018-01-28 下午10.22.52.png)

后来查找了下资料，这种报错一般都是缺少模块的引用导致的。具体的原因就是我在我的代码中使用了 koa 模块，但是却没在 package.json 文件的 “dependencies” 项中添加 “koa” 的依赖。

![](/assets/屏幕快照 2018-01-28 下午10.26.21.png)

### 解决办法

* 手动编写 package.json 文件。将所需的依赖包都写进去，写到 “dependencies” 为 key 的 字典里面。其他一些值可以不写![](/assets/屏幕快照 2018-01-28 下午10.29.05.png)

* 在终端运行 npm install koa@2.0.0





