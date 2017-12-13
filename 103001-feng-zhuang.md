# 封装

> 接上一篇 “原型protype”接着讲封装。

## 一、简单封装

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

## 二、构造函数模式问题

上面的构造函数很好用，但是存在一个内存浪费的问题。对于每一个实例对象，category 属性和 say\(\) 方法都是一模一样的内容，重复复制同样的内容。

```
function Cat(name, color) {
    this.name = name;
    this.color = color;

    this.category = "猫科动物";
    this.say = function() {
        console.log("猫在叫");
    }
}

var cat1 = new Cat("大猫", "黄色");
var cat2 = new Cat("二猫", "黑色");
cat1.say();
cat1.say();

console.log(cat1.category);
console.log(cat2.category);

console.log(cat1.say === cat2.say);


---------
猫在叫
index.html?__hbt=1513127720695:113 猫在叫
index.html?__hbt=1513127720695:122 猫科动物
index.html?__hbt=1513127720695:123 猫科动物
index.html?__hbt=1513127720695:125 false
```

## 三、prototype 闪亮登场

JS规定，每个构造函数都有 prototype 属性，指向另一个对象，这个对象的属性和方法都会被构造函数的实例继承。

这样我们可以把共享的属性和方法直接定义在 prototype 对象上。

