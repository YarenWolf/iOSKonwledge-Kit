### is可以检查协议遵守

```
协议也可以看作是某种类型，判断某个对象是否遵循某个协议，对某个对象尝试解包成遵循某个协议的对象。
```

```
import UIKit
protocol Shape{
    var name:String{get}
}

protocol HasArea{
    func area()->Double
}

struct Point:Shape{
    let name: String = "point"
    var x:Double
    var y:Double
}

struct Rectangle:Shape,HasArea{
    let name: String = "rectangle"
    var origin:Point
    var width:Double
    var height:Double
    func area() -> Double {
        return width*height
    }
}

struct Circl:Shape,HasArea{
    let name: String = "circle"
    var center:Point
    var radius:Double
    func area() -> Double {
        return M_PI * radius * radius
    }
}

struct Line:Shape{
    let name: String = "line"
    var a:Point
    var b:Point
}

let shapes:[Shape] = [
    Rectangle(origin: Point(x:1,y:2), width: 3, height: 4),
    Line(a: Point(x:0,y:0), b: Point(x: 0, y: 2)),
    Point(x: 3, y: 3),
    Circl(center: Point(x:0,y:0), radius: 5)
]

for shape in shapes{
    //判断这个对象是否遵循某个协议
    if  shape is HasArea {
        print("\(shape.name) has area")
    }else{
        print("\(shape.name) has no area")
    }
}

print("============================================================")

//协议也可以看作是某种类型，判断某个对象是否遵循某个协议，对某个对象尝试解包成遵循某个协议的对象。
for shape in shapes{
    if let rectange = shape as? HasArea{
        print("The area of ",shape.name,"is ",rectange.area() )
    }else{
        print(shape.name,"has no area")
    }
}
```



