# 继承

有2个对象，分别是 Cat 和 Animal。

```
function Animal(name, color) {
    this.category = "animal";
}

function Cat(name, color) {
    this.name = name;
    this.color = color;
}
```

如何让猫继承自动物？

## 一、构造函数绑定

使用 call 或 apply 方法，将父对象的构造函数绑定到子对象上，即在子对象的构造函数中加一行：

```
function Cat(name, color) {
    Animal.call(this, name, color);
    this.catrgory = "cat";
}
```

## 二、prototype 模式

```
function Animal(name, color) {
    this.name = name;
    this.color = color;
}

function Cat(name, color) {
    Animal.call(this, name, color);
}

Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;

var cat = new Cat("大毛", "黄色");

Animal.prototype.isPrototypeOf(cat)            //true
```



