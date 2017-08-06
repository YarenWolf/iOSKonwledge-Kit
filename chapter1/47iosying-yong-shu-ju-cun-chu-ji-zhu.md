# iOS应用数据存储技术

> * XML属性列表（plist）归档
> * Preference（偏好设置）
> * NSKeyedArchiver归档（NSCoding\)
> * SQLite3
> * Core Data

#### 

#### 

#### 

#### 应用沙盒包括4部分

* 应用程序包:MainBundle。不是文件夹，而是一个压缩包，包含所有的资源文件和可执行文件

* Documents：保存应用运行时生成的需要持久化的数据。

* Library:Preferences ：保存应用的偏好设置，itunes会备份；Caches：保存应用运行时生成的需要持久化的数据，一般存储体积较大，不需要备份的非重要数据

* Tmp：临时数据，系统会清楚该目录下的文件





例子

存数据

```
    //1、应用沙盒
    NSString *home = NSHomeDirectory();
    //2、查找Caches文件夹路径
    NSFileManager *mager = [NSFileManager defaultManager];
    NSArray *array =  [mager URLsForDirectory:NSCachesDirectory inDomains:NSUserDomainMask];
    NSLog(@"%@",home);
    NSLog(@"%@",array);
    
    //2、方法2:系统方法查找Caches文件夹
    NSString *cachesPath = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)[0];
    
    NSArray *datas = @[@"122",@"happy",@(2)];
    //3、拼接路径
    NSString *plistPath = [cachesPath stringByAppendingPathComponent:@"datas.plist"];
    //4、写入沙盒中
    [datas writeToFile:plistPath atomically:YES];
```

取数据

同样道理，什么数据结构存，什么数据结构取



##### 注意：plist存储数据，不能使用自定义对象





