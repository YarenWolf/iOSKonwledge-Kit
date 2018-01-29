> Nunjucks是Mozilla开发的一个纯JavaScript编写的模板引擎，既可以用在Node环境下，又可以运行在浏览器端。但是，主要还是运行在Node环境下，因为浏览器端有更好的模板解决方案，例如MVVM框架。



模版引擎就是基于模版配合数据构造出字符串输出的一个组件。



需要考虑的问题：

* 转义。避免受到 XSS 攻击。
* 格式化 
* 简单逻辑



需要考虑性能问题。

* 对于模版渲染本身来说，速度是非常快的，因为就是字符串拼接，纯 CPU 操作
* 性能问题主要出现在文件读取模版内容这一步，这是一个 IO 操作。在 Node.js 环境中，单线程的 JS 最不能忍受的是同步 IO，但是 Nunjucks 默认使用同步 IO 读取模版文件
* 但是 Nunjucks 会缓存已读取的文件内容，也就是说，模版文件最多读一次，就会放在内存中，后面的请求是不会再次读取文件的，我们指定 noCache:false 这个参数。
* 在开发环境下可以关闭 cache，这样每次加载模版都是读取最新的模版，在生产环境下需要打开 cache。这样就不会有性能问题

```

const nunjucks = require("nunjucks");

function createEnv(path, opts) {
    var autoescape = opts.autoescape === undefined ? true : opts.autoescape,
        noCache = opts.noCache || false,
        watch = opts.watch || false,
        throwOnUndefined = opts.throwOnUndefined || false,
        env = new nunjucks.Environment(
            new nunjucks.FileSystemLoader("view", {
                noCache: noCache,
                watch: watch,
            }), {
                autoescape: autoescape,
                throwOnUndefined: throwOnUndefined
            });
    if (opts.filters) {
        for (var f in opts.filters) {
            env.addFilter(f, opts.filters[f]);
        }
    }
    return env;
}

var env = createEnv("views", {
    watch: true,
    filters: {
        hex: function (n) {
            return '0x' + n.toString(16);
        }
    }
});

/*
var s = env.render("hello.html",{
    name:"<Nunjucks>",
    fruits:['Apple','banana','Pear'],
    count:12000
});
console.log(s);
*/

/*
var s = env.render("base.html");
console.log(s);
*/

var s = env.render("extend.html",{
    header:"I am header",
    body:"I am body",
    "footer":"I am  footer"
});
console.log(s);
```



