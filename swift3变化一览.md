###swift3变更

1、弃用++和--。 --> += 1, -= 1

2、Never类型：用于错误处理

```
import UIKit

func myFatalError()->Never{
    print("!!!!!!")
    fatalError()
}

func mode( a:Int, b :Int)->Int{
    guard  b != 0 else {
        myFatalError()
    }
    return a%b
}
//never用于错误处理
mode(a: 10, b: 3)

```
3、隐式可选型的类型推测

4、@autoclosure 

```
import UIKit

let username:String? = "lbp"
let password:String
//1
//if let userName = username{
//    password = userName
//}else{
//    password = "123456"
//}
//username
//password


//2
//let screenName = username != nil ? username! : "Guest"
//screenName

//3
//password = username ?? "123456"
//username
//password

```





