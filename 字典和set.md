###字典
```
var dic = ["swift":"雨燕、快速","groovy":"绝妙的、时髦的","java":"哇岛"]
var dic1:Dictionary<String,String> = [:]
var dic2:[Int:Int] = [:]
var dic3:Dictionary<String,String> = Dictionary<String,String>()
var dic4:[Int:Int] = [Int:Int]()

dic.count
dic.isEmpty
dic.keys
Array(dic.keys)
Array(dic.values)
for key in Array(dic.keys){
 print(key)
}
for value in Array(dic.values){
 print(value)
}
for (key,value) in dic{
 print("\(key):\(value)")
}
dic == dic1

//修改
dic["swift"] = "雨燕、快速"
dic
dic.updateValue("爪哇岛、咖啡", forKey: "java")
dic
if let oldIntroduce = dic.updateValue("雨燕、快速", forKey: "swift"),let newIIntroduce = dic["swift"],oldIntroduce==newIIntroduce {
 print(oldIntroduce)
 print(newIIntroduce)
 print("原来你还在这里")
}

//增加
dic["objective-c"] = "ios language"
dic
dic.updateValue("网页代码", forKey: "javascript")
dic

//删除
dic["javascript"] = nil
dic
if let language = dic.removeValue(forKey: "objective-c") {
 print("\(language)成功被删除了)")
}
dic
dic.removeAll()
dic
```

###集合
``` 
var speciality:Set<String> = ["swift","oc"] 
var hoddy = Set(["it","pingpang","food"])
var emptySet1 = Set<Int>()
var emptySet2 = Set<String>()
var set3:Set = [1,2,3]
set3

speciality.count
speciality.isEmpty
speciality.contains("oc")

for index in speciality{
 index
}
//set不会出现重复的元素
speciality = ["swift","oc","oc","js"]
speciality
//集合是无序的:声明的第一个元素但是输出不一定是第一个
speciality.first

speciality.insert("js")
speciality
speciality.insert("js")
speciality
speciality.remove("js")
speciality

if let skill = speciality.remove("oc") {
 print("又忘记了\(skill)")
}

speciality.removeAll()
speciality

var skillA = Set(["swift","oc"])
var skillB = Set(["html","css","js"])
var skillc = Set(["swift","hmml"])

//并集
skillA.union(skillB) //只并集不改变
skillA
//skillA.formUnion(skillB) //并集并改变
//skillA
skillB
skillA
//交集
skillA.intersection(skillB)

//set加元素
skillA.insert("html")
skillA
skillB

//集合的减法
skillA.subtract(skillB) //计算出A独有的而B不具有的
skillA

skillA.symmetricDifference(skillB)
skillA.intersection(["1","2"])
skillA


var skilld:Set<String> = ["oc"]
skilld.isSubset(of: skillA) //子集
skillA.isSuperset(of: skilld) //父集

skilld.isStrictSubset(of: skillA
skillA.isStrictSuperset(of: skilld)
```