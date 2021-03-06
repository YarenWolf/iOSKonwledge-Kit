### 属性观察器

```
 应用场景：属性观察器经常用于保护数据的合法性
```

```
class LightBulb{
 static let maxCurrent = 30 //静态常量用来描述类的限制
 var current = 0 {
 /*
 *注意：和计算型属性类似，括号里的值可以省略，省略了必须用系统自带的值。newValue和oldValue
 */

 //willSet{
 willSet(newCurrent){
 print("Current value changed, the change is \(newCurrent - current)")
 }

 //调用didSet的时候current已经是最新的值了
 didSet(oldCurrent){
 if current == LightBulb.maxCurrent{
 print("Pay attention, the current value get to the maximum point.")
 }else if current > LightBulb.maxCurrent{
 print("Current is too high, falling back to previous setting.")
 current = oldCurrent
 }
 print("The current is \(current)")
 }
 }
}

let bulb = LightBulb()
bulb.current = 20
bulb.current = 30
bulb.current = 40
```

```
注意：didSet和willSet不会在初始化阶段init调用。
```

```
enum Theme{
 case DayMode
 case NightMode
}

//注意：didSet和willSet不会在初始化阶段init调用。
class UI{
 var fontColor:UIColor!
 var backgroundColor:UIColor!
 var themeMode:Theme = .DayMode{
 didSet{
 self.changeColor(mode: themeMode)
 }
 }

 func changeColor(mode:Theme) ->Void {
 switch themeMode {
 case .DayMode:
 fontColor = UIColor.black
 backgroundColor = UIColor.white
 case .NightMode:
 fontColor = UIColor.white
 backgroundColor = UIColor.black
 }
 }
 init(themeMode:Theme) {
 self.themeMode = .DayMode
 self.changeColor(mode: themeMode)
 }
}

let ui = UI(themeMode: .DayMode)
ui.themeMode
ui.fontColor
ui.backgroundColor

ui.themeMode = .NightMode
ui.themeMode
ui.fontColor
ui.backgroundColor
```



