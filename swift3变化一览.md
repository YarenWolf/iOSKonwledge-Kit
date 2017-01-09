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
3、



