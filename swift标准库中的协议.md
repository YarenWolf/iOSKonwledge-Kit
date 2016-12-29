###标准库中的协议

    Equatable --必须重载 “== “运算符 
    Comparable-- 必须重载 “< “运算符 
    CustomStringConvertible  -- 必须重载 “description “。之后利用print(对象)就可以打印出该对象。

var description: String{
        return "Wins \(wins), Losses \(losses)"
    }
    
      

Equatable,Comparable,CustomStringConvertible,BooleanType



```
import UIKit
/*
struct Record{
    var wins:Int
    var losses:Int
}


func == (left:Record, right:Record)->Bool{
    return left.wins == right.wins && left.losses == right.losses
}

func != (left:Record,right:Record)->Bool{
    return !(left == right)
}

let recordA = Record(wins: 10, losses: 5)
let recorcdB = Record(wins: 10, losses: 5)

recordA == recorcdB
recordA != recorcdB
 */

/*
struct Record:Equatable,Comparable{
    var wins:Int
    var losses:Int
    var print:String{
        return "Wins \(wins), Losses \(losses)"
    }
}
 
 recordA.print
 
 
*/


struct Record:Equatable,Comparable,CustomStringConvertible{
    var wins:Int
    var losses:Int
    var description: String{
        return "Wins \(wins), Losses \(losses)"
    }
    
 

    
}


func == (left:Record, right:Record)->Bool{
    return left.wins == right.wins && left.losses == right.losses
}

func <(left:Record,right:Record)->Bool{
    if left.wins == right.wins {
        return left.losses > right.losses
    }
    return left.wins < right.losses
}


let recordA = Record(wins: 10, losses: 5)
let recorcdB = Record(wins: 10, losses: 5)
let recordC = Record(wins: 10, losses: 2)
let recordD  = Record(wins: 8, losses: 3)

recordA == recorcdB
recordA != recorcdB

recordA < recorcdB
recordA < recordC
recordA < recordD

let scorers = [recordA,recorcdB,recordC,recordD]
/*
scorers.sorted { (left, right) -> Bool in
    return left.wins < right.wins
}
*/
scorers.sorted()


recordA.description
print(recordA)



extension Int{
    func isTrue()->Bool{
        if self > 0 {
            return true
        }else{
            return false
        }
    }
}


if 1.isTrue() {
    print("true")
}
```

