### 范型类型

```
import UIKit

//范型类型
let arr:Array<Int> = Array<Int>()
let dct = Dictionary<String,Int>()
let set = Set<Float>()

//栈:后进先出
struct Stack<T>{
    var items = [T]()
    func isEmpty() -> Bool{
        return items.count == 0
    }

    mutating func push(item:T){
        items.append(item)
    }


    mutating func pop()->T?{
        guard !self.isEmpty() else {
            return nil
        }

        return items.removeLast()
    }
}

extension Stack{
    func top() -> T? {
        return self.items.last
    }
}

//创建Pair类型的变量
struct Pair<T1,T2>{
    var a: T1
    var b: T2
}

var s = Stack<Int>()
s.top()
s.push(item: 1)
s.push(item: 2)
s.top()
s.pop()
s

var pair = Pair(a: 0, b: "Hello")
```



