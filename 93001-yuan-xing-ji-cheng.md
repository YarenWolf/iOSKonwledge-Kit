# 原型继承

> JS中的继承跟其他面向对象语言的继承有点不太一样，要正确的了解JS中的继承还得了解JS的前世今生。

## 一、从JS的诞生之初说起

要理解Javascript的设计思想还得从它的诞生说起。

1994年网景公司（Netscape）发布了 Navigator 浏览器 0.9 版。这是历史上第一个比较成熟的网络浏览器，轰动一时，但是这个版本的浏览器只能用来浏览，不具备与访问者交互的能力。比如网页上有一个地方需要用户输入“用户名” ，当时浏览器无法判断访问者是否真的填写了，只有让服务器判断，如果没有填写，服务器就返回错误，**这太浪费时间和服务器资源了**。

因此，网景公司需要一种网页脚本语言，使得浏览器可以与网页互动，工程师 Brendan Eich 负责开发这种新语言。他觉得没有必要设计的很复杂，这种语言只要能够完成一些简单的操作就足够了，比如判断用户有没有填写表单。

1994年正是面向对象编程（OOP）最兴盛的时期，C++是当时最流行的语言，Java语言的1.0版即将在第二年推出，Sun 公司正在大肆造势。

Brendan  Eich 无疑收到了影响，JS里面所有的数据类型都是对象，这一点与Java非常相似，但是他随即遇到一个问题，到底要不要设计继承机制？

## 二、Brendan Eich的选择

如果 真的是一种简易的脚本语言，其实不需要有“继承”机制，但是JS里面都是对象，必须有一种机制，将所有的对象都联系起来，所以他最后还是设计了“继承”。

但是他没有引入“类”的概念，因为一旦有了“类”，JS就是一门完全面向对象的语言了，增加了初学者的难度，他考虑到 C++ 和 Java 都使用 new 命令，生成实例。

C++ 写法

```
ClassName *object = new ClassName(param);
```

Java 写法

```
Foo foo = new Foo();
```

因此引入了 new 命令，用来从原型对象生成一个实例对象，但是JS没有“类”的概念，怎么表示原型对象？

这时他想到 C++ 和 Java 都是使用 new 命令，会调用类的构造函数（constructor）。他就设计了一个简化版，在 JS 语言中，new 命令后跟的不是类，而是构造函数。

举例来说，用狗的构造函数表示狗对象的原型。

```
function Dog(name){
    this.name = name;
}
```

对这个构造函数使用 new， 就会生成一个狗对象的实例。

```
var dog1 = new Dog("阿拉斯加");
console.log(dog1.name);    //阿拉斯加
```

注意：构造函数中的 this 关键字，指向了新构建的实例对象。

## 三、new 运算符的缺点

用构造函数生成的实例对象，有一个缺点就是无法共享属性和方法。

比如，在Dog对象的构造函数中，设置一个实例对象的共有属性category

```
function Dog(name){
         this.name = name;
     this.categey = "dog";
}

var dog1 = new Dog("阿拉斯加");
var dog2 = new Dog("萨摩");

console.log(dog1.category);         //dog
console.log(dog2.category);         //dog

dog1.category = "阿拉斯加";                  
console.log(dog1.category);         //阿拉斯加
console.log(dog2.category);         //dog
```

这时候2个对象的category属性是独立的，修改其中一个，不会影响另一个。

这样 的弊端就是每一个实例对象，都有自己的属性和方法的副本，无法做到数据的共享，对内存的极大浪费。



## 四、prototype 属性的引入



考虑到这一点，Brendan Eich 决定为构造函数设置一个 prototype 属性。

这个属性包含一个对象，所有的实例对象需要共享的属性和方法都保存在这个对象里面，那些不需要共享的属性和方法就放在构造函数里面。

实例对象一旦创建，将自动引用 prototype 对象的属性和方法， 也就是说实例对象的属性和方法，分成两种，一种是自己（子类）的属性和方法，另一种是引用的（父类）。

```
function Dog(name) {
    this.name = name;
}

Dog.prototype.category = "dog";
var dog1 = new Dog("阿拉斯加");
var dog2 = new Dog("萨摩");
console.log(dog1.category); //dog
console.log(dog2.category); //dog
dog1.category = "啸天犬";
console.log(dog1.category); //啸天犬
console.log(dog2.category); //dog
dog1.__proto__.category = "啸天犬";
console.log(dog1.category); //啸天犬
console.log(dog2.category); //啸天犬	
```













