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

优点：比方法2优点是效率高，不用执行和建立 Animal 的实例了，比较省内存。

缺点：Cat.prototype 和 Animal.prototype 都指向了一块内存空间，都是同一个对象，那么对任意一端的  prototype 修改都会影响到另一端。

```
Cat.prototype.constructor  = Cat;
console.log(Animal.prototype.constructor);
ƒ Cat(name,color){
      Animal.call(this,name,color);
}
```

## 四、利用空对象作为中介

由于直接继承原型 prototype 存在上述缺点，所以存在第四办法，利用一个空对象作为中介。

```
function Animal() {

}
Animal.prototype.category = "动物";

function Cat(name, color) {
    Animal.call(this, name, color);
}
var F = function() {};
F.prototype = Animal.prototype;
Cat.prototype = new F();
Cat.prototype.constructor = Cat;

var cat1 = new Cat("小花猫", "彩色");
```

F 是空对象，几乎不占内存，这时修改 Cat 的 prototype 对象，就不会影响 Animal 的 prototype 对象

