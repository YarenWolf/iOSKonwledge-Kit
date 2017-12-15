> Javascript 是函数式编程语言，支持高阶函数、闭包。

Array 有 map\(\) 和 filter\(\) 方法，可是 Object 没有这些方法，此外低版本的浏览器诸如 IE6~8 也没有，怎么办？

* 自己将这些方法添加到 Array.prototype 中，然后给 Object.prototype 也加上 mapObject\(\) 方法
* 找一个合适的第三方开源库，[**underscore**](https://github.com/jashkenas/underscore)





