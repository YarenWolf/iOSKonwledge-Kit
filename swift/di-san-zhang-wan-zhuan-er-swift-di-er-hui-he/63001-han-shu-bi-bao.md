### 函数闭包

```
闭包和函数都是引用类型，另外，闭包的特点就是一个函数有权访问另外一个函数内的变量和参数
```

```
var arr:[Int] = [1,2,231,32,18,22]
arr.sort()
arr.sorted()
//闭包简化
arr.sort { (a, b) -> Bool in
 return a>b
}

arr.sort { (a, b) -> Bool in
 return String(a)>String(b)
}
arr.sorted(by: { (a,b)->Bool in return a>b })
//结尾闭包---如果一个闭包只存在函数的最后，那么该闭包可以写在函数外边
arr.sort(){ a,b in
 return a<b
}

//内容捕获
var num = 700
arr.sorted(by: { a,b in
 abs(a-num)<abs(b-num)
})
```

```
//闭包即函数，都是传引用，不是传值

func runningMetersWithMetersPerDay(metersPerDay:Int)->()->Int{
 var totalMeters = 0
 return{
 totalMeters += metersPerDay
 return totalMeters
 }
}

var planA = runningMetersWithMetersPerDay(metersPerDay: 5000)
planA()
planA()

var planB = runningMetersWithMetersPerDay(metersPerDay: 100)
planB()
planB()
var anotherPlan = planB
anotherPlan
anotherPlan() //传引用
```



