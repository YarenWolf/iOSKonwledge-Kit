### deinit--析构函数

```
import UIKit
class Person{
    var name:String
    init(name:String) {
        self.name = name
        print(name,"is coming")
    }
    func doSomething(){
        print(name,"is doing something")
    }

    deinit {
        print(name,"is leaving")
    }
}

var lbp:Person? = Person(name: "lbp")
lbp = nil

func inTheShop(){
    print("==============")
    print("Welcome")
    let person:Person? = Person(name: "lbp")
    person?.doSomething()
}

inTheShop()

print("==============")

for i in 0...10{
    var person = Person(name: "xinian")
    person.doSomething()
}


//在一个大括号内代表一个变量的作用域
```

### 引用计数器

```
import UIKit
class Person{
    var name:String
    var pet:Pet?
    init(name:String) {
        self.name = name
        print(name,"is coming")
        print("Person",name,"is initialized")


    }
    init(name:String,petName:String) {
        self.pet = Pet(name: petName)
        self.name = name
        print("Person",name,"is initialized")

    }
    func doSomething(){
        print(name,"is doing something")
    }

    deinit {
        print(name,"is leaving")
    }
}

class Pet{
    var name:String
    init(name:String) {
        self.name = name
        print("Pet",name,"is initialized")
    }

    deinit {
        print("Pet",name,"is deinitialized")
    }
}

/*
* 1
var lbp:Person? = Person(name: "lbp", petName: "huhu")
lbp = nil
*/

//2
var lbp:Person? = Person(name: "lbp")
var pet:Pet? = Pet(name: "huhu")
lbp?.pet = pet
pet = nil
lbp = nil

//3
var lbp1:Person? = Person(name: "lbp", petName: "huhu")
var lbp2 = lbp1
var lbp3 = lbp1

lbp1 = nil
lbp2 = nil
lbp3 = nil
```

1、  
![](/assets/屏幕快照 2017-01-07 下午10.57.39.png)

2、  
![](/assets/屏幕快照 2017-01-07 下午10.55.23.png)

3、![](/assets/屏幕快照 2017-01-07 下午11.12.59.png)

