### Assert 和 Precondition

```
//错误处理

//1、断言。assert和assertionFailure只在测试阶段起作用，Release时不起作用
assert(1>0)
//assert(1>2)

assert(1>0, "Error")

//assertionFailure("Error. Stop")


//2、precondition：作用和assert一样。区别：precondition在上线之后还是起作用
precondition(1>0)
//precondition(1>2, "Error")

fatalError("Error")     //fatalError程序执行到这里肯定奔溃
```

### 错误处理机制



