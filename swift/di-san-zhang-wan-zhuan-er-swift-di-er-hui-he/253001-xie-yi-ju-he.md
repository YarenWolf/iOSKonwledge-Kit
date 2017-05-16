### 协议聚合

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

protocol Prizable{
    func isPrizable()->Bool
}

struct BasketballRecord:Record,Prizable{
    var wins: Int
    var losses: Int
    var description: String{
        return String(format: "WINS :%d , LOSSES : %d", [wins,losses])
    }

    func isPrizable() -> Bool {
        return wins > 2
    }

}

struct BaseballRecord:Record,Prizable{
    var wins: Int
    var losses: Int
    let  gamePlayed: Int = 162
    var description: String{
        return String(format: "WINS : %i , LOSSES : %d ", [wins,losses])
    }


    func isPrizable() -> Bool {
        return gamePlayed  > 10 && winningPercent() >= 0.5
    }
}

struct FootballRecord:Record,Tieable,Prizable{
    var wins: Int
    var losses: Int
    var ties:Int

    func isPrizable() -> Bool {
        return wins > 1
    }
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


func award(one: Prizable & CustomStringConvertible){
    if  one.isPrizable(){
        print(one)
        print("Congratulation! You won a prize!")
    }else{
        print("You can not have the prize")
    }
}


struct Student:Prizable,CustomStringConvertible{
    var name:String
    var score:Int
    var description: String{
        return name
    }
    func isPrizable() -> Bool {
        return score >= 60
    }
}

let stuednt = Student(name: "geek", score: 100)
award(one: stuednt)

/** 此时的打印结果：
 *  Student(name: "geek", score: 100)
 *  Congratulation! You won a prize!
 * 因为需要award()函数打印出的东西需要一致性。比如打印出：
 *  name: "geek", score: 100
 *  Congratulation! You won a prize!
 * 因此就需要award()函数的参数需要遵循2个协议:Prizable和CustomStringConvertible因此写法上需要这样写protocol<CustomStringConvertible,Prizable>这叫做协议聚合
 * 但是在swift3中协议聚合的写法 Prizable&CustomStringConvertible
 */
```



