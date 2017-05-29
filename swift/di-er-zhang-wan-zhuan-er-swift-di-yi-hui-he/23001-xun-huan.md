### c语言中的do...while变成了swift中的repeat {... }while

### continue,break:continue结束当前循环开始下一次循环，break结束所有循环。

### 更为灵活的where case

### guard关键字：

```
/*guard...else{
*
* }
*保证guard后面的条件成立继续执行guard语句外面的函数，当不成立时执行else语句里面的代码。
 */
let a = arc4random()
let b = arc4random()
func compare(a:UInt,b:UInt){
 guard a != b else {
 print("a等于b")
 return
 }
 if a>b{
 print("a大于b")
 }else{
 print("a小于b")
 }
}
compare(a: UInt(a), b: UInt(b))
```



