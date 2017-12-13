# 封装

接上一篇 “原型protype”接着讲封装。

```
function Cat(name, color) {
    this.name = name;
    this.color = color;
}

var cat1 = new Cat("大猫", "黄色");
var cat2 = new Cat("二猫", "黑色");
console.log(cat1.name);
console.log(cat2.color);
```

这时候 Cat1 和 Cat2 会自动含有1个 constructor 属性，指向他们的构造函数。

```
cat1.constructor === Cat
true
cat2.constructor === Cat
true
cat2.constructor === cat1.constructor
true
```



