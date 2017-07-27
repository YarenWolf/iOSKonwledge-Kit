### 二维数组

```
import UIKit
var board = Array<Array<Int>>()
for i in 0...10{
 board.append(Array(repeating:0,count:10))
}

let randX = Int(arc4random()%10)
let randy = Int(arc4random()%10)
board[randX][randy] = 1
```



