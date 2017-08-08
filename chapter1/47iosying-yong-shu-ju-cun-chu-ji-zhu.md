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

### 1、XML属性列表归档

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

取数据:同样道理，什么数据结构存，什么数据结构取

![](/assets/屏幕快照 2017-08-06 下午11.13.49.png)

##### 注意：plist存储数据，不能使用自定义对象

### 2、Preference（偏好设置）

存数据

```
 NSUserDefaults *userDefault = [NSUserDefaults standardUserDefaults];

    [userDefault setObject:@"lbp" forKey:@"name"];
    [userDefault setInteger:22 forKey:@"age"];
}
```

取数据：先得到NSUserDefaults对象，再取数据（什么数据类型存就用什么数据类型取）![](/assets/屏幕快照 2017-08-06 下午11.14.01.png)

优点：1、可以快速进行键值对存储；2、不关心文件名

### 3、NSKeyedArchiver（归档）

* 存数据

  * 找出存储的路径
  * 存储

  ```
      NSString *url = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)[0];
      [url stringByAppendingPathComponent:@"person.data"];
      Person *p1 = [[Person alloc] init];
      p1.name = @"杭城小刘";
      p1.age = @"22";

      [NSKeyedArchiver archiveRootObject:p1 toFile:url];
  ```

![](/assets/QQ20170808-171610@2x.png)

注意：如果我们要将对象归档存储，那么需要将对象遵循NSCoding协议，实现encodeWithCoder方法

```
    NSString *url = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)[0];
    NSString *filePath = [url stringByAppendingPathComponent:@"person.data"];
    Person *p1 = [[Person alloc] init];
    p1.name = @"杭城小刘";
    p1.age = @"22";

    [NSKeyedArchiver archiveRootObject:p1 toFile:filePath];

//Person.m
//什么时候调用：把一个自定义对象归档的时候调用(保持拓展性)
//作用：告诉系统该对象的哪些属性需要归档
-(void)encodeWithCoder:(NSCoder *)aCoder{
    [aCoder encodeObject:_name forKey:@"name"];
    [aCoder encodeObject:_age forKey:@"age"];
}
```

![](/assets/屏幕快照 2017-08-09 上午12.31.06.png)

* 取数据
* \`\`\`
   NSString \*url = NSSearchPathForDirectoriesInDomains\(NSCachesDirectory, NSUserDomainMask, YES\)\[0\];
  ```
  NSString *filePath = [url stringByAppendingPathComponent:@"person.data"];
  ```

```
  Person *p = [NSKeyedUnarchiver unarchiveObjectWithFile:filePath];
  NSLog(@"%@",p);
```

```
会报错，所以如果需要解档一个对象，那么这个对象必须遵循NSCoding协议，实现-\(instancetype\)initWithCoder:\(NSCoder\*\)aDecoder方法。![](/assets/屏幕快照 2017-08-09 上午12.33.22.png)
```

-\(instancetype\)initWithCoder:\(NSCoder \*\)aDecoder{  
    if \(self = \[super init\]\) {  
        \_name = \[aDecoder decodeObjectForKey:@"name"\];  
        \_age = \[aDecoder decodeObjectForKey:@"age"\];  
    }  
    return self;  
}  
\`\`\`

![](/assets/屏幕快照 2017-08-09 上午12.38.53.png)





注意：解档和归档的key必须和对象的属性名称一致，这样存值和取值才保证正确。



