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

```
function Cat(name, color) {
    this.name = name;
    this.color = color;
}

Cat.prototype.category = "猫科动物";
Cat.prototype.say = function() {
    console.log(this.name + "在叫。");
}

var cat1 = new Cat("大猫", "黄色");
var cat2 = new Cat("二猫", "黑色");
cat1.say();
cat1.say();

console.log(cat1.category);
console.log(cat2.category);
console.log(cat1.say === cat2.say);

-------
大猫在叫。
index.html?__hbt=1513127720695:114 大猫在叫。
index.html?__hbt=1513127720695:122 猫科动物
index.html?__hbt=1513127720695:123 猫科动物
index.html?__hbt=1513127720695:125 true
```

## 四、prrototype 模式验证

为了配合 prototype 属性，Javascript定义了一些辅助方法，帮助我们使用它：

* isPropertyOf\(\)：用来判断，某个 prototype 对象和某个实例之间的关系。
* hasOwnProperty\(\)：用来判断一个属性是自己本地属性（子类的属性），还是继承自 prototype 对象的属性（父类属性）
* in 运算符： 用来判断某个实例是否含有某个属性，不管是不是本地属性

```
function Cat(name, color) {
    this.name = name;
    this.color = color;
}

Cat.prototype.category = "猫科动物";
Cat.prototype.say = function() {
    console.log(this.name + "在叫。");
}

var cat1 = new Cat("大猫", "黄色");
var cat2 = new Cat("二猫", "黑色");
cat1.say();
cat1.say();
-------------


Cat.prototype.isPrototypeOf(cat1)
true
Cat.prototype.isPrototypeOf(cat2)
true
cat1.hasOwnProperty("name")
true
cat1.hasOwnProperty("category")
false
name in cat1
false
"name" in cat1
true
"category" in cat1
true
"category" in Cat
false
for (var prop in cat1){  console.log("cat1['" + prop + "']= " + cat1[prop]); }
VM1076:1 cat1['name']= 大猫
VM1076:1 cat1['color']= 黄色
VM1076:1 cat1['category']= 猫科动物
VM1076:1 cat1['say']= function (){
     console.log(this.name + "在叫。");
}
```



