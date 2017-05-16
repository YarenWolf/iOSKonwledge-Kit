### 枚举 - Enum

```
枚举也可以定义函数
```

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

### 枚举和类一样可以声明函数。

```
import UIKit
enum Shape{
 case Square(side:Double)
 case Rectangle(width:Double,height:Double)
 case Circle(centerX:Double,centerY:Double,radius:Double)
 case Point
}

let square = Shape.Square(side: 2.0)
let circle = Shape.Circle(centerX: 0, centerY: 0, radius: 4)
let rectangle = Shape.Rectangle(width: 2.0, height: 3.0)
let point = Shape.Point


func area(shape:Shape)->Double{
 switch shape {
 case let .Circle(centerX: _, centerY: _, radius: radius):
 return M_PI*radius*radius
 case let .Rectangle(width,height):
 return width*height

 case let .Square(side):
 return side*side
 case .Point:
 return 0
 }
}

area(shape: square)
area(shape: circle)
area(shape: rectangle)
area(shape: point)
```

### 枚举的递归

```
enum ArithmeticExpression{

 case Number(Int)

 indirect case Addition(ArithmeticExpression,ArithmeticExpression)

 indirect case Multiplication(ArithmeticExpression,ArithmeticExpression)

}



let five = ArithmeticExpression.Number(5)

let four = ArithmeticExpression.Number(4)

let sum = ArithmeticExpression.Addition(five, four)

let two = ArithmeticExpression.Number(2)

let product = ArithmeticExpression.Multiplication(sum, two)


func evaluat(expression:ArithmeticExpression)->Int{
 switch expression {
 case let .Number(value):
 return value
 case let .Addition(left,right):
 return evaluat(expression: left)+evaluat(expression: right)
 case let .Multiplication(left,right):
 return evaluat(expression: left)*evaluat(expression: right)
 }
}
evaluat(expression: product)
```



