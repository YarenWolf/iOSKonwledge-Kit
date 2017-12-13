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

Animal.isPrototypeOf(cat1)            //false
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

何为继承,在 JS 里面继承就是子对象的 prototype 属性所指向的对象 指向 父对象的 构造函数，

这个步骤可以通过 Cat.prototype = new Animal\(\); 实现 但是此句代码执行后 Cat对象的 prototype 的 costructor 指向的

确实 Animal 的构造函数，这样会破坏原型链， 因此还必须为 Cat 对象的 prototype 所指向的对象的 constructor 设置为 Cat本身的构造函数 Cat.prototype.constructor = Cat;



## 三、直接继承 prototype

```
function Animal() {

}
Animal.prototype.category = "动物";

function Cat(name, color) {
    Animal.call(this, name, color);
}

Cat.prototype = Animal.prototype;
Cat.prototype.constructor = Cat;
console.log(Cat.prototype.constructor);
console.log(Animal.prototype.constructor);
----
Animal.prototype.constructor
ƒ Cat(name,color){
      Animal.call(this,name,color);
}
Cat.prototype.constructor
ƒ Cat(name,color){
	Animal.call(this,name,color);
}
```



