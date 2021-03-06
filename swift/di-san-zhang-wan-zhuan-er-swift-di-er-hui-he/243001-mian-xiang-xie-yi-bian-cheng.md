### default implementation 默认实现和协议拓展

```
import UIKit

/*
protocol Record{
    var wins:Int { get }
    var losses:Int { get }
    func winningPercent()->Double

}


struct BasketballRecord:Record,CustomStringConvertible{
    var wins: Int

    var losses: Int
    func winningPercent() -> Double {
        return Double(wins)/Double(wins + losses)
    }

    var description: String{
        return String(format: "WINS :%d , LOSSES : %d", [wins,losses])
    }
}

struct BaseballRecord:Record,CustomStringConvertible{
    var wins: Int
    var losses: Int
    func winningPercent() -> Double {
        return Double(wins)/Double(wins+losses)
    }

    var description: String{
        return String(format: "WINS : %i , LOSSES : %d ", [wins,losses])
    }
}

let basketballRecord = BasketballRecord(wins: 3, losses: 10)
let baseballRecord = BaseballRecord(wins: 4, losses: 10)
print(basketballRecord)
print(baseballRecord)

*/

//想让实现某协议的对象都需要实现某个方法。那么可以让该协议实现某个协议。此后可以在该协议的拓展中实现那个协议的方法

protocol Record:CustomStringConvertible{
    var wins:Int { get }
    var losses:Int { get }
    func winningPercent()->Double
}

//拓展协议可以拓展协议的细则。注意：实现的方法中只能包含协议定义中的属性或者方法
//在协议中写了某个方法，那么在继承该协议的对象中都可以调用该方法。
extension Record{
    var description:String{
        return String(format: "WINS :%d , LOSSES : %d", [wins,losses])
    }

    func shoutWins(){
        print("We win \(wins) times.")
    }

    var gamePlayed:Int{
        return wins + losses
    }
}


struct BasketballRecord:Record{
    var wins: Int
    var losses: Int
    func winningPercent() -> Double {
        return Double(wins)/Double(gamePlayed)
    }

    var description: String{
        return String(format: "WINS :%d , LOSSES : %d", [wins,losses])
    }

}

struct BaseballRecord:Record{
    var wins: Int
    var losses: Int
    func winningPercent() -> Double {
        return Double(wins)/Double(gamePlayed)
    }

    var description: String{
        return String(format: "WINS : %i , LOSSES : %d ", [wins,losses])
    }
}

let basketballRecord = BasketballRecord(wins: 3, losses: 10)
let baseballRecord = BaseballRecord(wins: 4, losses: 10)
print(basketballRecord)
print(baseballRecord)

basketballRecord.shoutWins()
baseballRecord.shoutWins()

//协议拓展不仅可以拓展自定义的协议，也可以拓展系统自己的协议
extension CustomStringConvertible{
    var description:String{
        return "lbp"
    }
    var descriptionWithDate:String{
        return NSDate().description + " " + description
    }
}

print(basketballRecord.descriptionWithDate)
baseballRecord.description
```

### 面向协议的程序设计

```
import UIKit

protocol Record:CustomStringConvertible{
    var wins:Int { get }
    var losses:Int { get }
    func winningPercent()->Double
}

extension Record{
    var description:String{
        return String(format: "WINS :%d , LOSSES : %d", [wins,losses])
    }

    func shoutWins(){
        print("We win \(wins) times.")
    }

    var gamePlayed:Int{
        return wins + losses
    }

    func winningPercent() -> Double {
        return Double(wins)/Double(gamePlayed)
    }
}

protocol Tieable{
    var ties:Int { get set }
}

//改进：
//拓展了遵守Record协议的类型，同时这种类型也遵守了Tieable协议
extension Record where Self:Tieable{
    var gamePlayed:Int{
        return wins + losses + ties
    }


    func winningPercent() -> Double {
        return Double(wins)/Double(gamePlayed)
    }

}

struct BasketballRecord:Record{
    var wins: Int
    var losses: Int
    var description: String{
        return String(format: "WINS :%d , LOSSES : %d", [wins,losses])
    }

}

struct BaseballRecord:Record{
    var wins: Int
    var losses: Int
    var description: String{
        return String(format: "WINS : %i , LOSSES : %d ", [wins,losses])
    }
}

struct FootballRecord:Record,Tieable{
    var wins: Int
    var losses: Int
    var ties:Int
    /*
 等待改进：改进见上方改进
    var gamePlayed:Int{
        return wins + losses + ties
    }
 */
}

let basketballRecord = BasketballRecord(wins: 3, losses: 10)
let baseballRecord = BaseballRecord(wins: 4, losses: 10)
let footballRecord = FootballRecord(wins: 1, losses: 1, ties: 1)

print(basketballRecord)
print(baseballRecord)

footballRecord.wins
footballRecord.losses
footballRecord.ties
footballRecord.gamePlayed
footballRecord.winningPercent()


basketballRecord.shoutWins()
baseballRecord.shoutWins()

extension CustomStringConvertible{
    var description:String{
        return "lbp"
    }
    var descriptionWithDate:String{
        return NSDate().description + " " + description
    }
}

print(basketballRecord.descriptionWithDate)
baseballRecord.description
```



