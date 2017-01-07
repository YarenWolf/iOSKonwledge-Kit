###强引用循环
    属性默认是强引用，表现：引用计数器加1。
    2个对象在引用上形成强引用循环就会造成内存泄漏。解决办法->weak
    weak:告诉对象：我只使用你，而不拥有你。引用计数器不加1
    用weak修饰的时候必须是：（1）、var修饰；（2）可选型 ?

```
import UIKit
class Person{
    var name:String
    weak var apartment:Apartment?
    init(name:String) {
        self.name = name
        print("Person",name,"is initialized")
    }
    
    deinit {
        print("Person",name,"is deinitialized!")
    }
}

class Apartment{
    let name:String
    var tenant:Person?
    init(name:String) {
        self.name = name
        print("Apartment",name,"is initialized")
    }
    deinit {
        print("Apartment", name ,"is deinitialized")
    }
}

var lbp:Person? = Person(name: "lbp")
var department:Apartment? = Apartment(name: "huahaiyuan")

lbp?.apartment = department
department?.tenant = lbp
department = nil
lbp?.apartment
lbp = nil


```

![](/assets/屏幕快照 2017-01-07 下午11.33.14.png)



###强引用循环---unowned

    循环引用--unowned,unowned修饰的量不能赋值为nil
    使用unowned必须保证它修饰的内存永远不会提前释放掉
```
import UIKit
class Person{
    var name:String
    var creditCard:CreditCard?
    init(name:String) {
        self.name = name
        print("Person",name,"is initialized")
    }
    deinit {
        print("Person",name,"is deinitialized!")
    }
}

class CreditCard{
    let number:String
    unowned let customer:Person
    init(number:String,customer:Person) {
        self.number = number
        self.customer = customer
        print("Credit Card ",number,"is initialized")
    }
    deinit {
        print("Credit Card ",number,"is deinitialized")

    }
}


var lbp:Person? = Person(name: "lbp")
var goldenCard:CreditCard? = CreditCard(number: "123456", customer: lbp!)
lbp?.creditCard = goldenCard
//循环引用--unowned
lbp = nil
goldenCard = nil

```



