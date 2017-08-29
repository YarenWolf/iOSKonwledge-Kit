#  NSClassFromString



有时候我们需要初始化一个类，但是这个类不一定存在，这样的话如果直接用 \[\[TestController alloc\] init\]的话会崩溃掉，但是用

\[NSClassFromString\("TestController"\) new\] 的话如果存在就返回一个对象，如果不存在的话则返回一个nil对象，这样子程序起码不崩溃



