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