# Node 开发技巧

1、在 Node.js 的模块中，读取文件推荐用绝对路径

```
fs.readFile(__dirname + "/1.txt",function(){
    console.log(data);
});
```

2、npm install express --save

```
1、npm init
根据自己的情况设置信息。然后会多出一个 package.json 文件
2、npm install express --save 
在安装了依赖的模块后还会在 package.json 文件的 dependencies 中会增加 模块的配置信息
```



