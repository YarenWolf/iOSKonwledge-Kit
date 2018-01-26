# Node.js



* 踩坑1

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

报错信息如下



![](/assets/QQ20180126-164115@2x.png)



后来查找了下资料，这种报错一般都是缺少模块的引用导致的。具体的原因就是我在我的代码中使用了 Express 模块，但是却没在 package.json 文件的 “dependencies” 项中添加 “express” 的依赖。



所以解决办法就是手动在终端执行命令 

```
npm install express --save
```



