### 字符串

```
var string = "swift lesson "
//字符串是基于unicode编码的
//前闭后开[startIndex,endIndex)
string.startIndex
string.endIndex

//定位具体位置的字符
let index:String.Index = string.index(string.startIndex, offsetBy: 4)
string[index]

// public func formIndex(before i: inout String.UTF16View.Indices.Index)
string[string.index(string.endIndex, offsetBy:-2)]
string.remove(at: string.index(string.endIndex, offsetBy: -1))
string
string.endIndex
string.removeSubrange(string.index(string.endIndex, offsetBy: -3)..<string.index(string.endIndex, offsetBy: 0))

string.uppercased()
string.lowercased()
string.capitalized
string
string.contains("Le")

//swift缺点
let s = "one third is \(1.0/3.0)"
let s2 = NSString(format:"one third is %.2f",1.0/3.0) as String
s2

//
var s3:NSString = "one third is 0.33"
s3.substring(to: 3)
s3.substring(from: 8)
s3.substring(with: NSMakeRange(4, 5))

let s5:NSString = "😇👿"
s5.length //NSString长度用length，string长度用.characters.count

let s6:String = "😇👿"
s6.characters.count

let s7 = " --- hello ---" as NSString
s7.trimmingCharacters(in: CharacterSet(charactersIn:"-"))
//字符串的的前一个、后一个字符串
```



