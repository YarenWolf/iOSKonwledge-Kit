### 运算符重载

```
import UIKit
struct Vector3{
    var x:Double = 0.0
    var y:Double = 0.0
    var z:Double = 0.0
    // swift下标基础
    subscript(index:Int)->Double?{
        get{
            switch(index){
            case 0 :return x
            case 1: return y
            case 2:return z
            default : return nil
            }
        }
        set{
            newValue
            guard let newValue = newValue else{
                return
            }
            switch(index){
            case 0: x = newValue
            case 1: y = newValue
            case 2: z = newValue
            default: return
            }
        }
    }
    subscript(axis:String)->Double?{
        switch(axis){
        case "x","X": return x
        case "y","Y": return y
        case "z","Z": return z
        default: return nil
        }
    }
}


//运算符重载的本质就是一个函数
func +(left:Vector3, right:Vector3)->Vector3{
    print("operating...")
    return Vector3(x:left.x + right.x,y: left.y + right.y,z: left.z + right.z)
}


func *(left:Vector3, right:Vector3)->Double{
    return left.x * right.x + left.y * right.y + left.z * right.z
}

func *(left:Vector3 , a:Double)->Vector3{
    return Vector3(x: a*left.x, y: a*left.y, z: a*left.z)
}

func *(a:Double, right:Vector3)->Vector3{
    return right * a
    //return Vector3(x:a*right.x , y:a*right.y ,z:a*right.z)
}

func +=(left:inout Vector3, right:Vector3){
    left = left + right
}

//取反
prefix func -(vector:Vector3)->Vector3{
    return Vector3(x: -vector.x, y:  -vector.y, z:  -vector.z)
}

//注意：= 不能被重载，被编译器所持有

var va = Vector3(x: 1, y: 2, z: 3)
let vb = Vector3(x: 3, y: 4, z: 5)

va + vb
va * vb

va * -1

-1 * vb

-va

va += vb
va
```

### 比较运算符重载

```
import UIKit
struct Vector3{
    var x:Double = 0.0
    var y:Double = 0.0
    var z:Double = 0.0
    // swift下标基础
    subscript(index:Int)->Double?{
        get{
            switch(index){
            case 0 :return x
            case 1: return y
            case 2:return z
            default : return nil
            }
        }
        set{
            newValue
            guard let newValue = newValue else{
                return
            }
            switch(index){
            case 0: x = newValue
            case 1: y = newValue
            case 2: z = newValue
            default: return
            }
        }
    }
    subscript(axis:String)->Double?{
        switch(axis){
        case "x","X": return x
        case "y","Y": return y
        case "z","Z": return z
        default: return nil
        }
    }
}
//===用来比较“类”
//比较运算符重载

func ==(left:Vector3, right:Vector3)->Bool{
    return left.x == right.x && left.y == right.y && left.z == right.z
}

func !=(left:Vector3, right:Vector3)->Bool{
//    return left.x != right.x || left.y != right.y || left.z != right.z
    return !(left == right)
}

var va = Vector3(x: 1, y: 2, z: 3)
let vb = Vector3(x: 3, y: 4, z: 5)

va == vb
va != vb

let arr = [1,5,4,3,2]
arr.sorted()

arr.sorted { (a, b) -> Bool in
    return a > b
}
```

### 运算符优先级、结合

```
import UIKit
struct Vector3{
    var x:Double = 0.0
    var y:Double = 0.0
    var z:Double = 0.0
    // swift下标基础
    subscript(index:Int)->Double?{
        get{
            switch(index){
            case 0 :return x
            case 1: return y
            case 2:return z
            default : return nil
            }
        }
        set{
            newValue
            guard let newValue = newValue else{
                return
            }
            switch(index){
            case 0: x = newValue
            case 1: y = newValue
            case 2: z = newValue
            default: return
            }
        }
    }
    subscript(axis:String)->Double?{
        switch(axis){
        case "x","X": return x
        case "y","Y": return y
        case "z","Z": return z
        default: return nil
        }
    }
}

func +(left:Vector3, right:Vector3)->Vector3{
    print("operating...")
    return Vector3(x:left.x + right.x,y: left.y + right.y,z: left.z + right.z)
}


func *(left:Vector3, right:Vector3)->Double{
    return left.x * right.x + left.y * right.y + left.z * right.z
}

func *(left:Vector3 , a:Double)->Vector3{
    return Vector3(x: a*left.x, y: a*left.y, z: a*left.z)
}

func *(a:Double, right:Vector3)->Vector3{
    return right * a
    //return Vector3(x:a*right.x , y:a*right.y ,z:a*right.z)
}

func +=(left:inout Vector3, right:Vector3){
    left = left + right
}

//告诉编译器这是我们自定义的运算符
postfix operator +++
postfix func +++(vector:inout Vector3)->Vector3{
    vector += Vector3(x: 1.0, y: 1.0, z: 1.0)
    return vector
}

var va = Vector3(x: 1, y: 2, z: 3)
var vb = Vector3(x: 3, y: 4, z: 5)


va+++
prefix operator +++
prefix func +++(vector:inout Vector3)->Vector3{
    let ret = vector
    vector += Vector3(x: 1, y: 1, z: 1)
    return ret
}

+++vb
vb

//声明双目运算符
infix operator ** : ATPrecedence
precedencegroup ATPrecedence{
    associativity : right
    higherThan: AdditionPrecedence
}

func **(x:Double, p:Double)->Double{
    return pow(x, p)
}

2**3
2**3**2
1+2**3**2
```



