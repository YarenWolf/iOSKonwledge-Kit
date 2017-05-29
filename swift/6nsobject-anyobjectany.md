### NSObject 、AnyObject、Any

```
AnyObject高于NSObject，脱离了继承树。
AnyObject类似于oc中的id。void *(指向任何类型的指针)
Any>AnyObject>NSObject
```

```
import UIKit

//NSObject 、AnyObject、Any

class Person{
    var name:String
    init(name:String){
        self.name = name
    }
}

var array1:[Person] = [Person(name:"lbp")]
var array2:[Any] = [Person(name:"lbp")]
var array3:[Any] = [Person(name:"lbp")]
array3.append({(a:Int)->Int in
    return a*a
})

var objects:[Any] = [
    CGFloat(3.1415926),
    "imooc",
    UIColor.blue,
    NSData(),
    Int(32),
    Array<Int>([1,2,3,4]),
    Person(name: "lbp")
]
objects[0]

objects.append("1")
//swift中函数很重要，因此数组也可以增加函数元素，但是数组类型一定为Any
objects.append({(a:Int)->Int in
    return a * a
    })
```



