### 闭包中的强引用循环

```
隐式可选型方便初始化。
```

```
import UIKit
class Country{
    let name:String
    //将属性声明为var和！代表在初始化的时候告诉编译器可以为nil，当实例化好之后就可以使用该变量
    var captical:City!
    init(countryName:String,capticalName:String) {
        self.name = countryName

        //隐式可选型 self.captical = nil
        //2段式构造
        self.captical = City(cityName:capticalName , country: self)
        print("Country",name,"is initialized")
    }
    deinit {
        print("Country",name,"is deinitialized")
    }
}

class City{
    let name:String
    unowned let country:Country
    init(cityName:String,country:Country) {
        self.name = cityName
        self.country = country
        print("City",name,"is initialized")
    }
    deinit {
        print("City",name,"is deinitialized")
    }
}

var China:Country? = Country(countryName: "China", capticalName: "Beijing")
China = nil
```



