# 封装

> 接上一篇 “原型protype”接着讲封装。



一、

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

Javascript 还提供了一个 instanceof 运算符，验证原型对象与实例对象之间的关系。

```
cat1 instanceof Cat
true
cat2 instanceof Cat
true
```



