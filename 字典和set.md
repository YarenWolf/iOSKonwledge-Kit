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