### 可选型

```
可选型的本质是枚举
```

```
import UIKit
var website:Optional<String> = Optional.some("lbp")

website = .none
website = "imooc.com"
website = nil
switch website {
case .none:
 print("no website")
case let .some(website):
 print("the course is \(website)")
}

if let website = website {
 print("the course is \(website)")
}else{
 print("no website")
}
```



