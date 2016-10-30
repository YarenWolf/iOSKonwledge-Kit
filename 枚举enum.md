###枚举 - Enum
    枚举也可以定义函数
```
     //枚举也可以定义函数 
enum Shape{
 case Squre(side:Double)
 case Rect(width:Double,height:Double)
 case Circle(radius:Double)
 case Point

 func area() -> Double {
     switch self {
         case let .Squre(side):
             return side*side
         case let .Rect(width,height):
             return width*height
         case let .Circle(radius):
             return M_PI*radius*radius
         case .Point:
             return 0
         }
}

let square = Shape.Squre(side: 2).area()
let circle = Shape.Circle(radius: 2.2)
let rect = Shape.Rect(width: 2.0, height: 2.0)
let point = Shape.Point

square.area()
circle.area()
rect.area()
point.area()
```