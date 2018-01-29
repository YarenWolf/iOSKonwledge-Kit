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

![](/assets/屏幕快照 2018-01-28 下午10.26.21.png)

#### 运行

* 终端执行 node app.js 然后在浏览器访问 [http://127.0.0.1:3000](http://127.0.0.1:3000)
* 终端执行 npm start ，然后在浏览器访问 [http://127.0.0.1:3000（此命令运行依赖](http://127.0.0.1:3000（此命令运行依赖)  package.json 中的 “script” 的内容）

```
"scripts": {
    "start": "node app.js"
}
```







#### 关于中间件的顺序重要性

* Express 中间件的执行是顺序执行的，从第一个中间件开始执行到最后一个中间件执行结束，发出响应

![](/assets/3663059-b6acea9ec3f0a8f9.jpg)

* Koa 中间件从第一个中间件开始执行，遇到 next\(\) 开始执行下一个中间件，等执行结束再逆序执行，执行上一个中间件后的代码，直到执行到第一个中间件为止。然后发出响应。

  
![](/assets/3663059-03622ea2a9ffce2a-2.jpg)

