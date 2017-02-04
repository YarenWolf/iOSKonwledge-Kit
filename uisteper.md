###UISteper


```
import UIKit

class ViewController: UIViewController {
    var steper:UIStepper!
    var label:UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        //UISteper
        steper = UIStepper()
        steper.center = self.view.center
        steper.maximumValue = 10
        steper.minimumValue = 1
        steper.value = 5
        steper.stepValue = 0.5      
        steper.isContinuous = true //按钮操作按钮不放可以连续改变值
        steper.addTarget(self, action: #selector(stepAdd(_:)), for: .valueChanged)
        self.view.addSubview(steper)
        
        
        //UILabel
        label = UILabel(frame: CGRect(x: 10, y: 64, width: UIScreen.main.bounds.size.width, height: 50))
        label.textColor = UIColor.black
        label.text = "当前值为：\(steper.value)"
        self.view.addSubview(label)
    }

 
    func stepAdd(_ steper:UIStepper){
        label.text = "当前值为：\(steper.value)"
    }

}
```

