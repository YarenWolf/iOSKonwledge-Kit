# Node 开发技巧

1、在 Node.js 的模块中，读取文件推荐用绝对路径

```
fs.readFile(__dirname + "/1.txt",function(){
    console.log(data);
});
```





