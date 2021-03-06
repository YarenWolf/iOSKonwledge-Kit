# PCH文件的作用

* pch使用后可以在整个项目的工程中使用这个文件中的信息

* pch文件命名规范：采用和项目同名的原则

* pch使用条件：

  * 先新建pch文件

  * 再将pch文件路径告诉工程。选中target，选择Build Settings选项，找到LLVM下的Prefix Header，将pch路径写进去（此时不需要写全路径，因为xcode默认会找到项目的路径。所以只需要写入pch文件在工程文件中的路径即可）

* pch文件可以做些什么？

  * 存放一些公用的宏

  ```
  #define LBPConstant  @"LBPConstant"
  ```

  * 存放一些公用的头文件

  ```
  #import "Person.h"
  ```

  * 自定义Log\(让Log在debug阶段运行，正式发布后不要打印log\)
  * DEBUG是系统的宏，编译阶段有效
  * ...：在宏参数中表示可变参数
  * \_\_VA\__ARGS_\_\_:代表NSLog可变参数

  ```
  #ifdef DEBUG

  #define LBPLog(...) NSLog(__VA_ARGS__)

  #else 

  #define LBPLog(...)

  #endif
  ```



