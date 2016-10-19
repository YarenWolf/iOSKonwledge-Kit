##Switch循环语句+元组

 swith正常用法



```

//: Playground - noun: a place where people can play



import UIKit

//switch对元组的遍历



let loginResult = (true,"昔年")



let (isLoginSuccess,_) = loginResult



if isLoginSuccess {



 print("登陆成功")



}





//元组的值是int可以对int进行范围的判断



let coordinate = (1,1)



switch coordinate {



case (0,0):



 print("it is origin")



case (-1...1,0):



 print("it is on y")



case (0,-1...1):



 print ("it is on x")



case (-2...2,-2...2):



 print("it is near by origin")



default:



 print("it is oridinary ")



}











//对元组还可以进行value binding



let coordinate1 = (0,2)



switch coordinate1 {



case (0,0):



 print("it is at origin")



case (let x,0):



 print("the point is on x")



 print("the x value of point is \(x)")







case (0,let y):



 print("the point is on y")



 print("the y value of this is \(y)")



default:



 print("it is an origin point")



}







//switch的case语句可以使用“where”语法



let coordinate2 = (2,2)



switch coordinate2 {



case let (x,y) where x==y:



 print("(\(x),\(y)) is on the line x==y")



case let (x,y) where x == -y:



 print("(\(x),\(y) is pn the line x == -y")



case let(x,y):



 print("(\(x),\(y)) is on the line x == -y" )



}







//swith case语句结合元组



let course = ("3-2","区间运算符")



switch course {



case (_,let courseName) where courseName.hasPrefix("区间"):



 print("《\(courseName)》是一门运算符课程")



default:



 print("《\(course.1)》是一门其他课程")



}











let courseName = "昔年"



switch courseName {



case let name where name.hasPrefix("昔"):



 print("\(name)含有年")



default:



 print("不含年")



}











//switch 语句不需要写break，但是当需要继续执行下一个case语句的时候可以在case里面写fallthrough



let point = (0,0)



switch point {



case (0,0):



 print("it is an origin")



 fallthrough



case (0,_):



 print("it is on y")



 fallthrough



case (_,0):



 print("it is on x")



 //fallthrough



case let (x,y) where x == y:



 print("it is on line x==y")



 fallthrough



default:



 print("it is an ordinary point")



}





```

 ### 控制转移：break、fallthrough、continue

～～～

switch语句的case使用了变量是无法使用fallthrough。



let coordinate = (2,2)



switch coordinate {



case (0,0):



 print("origin point")



 fallthrough



case (_,let y):



 print("not an origin point")



default:



 print("wrong")



}







～～～






