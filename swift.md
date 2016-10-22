###Optional解包unwrap
```
let num：Int? = 32
num = nil
print（“the num is \(bum!)"
```
    强行解包结果可能为nil导致App奔溃。
```
let num2 ：Int? = nil
```
###智能解包
```
if let unwrapNum = nun，let unwrapNum2 = num2｛
 Print（“解包后的值为\(unwrapNum)、\(unwrapNum2)")
}
```

###optional chaining && nil coalesce
```
import UIKit
/*
 * optional chainig
 */
var errorMessage:String? = "success"

/*
 * nil coalesce
 */
let message:String
//方式1
if let errorMessage = errorMessage {
 message = errorMessage
}else{
 message = "No error"
}
//方式2
let message2 = errorMessage == nil ? "No error" : errorMessag``

//方式3
let message3 = errorMessage ?? "No error"

//尝试解包1
if let message = errorMessage {
 message.uppercased()
}

//尝试解包2
var newMessage = errorMessage?.uppercased()

//强制解包--->有可能报错
errorMessage!.uppercased()
```

