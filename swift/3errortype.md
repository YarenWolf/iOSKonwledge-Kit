### 错误处理

```
定义错误用枚举，实现Error协议。
在函数中抛出异常用throw，这样在函数名 返回值 前面加throws
func display(par:String) throws ->String{

}

在调用声明异常的函数时需要用try。 try! xx.funcName()
捕获到异常用catch，后可加错误类型。

 defer作用类似finally。
 一个函数类可写多个defer，defer执行在return之后，多个defer存在时执行顺序为倒序。
```

    import UIKit

    //错误处理
    class VendingMachine{
        struct Item {
            enum `Type`:String {
                case Water
                case Cola
                case Juice
            }
            let type: Type
            let price: Int
            var count: Int
        }
        /**
         * Swift2中的写法，在Swift3中ErrorType协议变成Error
        enum Error:ErrorType {
            case <#case#>
        }
        */
        enum ExceptionError:Error,CustomStringConvertible{
            case NoSuchItem
            case NotEnoughMoney(Int) //关联值
            case OutOfStock

            var description: String{
                switch self {
                case .NoSuchItem:
                    return "No Such Item."
                case .NotEnoughMoney(let price):
                    return "Not Enough Money." + String(price) + " Yuan needed."
                case .OutOfStock:
                    return "Out Of Stock"
              }
           }
        }

        public var items = ["Mineral Water":Item(type:.Water,price:2,count:10),
                            "Coca Cola":Item(type: .Cola, price: 3, count: 5),
                            "Orange Juice":Item(type:.Juice, price: 5, count: 3)]

        func vend(itemName : String, money:Int) throws ->Int?{
            //defer 控制转移
            //defer 写在函数里面
            //常用来释放一些资源
            //defer 执行在Return以后
            //如果有多个Defer，Defer的执行顺序会倒序执行。

            defer {
                print("Have a nice day!")
            }

            guard let item = items[itemName] else {
                throw VendingMachine.ExceptionError.NoSuchItem
            }

            guard money >= item.price else {
                throw VendingMachine.ExceptionError.NotEnoughMoney(item.price)
            }

            guard item.count > 0 else {
    //            throw VendingMachine.ExceptionError.OutOfStock
                throw ExceptionError.OutOfStock
                //throw后面不需要写代码，因为已经抛出异常了
            }

            defer {
                print("Thank you")
            }

            dispenseItem(itemName: itemName)
            return money - item.price
        }

        private func dispenseItem(itemName:String){
            var item = items[itemName]
            item?.count -= 1
            print("Enjoy your ",itemName)
        }

        func display(){
            print("Want something to drink?")
            for itemName in items.keys {
                print("*",itemName)
            }
            print("==================================")
        }
    }

    let machine = VendingMachine()
    machine.display()

    var pocketMoney = 4
    //方式1:
    //try! machine.vend(itemName: "Coca Cola", money: pocketMoney)


    //方式2
    if let leftoney = try? machine.vend(itemName: "Coca Cola", money: pocketMoney){
        //
    }else{
        //error handling
    }


    //方式3
    do{
        pocketMoney = try machine.vend(itemName: "Coca Cola", money: pocketMoney)!
        print(pocketMoney,"Yuan left")
    }catch VendingMachine.ExceptionError.NoSuchItem{
        print("No Such Item.")
    }catch VendingMachine.ExceptionError.NotEnoughMoney(let price){
        print("Not Enough Money.",price,"Yuan needed")
    }catch VendingMachine.ExceptionError.OutOfStock{
        print("Out Of Stock")
    }catch{
        print("Error occured duting vending.")
    }


    //方式4
    do{
        pocketMoney = try machine.vend(itemName: "Coca Cola", money: pocketMoney)!
        print(pocketMoney,"Yuan left")
    }catch let error as VendingMachine.ExceptionError{
        print(error)    //对Error进行处理，
    }

    //方式5 finally <-> defer



